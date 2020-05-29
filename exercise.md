Exercise
========


__Task:__ Deploy and run the [given application](https://github.com/lucendio/lecture-devops-app) in a cloud or
cloud-simulated environment. Allocate the underlying infrastructure. Bootstrap components that are necessary 
to deploy and host the application in multiple environments. Automate these processes in a reasonable manner.

__Deliverables:__ [concept](./deliverables/exercise_concept.md) & [implementation](./deliverables/exercise_implementation.md)


#### Process

1. hand in the concept before the given deadline sometime within the first half of the semester
2. receive short-term feedback and sanity-check via email
3. present your implementation in a review session at the end of the semester
4. check in last commit by the end of the review day

Eventually, you'll end up with two repositories:

(1) the fork of the [application](https://github.com/lucendio/lecture-devops-app) you are going to deploy for this
    exercise
(2) the source code that defines your infrastructure, its documentation, and the
    [concept](./deliverables/exercise_concept.md)

*Please note, that it's not required to successfully & completely finish the implementation in order to pass (grade: 4.0)
the module.*


#### Rules

* teams (≤ 2 people) are allowed, but not required


#### Specifications

__Components:__

* Application (+ backing service)
* Version control system
* Automation system
* Target environments (test, production)
* Monitoring

Except for the given [application](https://github.com/lucendio/lecture-devops-app) and its dependencies, you are free to
choose any available technology for the implementation, as long as the exercise requirements are being met.  

*NOTE: for more details and specifications on the deliverable(s), please refer the corresponding document(s) 
[here](./deliverables)*


#### The application

* fork the [source code](https://github.com/lucendio/lecture-devops-app)
* contribute at least one Pull-Request that adds another test (either for the frontend or the backend)
* add whatever you feel necessary to the forked source code
* if you add sth. that might be worth sharing and could be useful for others, you are more than welcome to
* `README` files are usual a good starting point to familiarize yourself with the code base


#### Review session

* it's going to be a 1:1 
* demonstrate the entire deployment process for the [given application](https://github.com/lucendio/lecture-devops-app)
  by introducing an actual change to the code base
* verify the existence of every component
