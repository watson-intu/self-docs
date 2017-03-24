#Frequently Asked Questions: Architecture

###Here is a collection of FAQs specific to the Intu architecture

* Is there an orchestration layer in the backend architecture of Intu? If so, what is it?
  * Sometimes some services we communicate with can act as orchestration layers. However, usually we handle orchestration within an agent inside Intu.

* For a given topic, can the blackboard handle multiple outputs from a single/multiple agents and render them to the front end?
  * Yes, anyone can publish to a given topic and anyone can subscribe to that topic. 

* What exactly is a "parent" vs. a "child"?
  * Any Intu instance can have a parent Intu instance. This forms a heirarchy of children and parents. You can communicate with any instance through a single connection to any instance in the heirarchy.

* When a question is asked, a goal object is posted onto the blackboard for the goal agent to then determine the plan of action. How is the goal object generated, and by whom and where?
  * A agent creates the Goal object, the GoalAgent then selects a plan which executes one or more actions to complete the goal.

* What is a goal object? Is there only one or multiple per question?
  * Typically there is only one, but that's not a requirement since a Goal may execute a plan that may create additional Goals.

* How does the avatar communicate with the Intu console? Is it using the blackboard somehow? Or is it through a host IP? Or both?
  * The Avatar uses the web socket inteface of Intu to communicate. This is how all of our non-native SDK's communicate with an Intu instance.

* According to the Self architecture document, the different subsystems (sensors, models, actuators, agency) have their own blackboard. Are these blackboard separated? If yes, do their agents talk to one another in the same environment?
  * They are seperated into seperate sub-trees within a single blackboard instance.

* What is the relationship and flow between a topic, intent and agents?
  * There is no direct flow between a topic and intent/agents. Various systems in Intu will publish and subscribe to topics; this is simply done to allow communications through the topic system. An intent is a type of IThing, which is posted to the blackboard. An agent can subscribe to the blackboard for any IThing type, including intents and other types of objects.

* Why do we have to edit the body JSON file manually? How would we go about editing 100+ instances of this across users? And/or add agents at a later stage?
  * The body.json is eventually going away in future releases to be replaced by a cloud based graph.

* How will we go about enabling multiple users, as each user will not have a Bluemix account?
  * We are working to allow a single Intu instance in the cloud to handle a large number of users at the same time. 

[Back to the index](../../README.md)
