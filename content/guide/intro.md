Introduction
============


### Approach and workflow

* there is no universal best practise, only the most reasonable and suitable solution for your specific setup that
  you are aiming to implement
* infrastructure code is subject to development best practises just like it is with any kind of software code
  - you may establish a development life cycle starting by writing code on your local workstation, but instead of
    locally building and running your application, you *allocate* and *configure* resources, either on your workstation
    or in some public cloud context
* tear down and recreate your infrastructure on a regular basis to
  - indicate reproducibility
  - safe some credits
* automate every step of the way right from the beginning
  - not necessarily by starting to write jobs for your automation platform, but rather by *scripting* those steps
  - this can speed up your work and may serve as a foundation for the CI/CD integration happening later down the road