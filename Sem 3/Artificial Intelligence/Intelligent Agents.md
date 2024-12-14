
- **Agent:**
    - Anything that can perceive its surroundings (environment) using sensors.
    - Takes actions in the environment using actuators.
- **Examples:**
    - Humans
    - Cleaning robots
    - Software programs (software agents)

• Agent Function: Brains of an AI agent

- Maps what it senses (percept sequence) to what it does (action)
- Ideally, considers all past experiences (percept sequence) for best action 
- Agent Program: Puts the function into action
- Performance Measure: How well it works - Tracks how effective the agent function is at achieving goals


#### Performance Measures

- **Consequentialist Approach:** We focus on the **outcomes** of the agent's actions.
- **Measuring Success:** The performance measure determines how well the agent achieves its goals.
- **Example: Cleaning Robot** Imagine a robot programmed to clean a floor.
    - A good performance measure would reward clean squares (+ points) and penalize wasted energy or noise (- points).
    - By maximizing its points, the robot is incentivized to clean efficiently.
- **Focus on Goals:** The design of the performance measure should reflect what we want the agent to achieve in its environment, not how we think it should behave.

#### Omniscience vs. Rationality and Autonomy

- **Omniscient Agent:** A mythical being with perfect knowledge of all outcomes. (Think fortune teller who's always right!)
- **Rational Agent:** Makes the best decisions based on **expected** outcomes, not perfect knowledge. Relies on:
    - **Percept Sequence:** All its past experiences (what it's sensed)
    - **Learning:** Adapts based on new information
    - **Gathering Information:** Actively seeks knowledge to improve decisions

**Autonomy Spectrum:**

- **More Experience (Sensors) - More autonomous:**
    - Agent learns from its own interactions with the world
    - Relies less on pre-programmed knowledge
- **More Built-in Knowledge - Less autonomous:**
    - Agent relies heavily on pre-programmed information
    - Less adaptable to new situations

#### The Task Environment

Consists of PEAS
	• Performance Measure
	• Environment
	• Actuators
	• Sensors
In designing an agent, the first step must always be to specify 
the task environment as fully as possible.

![[Pasted image 20240715133205.png]]

#### Properties of Task Environments

| **Property**            | **Description**                                                                                                                                                                               | **Example**                                         |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------- |
| **Observability**       |                                                                                                                                                                                               |                                                     |
| Fully Observable        | The agent’s sensors give access to the complete state of the environment at each point in time.                                                                                               | Chess                                               |
| Partially Observable    | Some part of the state is missing from the sensor data, often due to noisy or inaccurate sensors.                                                                                             | Vacuum agent, Automated taxi                        |
| **Agents**              |                                                                                                                                                                                               |                                                     |
| Single-Agent            | The environment contains only one agent solving a problem.                                                                                                                                    | Crossword puzzle                                    |
| Multi-Agent             | The environment contains multiple agents which can be competitive or cooperative.                                                                                                             | Chess                                               |
| Competitive Multi-Agent | One agent trying to maximize its performance measure causes another agent’s performance measure to be minimized.                                                                              | Chess                                               |
| Cooperative Multi-Agent | One agent trying to maximize its performance measure also maximizes another agent’s performance measure.                                                                                      | Taxi-driving (avoiding collisions)                  |
| **Determinism**         |                                                                                                                                                                                               |                                                     |
| Deterministic           | The next state of the environment is completely determined by the current state and the action executed by the agent(s).                                                                      |                                                     |
| Nondeterministic        | The next state of the environment is not completely determined by the current state and the action executed by the agent(s).                                                                  | Taxi driving                                        |
| Stochastic              | The environment explicitly deals with probabilities.                                                                                                                                          | Weather prediction (e.g., 25% chance of rain)       |
| **Episodes**            |                                                                                                                                                                                               |                                                     |
| Episodic                | The agent’s experience is divided into atomic episodes, and the next episode does not depend on the previous actions. Episodic actions are simpler because the agent does not THINK <br>AHEAD | Defective parts spotting on an assembly line        |
| Sequential              | The current decision could affect all future decisions.                                                                                                                                       | Chess, Taxi driving                                 |
| **Dynamics**            |                                                                                                                                                                                               |                                                     |
| Dynamic                 | The environment can change while the agent is deliberating.                                                                                                                                   | Taxi driving                                        |
| Static                  | The environment does not change while the agent is deliberating.                                                                                                                              | Crossword puzzles                                   |
| **State and Time**      |                                                                                                                                                                                               |                                                     |
| Discrete                | The state of the environment, time, percepts, and actions are all distinct and countable.                                                                                                     | Chess (fixed number of moves)                       |
| Continuous              | The state of the environment, time, percepts, and actions are all continuous and not countable.                                                                                               | Taxi driving (continuous state and continuous time) |

#### Agent Structure: Brain and Body

An AI agent is a combination of hardware and software working together. Here's a breakdown:

- **Agent Program:** The brains - it figures out what to do based on what it senses.
    - Maps perceptions (sensory inputs) to actions.
- **Agent Architecture:** The body - the physical platform that executes the program.
    - Provides sensors for perception and actuators to take actions in the world.
    - Connects the program to the real world.

**Together they form a powerful duo:**
- **Agent = Architecture + Program**

#### Agent Program Types

1.Simple Reflex Agents

• **Actions depend only on present percepts ignoring the history**
• Limited Intelligence
• Works best in an observable environment
• E.g., Vacuum agent

![[Pasted image 20240715144722.png]]

2.Model-based Reflex Agents

• Maintains an internal state that depends on the percept history.
- **Example Scenarios:**
    - **Crossing the Road:** Agent remembers looking left before checking right, avoiding collisions.
    - **Braking in Traffic:** Internal state reflects the car ahead braking, prompting the agent to brake too.

![[Pasted image 20240715145245.png]]


3.Goal-based Agents

• Combines prior knowledge and perceptions and takes actions to achieve a goal.

![[Pasted image 20240715150629.png]]


4.Utility-based Agents

• A UTILITY FUNCTION maps a state or a sequence of states 
onto a real number.
- **The Happiness Scale:** A utility function maps states or sequences of states to real numbers, essentially representing a happiness scale. Higher numbers indicate more desirable outcomes.
- **Trading Off Goals:** This approach is particularly useful when goals conflict.
    - For example, a self-driving car might have goals for speed and safety. The utility function can determine the best balance between these goals.
- **Choosing the Best Option:** When faced with multiple possible actions, the agent selects the one that leads to the state with the highest expected utility.
- **Example:** A taxi driver with various routes (actions) to reach a destination. The utility function might consider factors like travel time, traffic congestion, and fuel efficiency to choose the most desirable route.

![[Pasted image 20240715151528.png]]


Learning Agents

Learning agents are a sophisticated breed that continuously improve their performance through experience.

![[Pasted image 20240715153229.png]]


#### How the Components of Agent Programs Work?

![[Pasted image 20240715153355.png]]


**Atomic representation**
• A state is a black box with no internal structure
• Algorithms
	• Standard searching algorithms
	• Hidden Markov Models

**Factored representation**
• A state consists of a set of variables or attributes, each of which can 
have a value.
• Two different factored states can share some attributes (such as being 
at some GPS location) and not others (such as having lots of gas or 
having no gas)
• Makes it much easier to work out how to turn one state into another.

**Structured representation**
• A state includes objects, each of which may have attributes of its own 
as well as relationships to other objects.
• Algorithms
	• First-order logic
	• First-order probability models


[[Solving Problems by  Searching]]