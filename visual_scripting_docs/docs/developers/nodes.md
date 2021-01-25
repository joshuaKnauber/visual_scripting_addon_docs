# Creating A Node

Below you can find an empty example node which you can copy to quickly get started.

In theory you could put multiple nodes into one file, but we don't recommend doing that to keep your files better organized.


## Example Node
Copy this code to get a base for your own nodes:
```python
import bpy
from ...node_tree.base_node import SN_ScriptingBaseNode



class SN_MyNode(bpy.types.Node, SN_ScriptingBaseNode):

    bl_idname = "SN_MyNode"
    bl_label = "My Node"
    bl_width_default = 160

    node_options = {
        "default_color": (0.3,0.3,0.3),
    }


    def on_create(self,context):
        self.add_execute_input("Execute")
        self.add_boolean_input("Boolean")

        self.add_execute_output("Execute").mirror_name = True
    
    
    def draw_node(self, context, layout):
        layout.label(text="My Node Label")


    def code_evaluate(self, context, touched_socket):
        
        return {
            "code": f"""
                    print({self.inputs["Boolean"].code()})
                    {self.outputs[0].code(5)}
                    """
        }
```