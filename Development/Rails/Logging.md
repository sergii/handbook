### The basics

### Getting Started

### Best practices

### Error Handling

### Logging

### Testing

```ruby
require 'rails_helper'

class Result
  def self.log(message)
    Rails.logger.info(message)
  end
end

RSpec.describe Result do
  describe '.log' do
    it 'logs message into Rails logs' do
      allow(Rails.logger).to receive(:info)

      described_class.log('Hello World')

      expect(Rails.logger).to have_received(:info).with('Hello World')
    end
  end
end
```
