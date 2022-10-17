---
title: 'Project Concept'
weight: 2
---


Deliverable: Project Concept
============================


__SUBMISSION DEADLINE:__ before the end of day on the [predefined date]({{< ref "/dates" >}})


## Requirements

### Formal

* language: German or English
* written in Markdown (or reStructuredText)
* file(s) are stored and tracked in an accessible Git repository
* hand in a deep link to the file(s) located in that repository
* changes to the technology stack after handing in the initial version
  are only permitted after consultation


### Content

* describe the life cycle of the application (e.g. automation steps)
* outline architecture of the infrastructure and how it's going to be provisioned
* explain your decisions
* briefly describe the technology stack you opted for

Think of the audience for this concept as one of your classmates who you are trying to convince to join you in
helping with the implementation part. Or, your team lead, who you are trying to convince to let you work one something
else for the upcoming weeks. 

The following questions might give you some inspiration. *(Only to be seen as an INCOMPLETE list of suggestions, and not
as headlines)*

* Where is your infrastructure hosted?
* How do you allocate the underlying resources hosting your environments?
* How is everything provisioned?
* Why did you choose one technology over the other?
* How does CI/CD talk to the VCS and/or vice versa?
* What are the names of your target environments?
* How do you set up all your services? Which of them do you actually need to set up?
  If some of them don't need to be set up, why?
* What are the differences between your environments, if any?
* Would some graphic(s) better illustrate your architectural design?
* What about the persistence layer (database)?
