#Frequently Asked Questions: Avatar

###Here is a collection of FAQs specific to the Intu avatar

* Why does the config.json file require all the service credentials, as this could be downloaded from the gateway?
  * The config.json does not require a user to manually enter the service credentials. When the Avatar starts up, if it is configured and pointing to the gateway, then it will obtain the service credentials from the Gateway and serialize them out to the file. 

* How does the config get updated? Does one have to manually update the config file, or will it update from the server? If we need to change a service endpoint we would need to update the server and have that downloaded to all the users' avatar instances.
  * The config gets updated when the Avatar starts up and grabs the latest service credentials and serializes back out to the file. There is still a manual step at the beginning prior to starting the avatar: the user must enter the group id, organization id, and bearer token provided by the "Get Credentials" on the gateway. A permament bearer token will then be used for each continued use.

* Why do we remove the Self ID from the config when setting up the avatar? What does this refer to as it gets populated when you connect to a self instance.
  * The blank Self ID is to force the Avatar to grab a permanent bearer token from the gateway. If the Self ID is not blank, then the Avatar will attempt to use the temporary bearer token as the permament one.

* What is the dark shadow page that can be moved around and obscures interaction with the default conversation page?
  * If Alchemy service is configured, then the shadow page behind the conversation page will show the syntactic parse structure of the questions that are being asked.

* Would we have one Intu talk to another Intu? Why would we do this and how would we do this?
  * The Avatar is almost like an Intu instance that is communicating with another Intu instance. This communication occurs over the TopicClient/Manager, where we can publish and subscribe to topics of interest.

* Why do you have to update the services in Intu after updating the gateway's services? Shouldn’t Intu pull the gateway's services data?
  * You don't. Services are updated when Intu starts up and grabs the latest services from the gateway.

* How do we turn the microphone off?
  * This functionality currently is not supported via speaking. However, the camera can be turned on/off by simply saying "turn on/off the camera"

* Is there a way to just type instead of selecting the chat window?
  * Currently, the user is required to click on the chat window and start typing.

* How do we see all open documents?
  * Currently, the user is required to manually move all documents to their own slot to visualize all at once.

* How do we remove a document?
  * Currently, we do not support deleting documents.

* How do we reset all the documents?
  * Currently, we do not support reseting all documents.

[Back to the index](../../README.md)
