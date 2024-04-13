# Alfalo
Alfalo is an **A**ny**L**ogic Library for **Fa**st **L**ay**o**ut Generation. It provides a easy way to generate complex layout programmatically in AnyLogic Models. 

**Use case:** 

There are cases where users may want to generate layout networks in AnyLogic programmatically with input from some databases, instead of drawing directly in AnyLogic software. For example, when the end user receives a stand-alone java application version of the simulation from the model builder, the end user do not have access to the wonderful AnyLogic space markup APIs, then to generate layout networks from databases enables the end user to test different layout networks. *The Alfalo library simplifies the layout network generation from database process, making it fast, easy and intuitive.*

## Main Features

There are two main features: `Rectangular Node/Node Group Manipulation` and `Fast Path Generation`.

**Rectangular Node/Node Group Manipulation**:

Manipulation of rectangular nodes and node groups means applying flipping and rotation on defined prototypes around their origin. In lots of complex layouts, there are rectangular nodes and node groups that are the same but are just manipulated in some way. For example, in some manufacturing plants, stations (rectangular node) and centers (node group) that are essentially the same are replicated across the plant. The Alfalo library enables the users to define prototypes for rectangular nodes and node groups, so that new rectangular nodes and node groups can be easily generated by specifying the position and the manipulation of the corresponding prototype, as shown in Figure 1 and 2. **Note: When manipulating rectangular nodes and node groups, flipping operations will by default happen before rotation operations.**

**In some cases where instead of rectangular node/node group manipulation, user prefers to use AnyLogic's node constructor to generate rectangular nodes, the Alfalo library would also support it with no problem. The user can use *putRectangularNode()* method to add the generated rectangular node for final compilation.**

<img src="https://github.com/m1ng2e/Alfalo/assets/62451645/dc666560-db1d-44e9-8c20-6581a0ce94be" width="600" height="250">

*Figure 1: Rectangular Node Manipulation*

<img src="https://github.com/m1ng2e/Alfalo/assets/62451645/f3e631c6-2a5b-40b6-b0f4-ac8cd2f60e67" width="600" height="250">

*Figure 2: Node Group Manipulation*

**Fast Path Generation**:

Rather than defining paths one by one and specifying source and target for each path, like how it is normally done in AnyLogic, the Alfalo library enables easy paths generation by requiring less parameters from the users by using Alfalo paths. When an Alfalo path is defined to cross one or more rectangular nodes, multiple paths connecting the rectangular nodes will be automatically generated, as shown in Figure 3. When two Alfalo paths are defined to cross each other, four paths connected by a point node will be automatically generated, as shown in Figure 4. The library will automatically figure out the source and target of each generated path.

<img src="https://github.com/m1ng2e/Alfalo/assets/62451645/4c7f804d-045d-40be-b33e-04c63face41e" width="700" height="230">

*Figure 3: Alfalo Path Crossing Rectangular Nodes*

<img src="https://github.com/m1ng2e/Alfalo/assets/62451645/895da81c-bd45-4a7a-b84d-30ea51efe576" width="600" height="250">

*Figure 4: Alfalo Paths Crossing Each Other*

## Memory Feature

The memory feature saves the layout generated to a text file, so that the same layout does not need to be recalculated for each experiment instance. This is especially beneficial when doing replicated experiments. To activate the memory feature, use *setStateFile(java.lang.String stateFileName)*.

## Getting Started

These instructions will get Alfalo integrated with AnyLogic. 

### Prerequisite

You need to have AnyLogic with any valid license (PLE, University, or Professional) installed on your machine.

### Installation

1. Download the Alfalo.jar file (from the [releases](https://github.com/m1ng2e/Alfalo/releases)) and place it somewhere it won't be moved (or accidentally deleted).
2. Add it to your AnyLogic palette. A step-by-step explanation of how to do this is available in the AnyLogic help article ["Managing Libraries"](https://anylogic.help/advanced/libraries/managing-libraries.html#managing-libraries).
3. You should see a new palette item for Alfalo with the Alfalo Layout Generator agent.

## Example Implementation

The layout in Figure 5 is generated with the AnyLogic model [Alfalo Library Example.zip](https://github.com/m1ng2e/Alfalo/files/14968433/Alfalo.Library.Example.zip).

![Layout Example](https://github.com/m1ng2e/Alfalo/assets/62451645/d27305b2-e332-40bf-a7d2-bbaab37a2b37)

*Figure 5: Example Layout*



