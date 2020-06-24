# The concept

The basic idea is that some of the nodes in the node tree are marked to be registered. Those are things like Start Operator or Create Panel. They mark the start of a block of code. Now the compiler will call the evaluate function on the node. This will return the python code of the node. This is not just text, but can also consist of other nodes that are connected to the start node. From here on the compiler will call evaluate on all the parts of the code that are nodes until all the code is text. This is the final python code and will get added to the addon. You will mainly write the evaluate function to give back the code that is returned. We provide a few helpful things to make that easier.

The information below should be useful to get started, but if you want to create a specific node, try to look at other nodes that do similar things to figure out how to get the things you want to achieve.

# Setting up a node

**Python file:**

To create a node, add your python file in the category directory you want it to go in. The name of the file doesn't matter.

**Imports:**

To create a scripting node, you'll need to import a few things. You can just copy the code below to get started:

```python
import bpy
from ..base.base_node import SN_ScriptingBaseNode
from ...compile.compiler import compiler
from ..base.node_looks import node_colors, node_icons
```

* **bpy:** This is obviously needed to create a custom node in blender

* **SN_ScriptingBaseNode:** This is the basic Scripting Node which your created node will inherit from

* **compiler:** This function returns the compiler object. You will only need the socket_update function from this. You need to call this in any properties you create 

* **node_colors:** This is a dictionary with the colors that are used by nodes. An example of this can be seen below in the init function. Here you can set the color of the node. The possible options are ["INTERFACE", "LOGIC", "INPUT", "OPERATOR", "PROGRAM", "SCENE"]

* **node_icons:** This is a dictionary with the icon names for the nodes. An example of this can be seen below under bl_icon. The possible options are ["INTERFACE", "LOGIC", "INPUT", "OPERATOR", "PROGRAM", "SCENE"]


**The node:**

Here's the basic code for creating a node:

```python
class SN_NodeName(bpy.types.Node, SN_ScriptingBaseNode):

    bl_idname = "SN_NodeName"
    bl_label = "The Nodes Label"
    bl_icon = node_icons["PROGRAM"]

    _should_be_registered = False

    _dynamic_layout_sockets = False

    _debug_node = False

    @classmethod
    def poll(cls, ntree):
        return ntree.bl_idname == 'ScriptingNodesTree'

    def socket_update(self, context):
        compiler().socket_update(context)

    def init(self, context):
        self.use_custom_color = True
        #self.color = node_colors["PROGRAM"]

    def draw_buttons(self, context, layout):
        pass

    def copy(self, node):
        pass

    def free(self):
        pass

    def evaluate(self, output):
        return {
            "blocks": [],
            "errors": []
        }

    def layout_type(self):
        return ""

    def data_type(self, output):
        return None

    def needed_imports(self):
        return []

    def get_register_block(self):
        return []

    def get_unregister_block(self):
        return []
```

* **_should_be_registered_:** This property defines if the node should be registered. This should be true if the result of the node should add something to the register and unregister functions. Examples of this are the Create Operator and Create Panel nodes
<br/><br/>
* **_dynamic_layout_sockets:** If this is true, layout sockets will be added to this node automatically. When a node is connected another one will be added, when one is disconnected it will be removed
<br/><br/>
* **_debug_node:** If this is true, the output of the evaluate function will be printed in the console when it is compiled. This is useful for debugging
<br/><br/>
* **socket_update:** You can use this as the update function of properties you create on the node. This is necessary to make the node work with the auto reload option
<br/><br/>
* **init:** This function gets called when the node is created. Here you can create the node in- and outputs. This is a blender function and the in and outputs can be created as usual. The types of sockets are ["SN_StringSocket", "SN_FloatSocket", "SN_IntSocket", "SN_BooleanSocket", "SN_DataSocket", "SN_VectorSocket", "SN_EnumSocket", "SN_LayoutSocket", "SN_ProgramSocket", "SN_SceneDataSocket"]
<br/><br/>
* **draw_buttons:** Here you can draw additional options on the node. This is a blender function and can be used as usual
<br/><br/>
* **copy:** This function gets called when the node is copied. This is a blender function and can be used as usual
<br/><br/>
* **free:** This function gets called when the node is deleted. This is a blender function and can be used as usual
<br/><br/>
* **evaluate:** This is the function that the compiler will call to get the python code from the node. The function gets an output. You can use this if youre node has multiple outputs. This doesn't apply to most nodes but if you make something like a data node where every output returns a single line then you might need to use this. The function returns a dictionary. This will be explained below.
<br/><br/>
* **layout_type:** This is needed if you create a layout node. An example is the Row node which returns "row". This can be used by connected nodes to find the layout type it should use. A Label node might call it on the connected node to return row.label
<br/><br/>
* **data_type** This is only needed if you create a node that uses data properties. An example is the scene data properties node which returns "bpy.types." combined with the type of the selected property, which can be found using .fixed_type. It should also return "bpy.types." + the data type + ".bl_rna.properties['" + the selected property + "']" if the selected property is a collection. This can be used by connected nodes to get the properties of the connected data type using .bl_rna.properties and .fixed_type.
<br/><br/>
* **needed_imports:** This contains a list of strings of the things that this node needs to import. The most common example is ["bpy"]
<br/><br/>
* **get_register_block:** This is only needed if _should_be_registered is true. It returns a list of strings, where every string is one line in the register function of the addon.
<br/><br/>
* **get_unregister_block:** This is only needed if _should_be_registered is true. It returns a list of strings, where every string is one line in the unregister function of the addon.



***Evaluate function:***

The evaluate function returns a dictionary containing a list called "blocks" and a list called "errors".


**Blocks:**

This is the code that the node creates. The blocks list contains dictionaries. These dictionaries have the following parameters:

*lines:* This is a list of code lines. Each line is either a list or a node socket. If it is a node socket, the compiler will call evaluate on the sockets node to generate the corresponding lines of code. This means that a socket without a list equates to multiple new lines of code. The other option is to have a list of strings and sockets. The sockets in these lists should belong to nodes which only return single lines. An example for this would be a print node where you have something like ["print(", connected_socket, ")"].

*indented:* This is also a list of lists and sockets and works the same way as the lines list. The different is that these lines will come after the lines list and will be indented. This can be used if you need to indent code in your node.

You can nest these code blocks, meaning that you can put dictionaries with lines and indented code inside other lists of lines or indents. With this you should be able to create any code you could need.

**Errors:**

This is a list of dictionaries that have the following parameters. It contains the errors that the node has. This could be that the user has connected a wrong input or something similar:

*error:* This is the type of error. It's a string from the following list: ["test_error", "wrong_socket_inp", "no_connection_inp", "wrong_socket_out", "no_connection_out", "no_connection", "no_layout_connection", "no_name_func", "no_name_var", "no_var_available", "no_prop_selected", "wrong_prop"]



**Helpful tools:**
We provide a few tools that should help you with the evaluating of nodes. You don't have to use these but they should make your life easier.

* **SocketHandler:** You can access the socket handler with self.SocketHandler. It helps you with getting the value of inputs that could be connected to other nodes. The most important function here is _socket_value_ which takes the input socket. It also can take an optional parameter for if the result should be returned as a list which is only useful for layout and program sockets. The function returns the value of the socket or the connected socket. It also checks for errors and returns those as well. The second useful function is get_layout_values which returns all layout values as well as the errors for the given node

* **ErrorHandler:** The function you will use here is handle_text. It checks for keywords in the text to see if they don't work with python syntax. This could be useful when creating something like a function which can't have any type of name
