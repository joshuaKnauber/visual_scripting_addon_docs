# Event Functions

These are functions that you can overwrite in your node. They are called on certain events and meant for handling different things:

**on_create(self, context)**
> This function is called when the node is created. You should add your initial sockets here

**on_copy(self, node)**
> This function is called when the node is copied. Reset properties here if you need to. It gives you the newly created node which you should use instead of self in this function

**on_free(self)**
> This function is called when this node is deleted

**on_node_update(self)**
> This function is called when this node is updated in any way, for example when sockets get connected

**on_link_insert(self, link)**
> This function is called when a link is inserted, meaning when another socket is connected to this node. It gives you the newly created link. Contrary to the normal python function for link updates it is called with a slight delay so that you can use this link normally

**on_any_change(self)**
> This is called every time before the auto compile function is called

**on_outside_update(self, node)**
> This function is the one that is called when ```update_nodes_by_type``` is called for this node type. It gives you the node which this function was initiated from

**on_dynamic_add(self, socket, connected_socket)**
> This function is called when a dynamic socket is added to this node. You're given the socket that was created as well as the socket that is connected. This can be None if the dynamic socket was added with the add button instead of connecting another node

**on_dynamic_remove(self, is_output)**
> This function is called when a removable socket has been removed. It gives you a boolean which defines if the removed socket was an output

**on_var_name_update(self, socket)**
> This function is called when the variable name of a socket is changed

<br></br>

## Update Functions

These are functions which you can use to update nodes:

**auto_compile(self, context)**
> This function is needed to make your node work with the compiler and to register any changes you're making. Make sure to call this function as the update function for node properties and in other cases where you update your node internally and the user needs to recompile

**update_nodes_by_type(self, idname)**
> This function is used to update every node with the given idname. It calls ```on_outside_update``` on these nodes

**update_nodes_by_types(self, idname_list)**
> This function is like the one above but takes a list of idnames