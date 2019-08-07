> Estimated reading time: 4 minutes

### What is Spike in Scrum?

> #### A spike is an investment to make the story estimable or schedule-able.

Spikes are an invention of Extreme Programming (XP), are a special type of user story that is used to gain the knowledge necessary to reduce the risk of a technical approach, better understand a requirement, or increase the reliability of a story estimate. A spike has a maximum time-box size as the sprint it is contained in it. At the end of a sprint, the spike will be determined that is done or not-done just like any other ordinary user story. A Spike is a great way to mitigate risks early and allows the team ascertain feedback and develop an understanding on an upcoming PBI’s complexity.

### When To Use Spike?

Sometime the team unsure if they can complete the story due to some potential blockers and probably can’t even estimate the story. Thus, you may consider a spike as an investment for a Product Owner to figure out what needs to be built and how the team is going to build it. The Product Owner allocates a little bit of the team’s capacity now, ahead of when the story needs to be delivered, so that when the story comes into the sprint, the team knows what to do.

Here are the examples of when Spikes may be used:

- The team may not have knowledge of a new technology, and spikes may be used for basic research to ensure the feasibility of the new technology (domain or new approach).
- A story requires to be implemented using a 3rd party library with API that is poorly written and documented.
- The story may contain significant technical risk, and the team may have to do some experiments or prototypes to gain confidence in a technological approach that may allow them to commit the user story to some future timebox.

### Technical Spikes and Functional Spikes

Spikes primarily come in two forms: technical and functional. A distinction can be made between technical spikes and functional spikes:

#### Technical Spike

The technical spike is used more often for evaluating the impact new technology has on the current implementation that the team needs experiment a new technology to gain more confident for a desired approach before committing new functionality to a timebox.

i.e. “how long it takes to update a customer display to current usage, determining communication requirements, bandwidth, and whether to push or pull the data”

#### Functional Spike

A functional spike are used whenever there is significant uncertainty as to how a user might interact with the system. Functional spikes are often best evaluated through some level of prototyping, whether it be user interface mockups, wireframes, page flows, or whatever techniques is best suited to get feedback from the customer or stakeholders.,

e. “Prototype a histogram in the web portal and get some user feedback on presentation size, style, and charting”

Spikes should be estimated as in-Sprint tasks during Sprint Planning. The task’s duration should be spent researching and developing some ‘thing’ that can be delivered. That thing can be a working piece of software, workflow, documentation, etc. Ultimately the value from the spike is a direction or re-direction in the course of the feature. If the team estimated that a Spike takes four hours, then ONLY four hours should be spent researching or developing. Prototypes, Proof of Concepts (PoC), and Wireframes all fall into the classification of Spikes.

### Acceptance Criteria for Spike Stories

Just like any other ordinary user story, they need fulfil some certain criteria to obtain the status of done by making sure that the “Spike Story” estimable, demonstrable, and acceptable:

##### Estimable

Like other stories, spikes are put in the backlog, estimable and sized to fit in an iteration. Spike results are different from a story, as they generally produce information, rather than working code. A spike may result in a decision, a prototype, storyboard, proof of concept, or some other partial solution to help drive the final results.

In any case, the spike should develop just the information sufficient to resolve the uncertainty in being able to identify and size the stories hidden beneath the spike.

##### Demonstrable

The output of a spike is demonstrable to the team. This brings visibility to the research and architectural efforts and also helps build collective ownership and shared responsibility for the key decisions that are being taken.

##### Acceptable

And like any other story, spikes are accepted by the product owner when the acceptance criteria for the spike have been fulfilled.

### How to Enter a Spike?

If the spike only takes, say, an hour or two, maybe we don’t need to track it in our ALM tool. If it’s larger than that, you should enter it into your backlog.

How would a BA or Product Owner write up a spike? The cocky answer is to say “make your tech lead enter it”. A more serious answer would be that the technical story format would be good:

```
In order to <achieve some goal> <a system or persona> needs to <some action>.
```

**Example:** In order to split the collection curve wizard story, the tech lead needs to do some detailed design. (Just enough to split the story.)

**Example:** In order to estimate the “user connection report” story, the tech lead and BA need to research capabilities of MSTR and have some conversations with the PO. (Just enough to estimate the story.)

Nice intention to start the name or title with “SPIKE:” to make them stand out in the application lifecycle management tool. Using a tag or a label could work as well.
