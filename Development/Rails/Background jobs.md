### The basics

Background jobs can serve for everything purposes like from regularly scheduled clean-ups, to billing charges, to mailings. Anything that can be chopped up into small units of work and run in parallel, really.

One of the main serves of background jobs is the as the backend for Action Mailer's #deliver_later functionality that makes it easy to turn any mailing into a job for running later. That's one of the most common jobs in a modern web application: sending emails outside of the request-response cycle, so the user doesn't have to wait on it.

### Getting Started

Declare a job like so:

```ruby
class MyJob < ActiveJob::Base
  # Set the Queue as Default
  queue_as :default

  def perform(*args)
    # Perform Job
  end
end
```
* NOTE: The purpose of nesting case for the job as middleware `ApplicationJob < ActiveJob::Base` need to be discussed ^^
* NOTE: What we should use as an example of `perform` method arguments: `record` or `args*`?

```ruby
class MyJob < ActiveJob::Base
  # Set the Queue as Default
  queue_as :default

  def perform(record)
    record.do_work
  end
end
```

### OR

```ruby
class MyJob < ActiveJob::Base
  # Set the Queue as Default
  queue_as :default

  def perform(*args)
    args.....
  end
end
```

Enqueue a job like so:

```ruby
MyJob.perform_later record  # Enqueue a job to be performed as soon as the queuing system is free.
```

```ruby
MyJob.set(wait_until: Date.tomorrow.noon).perform_later(record)  # Enqueue a job to be performed tomorrow at noon.
```

```ruby
MyJob.set(wait: 1.week).perform_later(record) # Enqueue a job to be performed 1 week from now.
```


### Best practices

1. Make your job parameters small and simple

```ruby
class MyJob < ActiveJob::Base
  # Set the Queue as Default
  queue_as :default

  def perform(record)
    record.do_work
  end
end
```

2. Make your job idempotent and transactional
3. Embrace Concurrency

### Error Handling

One of the most important things is to correctly handle background job execution errors.
ActiveJob exception handling logic can help us in that work.
https://api.rubyonrails.org/classes/ActiveJob/Exceptions/ClassMethods.html

**discard_on**(exception)

Discard the job with no attempts to retry, if the exception is raised. This is useful when the subject of the job, like an Active Record, is no longer available, and the job is thus no longer relevant.

You can also pass a block that'll be invoked. This block is yielded with the job instance as the first and the error instance as the second parameter.

```ruby
class SearchIndexingJob < ActiveJob::Base
  discard_on ActiveJob::DeserializationError
  discard_on(CustomAppException) do |job, error|
    ExceptionNotifier.caught(error)
  end

  def perform(record)
    # Will raise ActiveJob::DeserializationError if the record can't be deserialized
    # Might raise CustomAppException for something domain specific
  end
end
```

**retry_on**(exception, wait: 3.seconds, attempts: 5, queue: nil, priority: nil)`

Catch the exception and reschedule job for re-execution after so many seconds, for a specific number of attempts. If the exception keeps getting raised beyond the specified number of attempts, the exception is allowed to bubble up to the underlying queuing system, which may have its own retry mechanism or place it in a holding queue for inspection.

You can also pass a block that'll be invoked if the retry attempts fail for custom logic rather than letting the exception bubble up. This block is yielded with the job instance as the first and the error instance as the second parameter.

**Options:**

`:wait` - Re-enqueues the job with a delay specified either in seconds (default: 3 seconds), as a computing proc that the number of executions so far as an argument, or as a symbol reference of :exponentially_longer, which applies the wait algorithm of (executions ** 4) + 2 (first wait 3s, then 18s, then 83s, etc)

`:attempts` - Re-enqueues the job the specified number of times (default: 5 attempts)

`:queue` - Re-enqueues the job on a different queue

`:priority` - Re-enqueues the job with a different priority

```ruby
class RemoteServiceJob < ActiveJob::Base
  retry_on CustomAppException # defaults to 3s wait, 5 attempts
  retry_on AnotherCustomAppException, wait: ->(executions) { executions * 2 }
  retry_on(YetAnotherCustomAppException) do |job, error|
    ExceptionNotifier.caught(error)
  end
  retry_on ActiveRecord::Deadlocked, wait: 5.seconds, attempts: 3
  retry_on Net::OpenTimeout, wait: :exponentially_longer, attempts: 10

  def perform(*args)
    # Might raise CustomAppException, AnotherCustomAppException, or YetAnotherCustomAppException for something domain specific
    # Might raise ActiveRecord::Deadlocked when a local db deadlock is detected
    # Might raise Net::OpenTimeout when the remote service is down
  end
end
```

Example 1: We log the exceptions only when running the app in the `development` mode, and use `rescue_from` and `ApplicationJob` as a base job class

```ruby
class ApplicationJob < ActiveJob::Base
  discard_on ActiveJob::DeserializationError

  discard_on(CustomAppException) do |job, error|
    ExceptionNotifier.caught(error)
  end

  retry_on CustomAppException # defaults to 3s wait, 5 attempts
  retry_on AnotherCustomAppException, wait: ->(executions) { executions * 2 }

  retry_on(YetAnotherCustomAppException) do |job, error|
    ExceptionNotifier.caught(error)
  end

  retry_on ActiveRecord::Deadlocked, wait: 5.seconds, attempts: 3
  retry_on Net::OpenTimeout, wait: :exponentially_longer, attempts: 10
  
  protected
  
  def handle_error_in_development(e)
    logger.error(e.message)
    logger.error(e.backtrace.join("\n"))

    Honeybadger.notify(e, context: { job_arguments: arguments })
  end
end

class RemoteServiceJob < ApplicationJob
  def perform(*args)
    # Might raise CustomAppException, AnotherCustomAppException, or YetAnotherCustomAppException for something domain specific
    # Might raise ActiveRecord::Deadlocked when a local db deadlock is detected
    # Might raise Net::OpenTimeout when the remote service is down
  rescue => e
    raise(e) unless Rails.env.development?
    handle_error_in_development(e)
  end
end
```
### Logging

### Testing
