#Frequently Asked Questions: Manager

###Here is a collection of FAQs specific to the Intu manager

* What other language is Intu going to support apart from C++?
  * While the core of Intu is written in C++, we are working on enabling Intu through sdks in Java, Python, and Javascript

* How does Intu handle the user's question memory? E.g: If I ask a question, and a follow on question is asked, it knows that it’s merely a filter/linked to the first question for more detail.
  * For a more indepth and linked question flow, you would need to build out a detailed and complex Conversation workflow through Bluemix's Conversation Service.

* Does the Intu manager need to be installed on a physical machine locally, on someone's PC? Can it be installed on a server/VM/cloud?
  * Sample answer

* Why do we need to remove Self ID on the config file in the exercise when the Intu gateway has a default value? Is it just a dummy value which does nothing?
  * Sample answer

* What is the Self Dialog ID that is in the Intu gateway? What does it do?
  * The "self_dialog" id on the Intu Gateway is a Conversation workspace id that is associated with your Conversation workspace.  "self_dialog" is meant to be associated with the json downloaded from the Intu Starter Kit and "self_domain" is meant to be your own personal Conversation workspace id.

* Can multiple avatars on different users' machines talk to a single admin Intu gateway? Or does it have to be a 1:1 relationship, meaning that 1 Intu gateway for user x talk to the avatar of user x on the SAME machine?
  * Sample answer

* When a user runs the avatar, is it mandatory to also run the Intu manager on the SAME machine? Can a user run the avatar without having to run the Intu manager, as it can be sitting on some server which is connected to the avatar?
  * As long as an instance of Intu is running, you do not need to run the Intu Manager while running the Intu Avatar.  Once you turn an Intu instance on through the Manager, you can close it.

* Why do you have to physically turn on both your device and your avatar (SelfUnitySDK) in the Intu Manager in order to get the avatar running? Why does a second terminal window open?
  * The Avatar requires a local instance of Intu to be running in order to work.

* What is the purpose of the manager and gateway, why not just have the gateway?
  * Sample answer

* Health Services do not appear until Intu seems to be fully up and running; this doesn’t help diagnosing which services could be causing Intu to fail.
  * Sample answer

* What does a red dot mean in the user list?
  * A red dot indicates a device is offline

* What is the purpose of Self-Domain used in Services > Conversationv1
  * "self_domain" is the workspace id of your own custom conversation workspace, if you choose to use one.  "self_dialog" is the conversation workspace id for the workspace downloaded and imported from the Intu Starter Kit.

* What are groups and organizations used for?
  * Organizations can hold multiple groups.  Groups are where you register devices to and configure services for.

* What is the difference between the computer and the brain?
  * The brain icon indicates a device is a Parent instance.  A computer icon indicates a device is a laptop device, such as PC or Mac.

* What is the parent? Why do you have a parent and Intu when you start up manager?
  * The "parent" is created when the user creates a new organization in the gateway. There are multiple reasons for why an Intu instance has a parent, but the two main reasons are: 1) To monitor the embodiment when not directly connected to the same network, and 2) for skill sharing. The first point is very important if you require to know how the embodiment is performing in a remote location. The second reason is how we crowd source teaching. One of the fundamental principals of Intu is to learn through interactions. When one embodiment learns something, it will transfer the knowledge to it's parent. That way, if another Intu embodiment is asked to perform some task and it hasn't heard it before, it will ask it's parent and then bring down that knowledge

* How do you change the installed instance's credentials on a machine without reinstalling?
  * Sample answer

* Why does the Avatar have an on off button?
  * Sample answer

[Back to the index](../../README.md)
