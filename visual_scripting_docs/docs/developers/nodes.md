# Setting up a node

**Python file:**

To create a node, add your python file in the category directory you want it to go in. The name of the file doesn't matter.

**Imports:**

To create a scripting node, you'll need to import a few things. You can just copy the code below to get started:

```python
import bpy
from ..base.base_node import SN_ScriptingBaseNode
from ...compile.compiler import compiler
```

* **bpy:** This is obviously needed to create a custom node in blender

* **SN_ScriptingBaseNode:** This is the basic Scripting Node which your created node will inherit from

* **compiler:** This function returns the compiler object. You will only need one function from this as explained below


**The node:**

Here's the basic code for creating a node:

```python
class SN_NodeName(bpy.types.Node, SN_ScriptingBaseNode):

    bl_idname = "SN_NodeName"
    bl_label = "The Nodes Label"
    _should_be_registered = False

    @classmethod
    def poll(cls, ntree):
        return ntree.bl_idname == 'ScriptingNodesTree'

    def socket_update(self, context):
        compiler().socket_update(context)

    def init(self, context):
        pass

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

    def needed_imports(self):
        return []

    def get_register_block(self):
        return []

    def get_unregister_block(self):
        return []
```

* **_should_be_registered_:** This property defines if the node should be registered. This should be true if the result of the node should add something to the register and unregister functions. Examples of this are the Create Operator and Create Panel nodes.