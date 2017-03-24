#Frequently Asked Questions: Agents

###Here is a collection of FAQs specific to the agent mechanism in Intu

* When the output of an agent is of interest to other agents, does it have to first publish the results to the blackboard, with the rest of the agents then listening to grab the input, or does the communication happen beneath the blackboard layer?
  * Currently, yes agents must subscribe to the **Type** of objects they are interested in receiving. Additionally, they may subscribe to different levels of events for those objects.

* What is the specification for defining an agent?
  * Agents in C++ must be derived from the IAgent interface class. Typically, an agent will subscribe to the blackboard for objects they are interested in, and publish to the blackboard new types of objects based on those inputs.

* Where do the agents sit? Locally? In the cloud? Or is it some hybrid of the two?
  * In both places. An agent can be implemented natively in C++ and loaded by a shared library when Intu starts. Additionally, the user may use one of our many other language SDK's to implement an agent in Java, Javascript, Python, or C# currently.

* How does one access the agents? They do not appear to be visible when launching the avatar or the manager.
  * An agent is just an instance of an object, they have no visible appearance.

* When a goal agent determines the plan, are other agents also working in parallel? Or do they wait until the goal agent determines the plan of action, and then carry out the action required to achieve the outcome?
  * Plans typically put new objects on the blackboard for other agents to act upon.

* When you start Intu, do all of the agents start up? Or only the ones subscribed to the topic of interest?
  * All agents are started. They typically only act when an object they subscribed too is placed on the blackboard.

* How do you define autonomous vs reactive behaviour of agents? How do they work?
  * All agents are the same. 

* When an agent is added locally, the code to create it is added locally in Intu config file. If an agent is added/configured in the cloud, can the code then reside in the cloud? Any changes can then be pushed to the user's machine. Surely it is not practical to update the config locally on every single machine that has Intu.
  * Agents running in the cloud do not need to be configured locally for each Intu instance. The agent connects into the Intu instance from the cloud and just subscribes to the blackboard objects they are interested in.

* How would we have local agents talk to agents on the cloud?
  * All communication is done via the Blackboard, your agent would simply post an object that the cloud agent is subscribed too.

* How does one install custom agents?
  * For local C++ agents, you just build a shared library and add the shared library to the m_Libs array in the body.json.

* Every time we add an agent(behavior), do we have to rebuild Intu? Are there any other ways of adding agents? (Adding to the config file)
  * For local C++ agents you can simply recompile the shared library containing the agent. Agents that use the other language SDK's do not need to be configured, they can simply register when they run.

* What is the difference between intent and agent, and how do they work together?
  * The Intent is just another **Type** of object that can be placed on the Blackboard. An agent is not a blackboard objects, but an instance of a class derived from the IAgent class.

[Back to the index](../../README.md)
