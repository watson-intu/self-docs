#Frequently Asked Questions: Gateway

###Here is a collection of FAQs specific to the Intu gateway

* What is the self_dialog in the services used for, as we seem to never edit it?
  * The self_dialog key/value pair, which you see in the ConversationV1 service, is the workspace id that points to your base Conversation workspace.  It is generally the key for the conversation workspace that you would download from the Intu Starter Kit.

* Credentials: M_BearerToken expires after a period, how long is this period, and what was the thought behind this as we would have to manually update all Intu users every time this expires?
  * When Intu is installed, it is given a temporary bearer token. This temporary bearer token is used to access the gateway, where the gateway will provide a permanent bearer token. The permanent bearer token is then used which will not expire. 

* What is the parent in the gateway? And what would one use it for?
  * The "parent" is created when the user creates a new organization in the gateway. There are multiple reasons for why an Intu instance has a parent, but the two main reasons are: 1) To monitor the embodiment when not directly connected to the same network, and 2) for skill sharing. The first point is very important if you require to know how the embodiment is performing in a remote location. The second reason is how we crowd source teaching. One of the fundamental principals of Intu is to learn through interactions. When one embodiment learns something, it will transfer the knowledge to it's parent. That way, if another Intu embodiment is asked to perform some task and it hasn't heard it before, it will ask it's parent and then bring down that knowledge.
  
* What services are required to run Intu?
  * Sign into the Intu Gateway. Select Manage > Services. Select your organization and group. Verify that you configured the following services with these specific names in the "Service Name" field:
   
   * SPEECH TO TEXT:
   * Service Name: SpeechToTextV1
   
   * TEXT TO SPEECH:
   * Service Name: TextToSpeechV1
   
   * CONVERSATION
   * Service Name: ConversationV1
   
   * These are the only three required services.  All other Bluemix services are not required but may add additional usability to your Intu instance.  For example, if you have Weather Company Data configured, you can ask Intu weather related questions.
   * Additionally for Conversation service, you will need to click the "+" button to add your Conversation workspace(s). For these key/value pairs, the key named "self_domain" is your own Conversation workspace. The key named "self_dialog" should have the workspace id created when you imported our sample Conversation workspace from the starter kit. (For more instructions on creating a workspace from our starter kit, see https://github.com/watson-intu/self-sdk/blob/develop/docs/workshops-devcon/2/lab-docs/workshop_2.md),













[Back to the index](../../README.md)
