#Frequently Asked Questions: Services

###Here is a collection of FAQs specific to the Intu services

* What is a topic? Is there one topic in the blackboard or can there be multiple topics?
  * A topic is a channel that a user has the ability to subscribe and publish to. For example, if the end user is interested in information that is being published onto the blackboard, we can subscribe to the 'blackboard' topic and see all information. Additionally, we can subscribe to specific events that are occuring on the blackbaord - for example, all text objects. Over the TopicManager, we can also publish information over topics. This publish/subscription logic is a fundamental behavior of all agents.

* When a question comes into the blackboard, how does it get classified into the topic of interest? Does the question flow through to all topics in the blackboard, from where the agents subscribed to the corresponding topics respond? OR does a/couple of agents interprets the question first, and then sends it to the corresponding topic bucket related to the question for other follow on agents?
  * A "question" doesn't necessarily get published to the blackboard, but a "Text" object does. The Text object contains the transcription of what the user is saying. This is ultimately picked up by the TextClassifier which utilizes Watson services, such as Natural Language Classifier and/or Conversation. When an intent from these services is returned with a high enough confidence, Intu will place an "Intent" object on the blackboard. Depending on what that intent is, an agent will pick it up and do some small peice of logic.

[Back to the index](../../README.md)
