Frequently ask questions
========================


#### 1. Where do I even start?

The first step would be to get an overview of what [this repository](./README.md#table-of-contents) and the
[external resources](./README.md#external-resources) contain. As a second step, you might want to grab a coffee
and click your way through the [lists of link collections on the topic of *DevOps*](./links.md#devops). Most of
them not only provide a comprehensive list of technologies but refer to books, blogs and other 
knowledgeable resources, as well. Then, as a next step, you could try to get
[the application](https://github.com/lucendio/lecture-devops-app), that is going to be deployed as part of the exercise
implementation, to run on your local machine.

You might also find [this roadmap](https://roadmap.sh/devops) useful to you. It shows you a possible learning path. 
Towards the end, you may postpone the monitoring part until the end and instead have a look into cloud-related things
first.


#### 2. I have no clue about DevOps. How am I supposed to write this concept?

Don't worry. It's an initial step to get you start digging into things related to DevOps. After handing in the
concept you will receive individual feedback, that you can use to iterate over your design and implementation. So,
nothing is set in store. You can change things along the way - after some consultation first.


#### 3. Where do I get some infrastructure to play with or maybe even for the exercise?

Have a look at the link section on [*Infrastructure resources*](./links.md#infrastructure-resources).


#### 4. How do I obtain [AWS CLI credentials](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html) or enter the AWS web console?

You should have received a personal sign-up link for AWS Educate from your lecturer. Use that link to create an account
on *www.awseducate.com*. In case you already have a normal AWS account and you wish to use it - it's possible to
*'connect'* them during the registration process. 
 
To obtain your AWS CLI credentials, you have to log in to
[AWS Educate](https://www.awseducate.com/signin/SiteLogin). Afterwards, you should be forwarded to a 
[landing page](https://www.awseducate.com/educator/s/). In the top right menu navigate to *AWS Account* or directly
follow [this link](https://www.awseducate.com/educator/s/awssite). You will then see a big orange button labeled with
*AWS Educate Starter Account*. Click this button and it will open a new window provided by *labs.vocareum.com*. On the
right side, you can enter the *AWS console* or see your *Account Details*. The latter opens an overlay window, in which
you can unveil your CI credentials by clicking on *Show*.

Follow the [official documentation](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html) in order
to configure the CLI accordingly. 


#### 5. How do I get access to AWS and unlock [AWS Educate](https://aws.amazon.com/education/awseducate/) credits?

1. add your mail address to the provided list
2. when receiving the invitation email (subject: *Your AWS Educate Application*), follow the link to complete the AWS
   Educate application process
3. receive an approval notification via email and log in to the [AWS Educate platform](https://www.awseducate.com/signin/SiteLogin)
4. once you logged the first time, choose *AWS Educate Starter Account* and click *Get Started* --> click button to
   create the account
5. to obtain your AWS credentials, go to *AWS Account* (top right) --> *AWS Educate Starter Account* button --> 
   *Account Details* button --> AWS CLI: *Show* button. As an alternative, you may want to use the 
   [`refresh-aws-session.sh` script](https://github.com/lucendio/lecture-devops-code/blob/master/hack/refresh-aws-session.sh)
   and run it every 3 hours since a session only lasts so long.


#### 6. Which AWS services are supported with the AWS Educate program?

Please refer to [this document](https://s3.amazonaws.com/awseducate-starter-account-services/AWS_Educate_Starter_Accounts_and_AWS_Services.pdf)
for details about available services. Furthermore, please be aware that all services are located in the `us-east-1`
region, hence hosted outside of european jurisdiction (GDPR etc.).
