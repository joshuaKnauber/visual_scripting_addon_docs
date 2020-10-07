# Creating A Node

At the end of this page, you can find an empty example node which you can copy to quickly get started.

In theory you could put multiple nodes into one file, but we don't recommend doing that to keep your files better organized.

##Setup

You start by noting down the idnames of the nodes you want to create. This should be in the first lines of your file and written as a comment. Without this the addon will not find your node. We will use the print nodes structure as an example:

```python
#SN_PrintNode
```

##Imports

After this, there are a few imports that we require. This includes the bpy module as well as the basic visual scripting node class. The imports look as follows:

```python
import bpy
from ...node_tree.base_node import SN_ScriptingBaseNode
```

##Node Class

Now you can get to your actual node class. Be aware that you could also add things like property groups or operators inbetween here, if that is something that your node requires.

---

```python
class SN_PrintNode(bpy.types.Node, SN_ScriptingBaseNode):

    # required
    bl_idname = "SN_PrintNode"
    bl_label = "Print"
    bl_icon = "CONSOLE"

    # optional
    bl_width_min = 40
    bl_width_default = 160
    bl_width_max = 5000
    node_color = (0.2, 0.2, 0.2)
    should_be_registered = False
    min_blender_version = (2,83,0)
    serpens_versions = None
    docs = {
        "text": ["The print node is used to <important>write things to the console</>.",
                "",
                "Value Inputs: <subtext>What is connected here will be printed.",
                "<subtext>                   This can be any type of data like </>numbers, strings, etc."],
        "python": ["<function>print</>( <string>\"my example\"</>, <number>123</>, <string>\"test\"</> )"]
    }
```

To create your node, add a class with the name you specified at the top of your file. The class should inherit from _bpy.types.Node_ and the base node you imported.

You can treat this as a normal blender python class, meaning you can add properties and functions to it.

##Attributes

* **bl_idname**: The idname of your node. This needs to be the same as the class name and the name at the top of your file.
<br><br>
* **bl_label**: The label shown in the node categories and in the nodes header.
<br><br>
* **bl_icon**: The name of the icon that your node shows in it's header.
<br><br>
* **bl_width_min**: The minimum width of your node.
<br><br>
* **bl_width_default**: The default width of your node.
<br><br>
* **bl_width_max**: The maximum width of your node.
<br><br>
* **node_color**: This is the color of your node. Make sure to keep these pleasing for the eye and consistent between node types.
<br><br>
* **should_be_registered**: This is a very important argument as it determines if the node should be registered or not. We will go over this again below.
<br><br>
* **serpens_versions**: This is an optional parameter for providing versions of Serpens that this node is compatible with. You need to list all versions here. Changes that could affect your node are marked with changes in the first two numbers ```(x,x,-)```. The last one is for bugfixes, etc.. The parameter is given in the format ```[(x,x),(x,x),...]``` where the x's are the first two numbers of the addon version. The default is ```None``` which allows for all versions. You can get the addon version by calling ```get_serpens_version``` in your node
<br><br>
* **min_blender_version**: This is the minimum required blender version for this node. If it isn't above this version it will return empty code and an error. Make sure to provide the version in the format of ```bpy.app.version```: (x,xx,x)
<br><br>
* **docs**: This is the documentation for your node. It will be explained below as well.

**should_be_registered**<br>
As mentioned before, when the node tree is compiled, the addon goes through the tree and evaluates the nodes one by one. To find the nodes to start at you use this attribute. If it is true it is a node that the addon starts from. This is the case for nodes like 'Create Operator' or 'Panel'.

**docs**<br>
This holds the documentation for that node. It will be shown if the documentation overlay is enabled.<br>
The dictionary is split into _text_ and _python_. **"text"** should explain the function of the node and what the different inputs do. Please do not just repeat the basics here, but make note of things that are specific to this node. **"python"** should hold the python equivalent of your node if possible.

Both attributes need to be lists of strings, where one string represents one line. They can be left empty but both lists need to be there.

You can use color formatting on your strings. This works by wrapping the text you want to color in tags. The start of a color is marked with **<color_name\>**. The color can be one of these: **[serpens, subtext, important, function, number, string, data, red, blue, green, orange, yellow, grey, lightgrey, black]** or just a color in the form **<(0.2, 0.3, 0.4)\>**. To mark the end of a colored section, use **<\\>**. This way you can format your python code and also mark important parts of your documentation text.


These are the attributes that you need to and can set for your node. Next we will cover the functions.

##Functions

You can overwrite these functions in your node to make it actually do something:

---
**initialize(self, context)**

This is the function that is called once when the node is created. Here you should add the inputs and outputs to your node and initialize any other values.

You can add an inputs and outputs like this:
```python
def inititialize(self, context):
    self.sockets.create_input(self,"EXECUTE","Execute")
    self.sockets.create_input(self,"DATA","Value")
    self.sockets.create_output(self,"EXECUTE","Execute")
```
As you can see, you need to call functions from self.sockets, which handles the nodes sockets. You can find them [here](./socket_handler.md).

---


---
**evaluate(self, socket, node_data, errors)**

This is the most important function in your node, as it is what the addon calls to get the generated python code. This needs to return a dictionary in a specific form for this to work.

```python
def evaluate(self, socket, node_data, errors):
    return {
        "blocks": [
            {
                "lines": [
                    ["line"," 1"],
                    ["line 2"]
                ],
                "indented": [
                    ["indented line"," 1"],
                    ["indented line 2"],
                    {
                        "lines": [
                            ["code in nested code block"]
                        ],
                        "indented": [
                            ["double indented code"]
                        ]
                    }
                ]
            }
        ],
        "errors": errors
    }
```
The evaluate function needs to return a dictionary with two keys. The first is **"blocks"** which contains the actual generated python code. This is a list of code blocks.

A code block is a dictionary with two keys. Those are **"lines"** and **"indented"**. Both are lists which hold lines of code. One line is a list of strings and sockets. That means that you can separate parts of the line with commas. If you have a socket in one of your lines, the node of that socket will be evaluated and the code will be added to that line. If you know that the sockets node will return multiple lines just use a list with only that socket, it will then be adjusted accordingly to add those lines.

An example would be the print node where you have a line like ```["print(", socket, ")"]```. The socket is the socket that is connected to the input of the print node.

While you could check what sockets are connected yourself, we provide the _node_data_ for evaluate. This is a dictionary containing **"input_data"** ,**"output_data"** and **"node_tree"**. Node Tree is the tree, the node lives in.

Input and Output data are lists. They contain one dictionary for each input/output your node has in the order they appear. Those dictionaries have the following data:

* **socket**: The socket that this dictionaries data belongs to.
* **name**: The name of the socket.
* **value**: The value of the socket. This is always a string and is the actual value entered in the socket. For a integer socket this could be '5'.
* **connected**: This is None if there is nothing connected to the socket or the connected socket. This can be used as a part of a line.
* **code**: This is the combination of value and connected. If there is no node connected to the socket it is equal to _value_, if there is a valid socket connected it is equal to _connected_.

Most of the time you can use the code parameter in your lines to return the sockets value. For the print node example this would be ```["print(", node_data["input_data"][1]["code"], ")"]```.

The **"indented"** list works the same as the **"lines"** list, but they come after the lines and are one step indented. It is important to know that you can nest code blocks. This means you can put code blocks (dictionaries with _lines_ and _indented_ lists) in the lists instead of a line. You can see this better in the example above. This way you can create more complicated indents and nesting in your generated code.

Make sure you understand this structure before starting your nodes as it is very important for proper generated code.

Finally the dictionary returned by evaluate also contains a key called **"errors"**. This is a list of errors that are related to this node. This could be a socket that is connected to something it shouldn't be or a name that hasn't been set properly. These errors will be displayed in the N-Panel. It is important that you actually make use of these and not just let the error trickle down to the actual python code. Make sure to always have default alternatives for these errors, so you can still return working code.

Errors is a list of dictionaries, where each dictionary is an error. It has the following keys:

* **title**: The title of the error message.
* **message**: The content of the error message.
* **node**: The node the error came from. This is usually _self_.
* **fatal**: True or False, depending on if the error completely changes how the addon behaves or is just a minor issue that doesn't affect much.

There is a list of errors provided as a parameter for evaluate. This contains errors coming from wrongly connected sockets. While you should use these, you can of course add your own errors if you have more specialized cases.

Another parameter passed to the evaluate function is _socket_. This is the socket that your node got evaluated from. If you have a node that needs to return different things depending on what socket it's called from then you need to use this. If your node is being called because it's being registered this will be None.

The evaluate function is where you will probably spend most of the time and where most of your code lives. Make sure to organize it properly and split some of the things in other functions to be able to properly keep track of errors. Again, it's important that the user doesn't have to deal with actual python errors. This addon is not about generating the most efficient code, but to provide the best user experience when making addons with no coding knowledge.

---


---
**draw_buttons(self, context, layout)**

Here you can draw additional layouts on your node. This will be shown at the top of your nodes and can include anything you can put in a blender ui layout.

```python
def draw_buttons(self, context, layout):
    layout.label(text="My label")
    layout.prop(self,"my_node_property")
```
You can also draw an icon selector here by calling ```self.draw_icon_chooser(layout)```. The chosen icon name can then be accessed with ```self.icon``` anywhere in your node.

---


---
**get_register_block(self)**

If your node has **should_be_registered** set to True, it needs to provide code that is added to the register function. The function returns a list where every string is one line.

```python
def get_register_block(self):
    return ["bpy.utils.register_class(\"MyClass\")"]
```

---


---
**get_unregister_block(self)**

If your node has **should_be_registered** set to True, it needs to provide code that is added to the unregister function. The function returns a list where every string is one line.

```python
def get_unregister_block(self):
    return ["bpy.utils.unregister_class(\"MyClass\")"]
```

---


---
**required_imports(self)**

Your node can import modules by adding them to the required imports function. This returns a list of strings where every string is one module.

```python
def required_imports(self):
    return ["bpy","random"]
```

---


---
**layout_type(self)**

If your node is a layout node like row, column, panel, etc. it needs to return it's layout type for the connected nodes to use. Nodes like checkbox, etc. that just live in layouts, but can't have anything living in them, don't need this function. Some nodes just need to pass the layout type of the previous node. If that's the case just call layout_type on the node connected to your nodes layout input.

Be aware that this layout type is actually just the name of the variable that you still have to set in the code returned in your evaluate function. For row this would be ```["row = ",layout_type,".row()"]``` where _layout_type_ is the layout type of the connected node. In the case of the row node it comes from ```layout_type = self.inputs[0].links[0].from_node.layout_type()```.

```python
def layout_type(self): # example for panel
    return "layout"

def layout_type(self): # example for row
    return "row"
```

---


---
**data_type(self, output)**

This returns the data type of the given output. It is needed for nodes like Object Data. The data type needs to be in a string and come from _bpy.types_.

This function is called by connected data nodes that need the data type of the previous node before the addon is compiled. You can also do this from your nodes. You can see this in action with the Object Data and Get Object Data Properties nodes.

```python
def data_type(self, output):
    if output.name == "Object":
        return "bpy.types.Object"

    elif output.name == "Bone":
        return "bpy.types.Bone"
```

---


---
**reset_data_type(self, context)**

This is used for "chains" of data nodes. An example would be an Object Data node which has multiple Get Object Data Properties nodes connected after each other. Now the user changes the enum on the Object Data node. The result is that all connected nodes need to be updated because their options might change. The Object Data node starts by calling reset_data_type on the connected node. This updates its values and then calls reset_data_type on the next node.

So if you create a node which makes use of these features then you need to use the reset_data_type node for that and also call that function on all nodes connected to your nodes outputs when you update something that could change the connected nodes.

```python
def reset_data_type(self, context):
    # do some stuff on this node

    if self.outputs[0].is_linked: # you should check if the right socket is connected
        self.outputs[0].links[0].to_node.reset_data_type(context)
```

---


---
**get_variable_line(self)**

This is used to create new variables in the generated addons property group. Note that there are no local variables accessible to the user. They are all stored as properties so they can be used everywhere. If you want to add a variable, you need to use this function. You will return the line that gets added to your property group like in the examples below.

```python
def get_variable_line(self):
    return "prop_name: bpy.props.IntProperty(name\"Name\")"

# or 

def get_variable_line(self):
    return "my_collection: bpy.props.CollectionProperty(type=MyPropertyGroup)"
```

If you want to create an array, create a collection property where the name is ```ArrayCollection_UID_```. UID will then be replaced with the addons unique identifier. You don't need to do that, this is managed in the compiler. This array property group has properties for any type, meaning you can use it no matter what type of array you're creating.

---


---
**get_array_line(self)**

This is used to set create defaults in your created collection properties. This is done in a function that is only called once for each file. This is meant for adding default values to arrays.

```python
def get_array_line(self):
    return ["bpy.context.scene.sn_generated_addon_properties_UID_.my_name.add().bool = True",
            "bpy.context.scene.sn_generated_addon_properties_UID_.my_name.add().bool = False"]
```

This example adds two variables to an array called _my_name_.

---


---
**update_node(self)**

This function is called when the node is updated. This is the case when, for example, a socket is connected.

```python
def update_node(self):
    pass # do stuff to update your node
```

---


##Accessing Variables

You can access the created variables via the generated addons collection property. This is done with ```bpy.context.scene.sn_generated_addon_properties_UID_```.

This also makes use of the **\_UID_** Tag which allows you to access the generated addons unique identifier in your code. You mostly need this for variables or making sure that multiple generated addon classes don't register with the same name. 


##One more thing

Finally, we have an example node here. This doesn't have all the functions, but it has all required things and those that you usually want to use in your nodes. You can copy this to get started more quickly:

```python
#SN_MyNode

import bpy
from ...node_tree.base_node import SN_ScriptingBaseNode


class SN_MyNode(bpy.types.Node, SN_ScriptingBaseNode):

    bl_idname = "SN_MyNode"
    bl_label = "My Node"
    bl_icon = "MONKEY"
    node_color = (0.2, 0.2, 0.2)
    should_be_registered = False

    docs = {
        "text": ["<orange>This node hasn't been documented yet.</>"],
        "python": []
    }

    def inititialize(self,context):
        self.sockets.create_input(self,"EXECUTE","Execute")
        self.sockets.create_output(self,"EXECUTE","Execute")

    def draw_buttons(self, context, layout):
        pass

    def evaluate(self, socket, node_data, errors):
        return {
            "blocks": [
                {
                    "lines": [
                    ],
                    "indented": [
                    ]
                }
            ],
            "errors": errors
        }
```