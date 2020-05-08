Guidelines
==========

This documents gives some advice on how to approach the exercise and how to deal with certain problems. The content
might only be applicable to a certain extend depending on your approach. It is based only on personal experience and
therefore largely subjective. 

*NOTE: the content of this document is not finite but also not updated regularly*


* there is no universal best practise only the most reasonable and suitable solution for your specific problem that you
  are trying to solve 
* infrastructure code is subject to development just as it is with application code
  - you may establish a development life cycle starting where you write code on your local computer, but instead of
    building and running your application locally, you allocate and bootstrap either on your computer or just
    triggered locally  
* tear down and recreate your infrastructure on a regular basis
  - shows reproducibility
  - might safe some credits
* automate every step of the way right from the beginning
  - not necessarily by starting to write jobs for your automation tool, but rather by scripting those steps  
  - this can speed up your work and may serve as the foundation for the CI/CD integration happening later down the road
