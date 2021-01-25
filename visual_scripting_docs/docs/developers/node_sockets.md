# Node Sockets

```python
def on_create(self,context):
    self.add_execute_input("Execute")
    self.add_boolean_input("Boolean")
    self.add_execute_output("Execute").mirror_name = True
```

To set up the basic sockets of your node, add them in ```on_create```. The functions for adding inputs and outputs are all based on the node, meaning you can call them with ```self.add_TYPE_SOCKET("My Socket")``` where *TYPE* is the type of the socket you're adding and *SOCKET* is either input or output.

There are functions for all different socket types. Some of them are also dynamic, which means that they are placeholders until another socket is connected or the add button is pressed which will add another socket with the same type and name before it. Each of the following functions is called on the node and takes in the name of the socket as a parameter. They all return the socket they create:

**add_interface_input/add_interface_output/add_dynamic_interface_input/add_dynamic_interface_output** [Interface]

**add_execute_input/...** [Execute]

**add_string_input/...** [String]

**add_boolean_input/...** [Boolean]

**add_float_input/...** [Float]

**add_integer_input/...** [Integer]

**add_data_input/...** [Data]

**add_list_input/...** [List]

**add_dynamic_variable_input/add_dynamic_variable_output** [Variable]

**add_icon_input/add_icon_output** [Icon]

**add_blend_data_input/add_blend_data_output** [Blend Data]

To remove sockets simply use ```self.inputs.remove(input)``` or ```self.outputs.remove(output)```.

<br></br>    

## Socket Properties
The functions return the socket which allows you to set a bunch of parameters depending on the socket type:

**group** [All socket types, readonly] (str)
> Set to ```DATA``` for all socketes except for Interface and Execute where it is ```PROGRAM``` 

**socket_type** [All socket types, readonly] (str)
> The type of the socket

**subtype** [All socket types] (str)
> The subtype of the socket. This is different for each type of socket:

*    String (NONE, FILE, DIRECTORY, ENUM)
*    Boolean (NONE, VECTOR3, VECTOR4)
*    Float (NONE, FACTOR, COLOR, COLOR_ALPHA, VECTOR3, VECTOR4)
*    Integer (NONE, VECTOR3, VECTOR4)
*    Blend Data (NONE, COLLECTION)

**dynamic** [All socket types, readonly] (boolean)
> Boolean that tells you if the socket is dynamic

**copy_socket** [All socket types] (boolean)
> Boolean that tells you if the dynamic socket should copy all of the properties when a socket is connected. This includes type, name, etc.

**removable** [All socket types] (boolean)
> Boolean that defines if this socket shows a remove button

**addable** [All socket types, readonly] (boolean)
> Boolean that defines if this socket shows an add button for adding another socket of this type

**to_add_idname** [All socket types, readonly] (string)
> Required if addable or dynamic is true. Should be set to the idname that is added by this socket

**disableable** [All socket types] (boolean)
> Shows an eye icon on the socket to disable this socket

**enabled** [All socket types] (boolean)
> Enables or disables the socket. You can check this in your code functions

**default_text** [All socket types] (string)
> The displayed text of this socket. Use this instead of name

**mirror_name** [All socket types] (boolean)
> Mirrors the name of the connected socket if this is enabled. This is purely visual. Execute outputs should mirror the name of the connected socket and so should interface inputs

**take_name** [All socket types] (boolean)
> Makes this socket take over the name of the socket that is first connected to this socket

**variable_name** [All socket types] (string)
> The variable name of this socket. This property is used by the variable properties below

**return_var_name** [All socket types] (boolean)
> Skips the code functions for this socket and just returns the variable name instead

**show_var_name** [All socket types] (boolean)
> Shows the variable name on the socket

**edit_var_name** [All socket types] (boolean)
> If enabled together with show_var_name, the variable name can be edited on the socket

**data_type** [Blend Data] (string)
> Should be set to the data type of this socket. This is something from ```bpy.types```

**data_name** [Blend Data] (string)
> Should be set to the name of this sockets data type

**data_identifier** [Blend Data] (string)
> Optional property that can be set to the identifier of this blend data type

**enum_values** [String] (string)
> The enum items if this string is an enum subtype. This should be in the format of ```"[("ITEM","name","description"),]"```

<br></br>   

## Socket Utility Functions
With this you should be able to add sockets. You can also manipulate your sockets after the node is added. There's a bunch of functions which help you with that:

**add_input_from_prop(self, prop)** - **return** input
> Takes in a property like StringProperty, BoolProperty, etc. and creates a socket with the proper type and name

**add_output_from_prop(self, prop)** - **return** output
> See ```add_input_from_prop```

**enum_items_as_string(self, enum_prop)** - **return** string
> Gives you a string which you can set your enum_values of a string socket from an enum property

**change_socket_type(self, socket, idname)** - **return** socket
> Takes a socket and an idname. It changes the socket type to the given idname but keeps the connections

**remove_output_range(self, start_index, end_index=-1)**
> Removes all outputs from the start index to the end index

**remove_input_range(self, start_index, end_index=-1)**
> Removes all inputs from the start index to the end index