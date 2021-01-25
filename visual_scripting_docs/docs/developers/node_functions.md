# Node Functions
These are functions that are also important for you to work with. They are split into different categories:

<br></br>

## Compiler functions

These are functions that you can call from your node which are required for the compiling to work:

**auto_compile(self, context)**
> This function is needed to make your node work with the compiler and to register any changes you're making. Make sure to call this function as the update function for node properties and in other cases where you update your node internally and the user needs to recompile. The function is part of your node

**add_error(self, title, description, fatal=False)**
> This function is used to add errors to the error panel. These should be shown when there are things the user didn't do properly and which should cause an error. These are not shown in the system console and should always be preferred over real python errors. It is an opportunity for you to improve the experience of the user with debugging. The function takes a title, a description and a boolean for defining if the error was fatal. A fatal error is something that is fundamentally different from what the user expected to happen. Fatal errors will cause a popup to appear informing the user of the error. This function is also part of your node

<br></br>

## Related node information

Sometimes you need to get information about connected nodes. These functions can help you with that:

**what_layout(self, socket)**
> This function is used for Interface nodes only. It returns a string which helps you get the layout type that the previous node is using. This could for example be a panel which returns ````layout``` or a row returning ```row``` or a split interface node returning ```split```. You use it to put your layouts in the right place, so for example ```{YOUR_LAYOUT}.label(text="")``` where you would get *YOUR_LAYOUT* with this function

**what_start_idname(self)**
> This will give you the idname of the node starting your node tree. This only works with execute and interface sockets. The function will try to follow these sockets until it finds the first node in the tree and give you the idname

**what_start_node(self)**
> This acts the same way the function above does but gives you the start node instead of its idname

<br></br>

## Unique names

Sometimes you need unique names or python compatible names. These are functions to help you with that:

**get_python_name(self, name, empty_name="")**
> This is a function for making any text into a valid python name. This can then be used as a variable name, a function name, etc.. The empty name is what is used if the provided name is empty or equates to empty when it's made into a valid python name

**get_unique_name(self, name, collection, separator="_")**
> This gives you a unique name for a given collection property or list of strings. It will try to increment a number at the end of the name if it finds one. If it doesn't, it will use the separator and add a number

**get_unique_python_name(self, name, empty_name, collection, separator="_")**
> This is a combination of the two functions above which will give you a unique python name

<br></br>

## Node Collections

These are functions which you can use to access the node collections. These are related to the has_collection parameter in the node options. In general collections are stored in the addon tree and accessible from nodes with ```self.addon_tree.sn_nodes[node_idname]```. This gives you a property group with a name and items. The items are a collection property which hold all node items with that idname. These have a ```name```, the ```node_uid``` and a function called ```node()``` to get the actual node belonging to the item:

**collection**
> This is a property function (call it like a property on the node) which returns the collection for this node. It is a shortcut for ```self.addon_tree.sn_nodes[self.bl_idname]```

**item**
> This is a property function which returns this nodes item from the collection for this node

**add_required_to_collection(self, idname_list)**
> This should be used in ```on_create``` to set up nodes collections if you need to access them. An example is the run operator node which adds a collection for the operator node as it needs to use a prop_search to find operator nodes. Collections are automatically created when a node is added but if none of the required type have been added yet this collection won't exist
