# Alfalo
Alfalo is an **A**ny**L**ogic Library for **Fa**st **L**ay**o**ut Generation. It provides a easy way to generate complex layout programmatically in AnyLogic Models. 

**Use case:** 

There are cases where users may want to generate layout networks in AnyLogic programmatically with input from some databases, instead of drawing directly in AnyLogic software. For example, when the end user receives a stand-alone java application version of the simulation from the model builder, the end user do not have access to the wonderful AnyLogic space markup APIs, then to generate layout networks from databases enables the end user to test different layout networks. *The Alfalo library simplifies the layout network database generation process.*

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

Rather than defining paths one by one and specifying source and target for each path, like how it is normally done in AnyLogic, the Alfalo library enables easy paths generation by requiring the users to define less (Alfalo) paths. When an Alfalo path is defined to cross one or more rectangular nodes, multiple paths connecting the rectangular nodes will be automatically generated, as shown in Figure 3. When two Alfalo paths are defined to cross each other, four paths connected by a point node will be automatically generated, as shown in Figure 4. The library will automatically figure out the source and target of each generated path.

<img src="https://github.com/m1ng2e/Alfalo/assets/62451645/4c7f804d-045d-40be-b33e-04c63face41e" width="700" height="230">

*Figure 3: Alfalo Path Crossing Rectangular Nodes*

<img src="https://github.com/m1ng2e/Alfalo/assets/62451645/895da81c-bd45-4a7a-b84d-30ea51efe576" width="600" height="250">

*Figure 4: Alfalo Paths Crossing Each Other*

## Getting Started

These instructions will get Alfalo integrated with AnyLogic. 

### Prerequisite

You need to have AnyLogic with any valid license (PLE, University, or Professional) installed on your machine.

### Installation

1. Download the Alfalo.jar file (from the [releases](https://github.com/m1ng2e/Alfalo/releases)) and place it somewhere it won't be moved (or accidentally deleted).
2. Add it to your AnyLogic palette. A step-by-step explanation of how to do this is available in the AnyLogic help article ["Managing Libraries"](https://anylogic.help/advanced/libraries/managing-libraries.html#managing-libraries).
3. You should see a new palette item for Alfalo with the Alfalo Layout Generator agent.

## Example Implementation

The following code generates the layout in Figure 5. For implementation model please download the AnyLogic model file [Alfalo_Example1.zip](https://github.com/m1ng2e/Alfalo/blob/main/Alfalo_Example1.zip).

<img src="https://github.com/m1ng2e/Alfalo/assets/62451645/8ac11355-6f09-406f-83f2-8e5463397de2" width="450" height="300">

*Figure 5: Example Layout*

```
ArrayList<RectangularNode> rn_list = new ArrayList();

//Generate a rectangular node with AnyLogic node constructor
RectangularNode rn = new RectangularNode();
rn.setSize(220, 120);
rn.setPos(-110, -60, 0);
Attractor a = new Attractor(100, 20, 30);
rn.addAttractor(a);
alfaloLayoutGenerator.putRectangularNode(rn);
rn_list.add(rn);

//Define a node group with two rectangular nodes in it
alfaloLayoutGenerator.addNodeGroupPrototype("My node group", 300, 200);
alfaloLayoutGenerator.addRectangularNodePrototype("My rectangular node 1", 30, 60);
alfaloLayoutGenerator.addAttractorToRectangularNodePrototype("My rectangular node 1", 20, 40, 0);
alfaloLayoutGenerator.addRectangularNodePrototype("My rectangular node 2", 10, 30);
alfaloLayoutGenerator.addAttractorToRectangularNodePrototype("My rectangular node 2", 8, 20, 0);
alfaloLayoutGenerator.addRectangularNodeToNodeGroupPrototype("My node group", "My rectangular node 1", 200., 100., true, false, 90);
alfaloLayoutGenerator.addRectangularNodeToNodeGroupPrototype("My node group", "My rectangular node 2", 100, 100, false, false, 0);

//Manipulate and place four instances of the node group into the layout
rn_list.addAll(alfaloLayoutGenerator.putNodeGroup("My node group", 0, 0, true, true, 0));
rn_list.addAll(alfaloLayoutGenerator.putNodeGroup("My node group", 0, 0, false, false, 0));
rn_list.addAll(alfaloLayoutGenerator.putNodeGroup("My node group", 0, 0, true, false, 0));
rn_list.addAll(alfaloLayoutGenerator.putNodeGroup("My node group", 0, 0, false, true, 0));

//Manipulate and place two instances of the rectangular node into the layout
rn_list.add(alfaloLayoutGenerator.putRectangularNode("My rectangular node 1", -140, 15, true, false, 90));
rn_list.add(alfaloLayoutGenerator.putRectangularNode("My rectangular node 1", 200, -15, false, false, 90));

//Add alfaloLayoutGenerator Paths
double[][] alf_paths = new double[][] {{-170, -115, -170, 115, 170, 115, 170, -115, -170, -115},
				   {0, -115, 0, 115},
				   {-105, -100, -105, 100},
				   {105, -100, 105, 100},
				   {-170, -85, 170, -85},
				   {-170, 85, 170, 85},
				   {-170, 0, 170, 0}};
for (double[] alf_path: alf_paths){
	alfaloLayoutGenerator.addAlfaloPath(alf_path, false);
	}
	
//Generate Layout
presentation.add(alfaloLayoutGenerator.generateLayout(this, "myNetwork", "myLevel", SHAPE_DRAW_2D3D, 0, false));

//Initialize circle agents to show attractors in nodes
for (RectangularNode RN: rn_list){
	Circle c = new Circle();
	c.goToPopulation(circles);
	c.jumpTo(RN);
	}
```

## Methods

### addRectangularNodePrototype
> public void addRectangularNodePrototype​(java.lang.String prototypeName, double width, double height)

*Description:*

Adds a new rectangular node prototype to the pool.

*Parameters:*

* prototypeName - User specified name of the prototype. Used as an identifier.

* width - Width of the prototype.

* height - Height of the prototype.

### addAttractorToRectangularNodePrototype

> public void addAttractorToRectangularNodePrototype​(java.lang.String prototypeName, double relX, double relY, int orientation)

*Description:*

Adds an attractor to an existing rectangular node prototype.

*Parameters:*

* prototypeName - Name of the prototype to add the attractor.

* relX - X-coordinate of the attractor relative to the prototype origin.

* relY - Y-coordinate of the attractor relative to the prototype origin.

* orientation - Orientation of the attractor in degrees.

### addNodeGroupPrototype

> public void addNodeGroupPrototype​(java.lang.String prototypeName, double width, double height)

*Description:*

Adds a new node group prototype to the pool.

*Parameters:*

* prototypeName - User specified name of the prototype. Used as an identifier.

* width - Width of the prototype.

* height - Height of the prototype.

### addRectangularNodeToNodeGroupPrototype

> public void addRectangularNodeToNodeGroupPrototype​(java.lang.String nodeGroupPrototypeName, java.lang.String rectangularNodePrototypeName, double relX, double relY, boolean flipX, boolean flipY, int rotation)

*Description:*

Adds a rectangular node to an exiting node group prototype.

*Parameters:*

* nodeGroupPrototypeName - Name of the node group prototype to add the rectangular node.

* rectangularNodePrototypeName - Name of the rectangular node prototype of the rectangular node to be added.

* relX - X-coordinate of the prototype origin of the rectangular node relative to the node group prototype origin.

* relY - Y-coordinate of the prototype origin of the rectangular node relative to the node group prototype origin.

* flipX - Whether the rectangular node to be added is flipped along the X-axis from its prototype.

* flipY - Whether the rectangular node to be added is flipped along the Y-axis from its prototype.

* rotation - The degrees of rotation of the rectangular node from its prototype. (Must be one of the following values: 0, 90, 180, 270)

### putRectangularNode

> public com.anylogic.engine.markup.RectangularNode putRectangularNode​(java.lang.String rectangularNodePrototype, double x, double y, boolean flipX, boolean flipY, int rotation)

*Description:*

Generates a rectangular node instance from prototype and places into the layout.

*Parameters:*

* rectangularNodePrototype - Name of the rectangular node prototype of the rectangular node to be placed.

* x - X-coordinate of the prototype origin of the rectangular node.

* y - Y-coordinate of the prototype origin of the rectangular node.

* flipX - Whether the rectangular node is flipped along the X-axis from its prototype.

* flipY - Whether the rectangular node is flipped along the Y-axis from its prototype.

* rotation - The degrees of rotation of the rectangular node from its prototype. (Must be one of the following values: 0, 90, 180, 270)

*Returns:*

The rectangular node generated.

### putRectangularNode

> public void putRectangularNode​(com.anylogic.engine.markup.RectangularNode node)

*Description:*

Places a rectangular node instance into the layout.

*Parameters:*

* node - Rectangular node instance to be placed into the layout.

### putNodeGroup

> public java.util.ArrayList<com.anylogic.engine.markup.RectangularNode> putNodeGroup​(java.lang.String nodeGroupPrototype, double x, double y, boolean flipX, boolean flipY, int rotation)

*Description:*

Generates a node group instance from prototype and places into the layout.

*Parameters:*

* nodeGroupPrototype - Name of the node group prototype of the node group to be placed.

* x - X-coordinate of the prototype origin of the node group.

* y - Y-coordinate of the prototype origin of the node group.

* flipX - Whether the node group is flipped along the X-axis from its prototype.

* flipY - Whether the node group is flipped along the Y-axis from its prototype.

* rotation - The degrees of rotation of the node group from its prototype. (Must be one of the following values: 0, 90, 180, 270)

*Returns:*

The ArrayList of rectangular nodes generated in sequence of addition to the node group prototype.

### getNodeAttractors

> public java.util.ArrayList<com.anylogic.engine.markup.Attractor> getNodeAttractors​(com.anylogic.engine.markup.Node n)

*Description:*

Returns all attractors in a generated node in sequence of addition to the node prototype.

*Parameters:*
* n - Node to get attractors.

*Returns:*

ArrayList of attractors in sequence of addition to the node prototype.

### addAlfaloPath

> public void addAlfaloPath​(double inX, double inY, double outX, double outY, boolean bidirectional)

*Description:*

Adds an Alfalo Path. All added alfalo paths will be configured and generated in the final layout after calling generateLayout().

*Parameters:*

* inX - X-coordinate of the source of the Alfalo path.

* inY - Y-coordinate of the source of the Alfalo path.

* outX - X-coordinate of the target of the Alfalo path.

* outY - Y-coordinate of the target of the Alfalo path.

* bidirectional - Whether this path is bidirectional.

### addAlfaloPath

> public void addAlfaloPath​(double[] path, boolean bidirectional)

*Description:*

Adds an Alfalo Path. All added alfalo paths will be configured and generated in the final layout after calling generateLayout().

*Parameters:*

* path - Array of coordinates that define the path. (e.g. [x1, y1, x2, y2, x3, y3])

* bidirectional - Whether this path is bidirectional.



