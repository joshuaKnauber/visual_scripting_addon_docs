# Node Options

The node options tell Serpens what properties this node should have and how it should be treated when compiling. You only need to add the parameters to the node options that you actually want to change. You can see the defaults here:

```python
node_options = {
    "default_color": (0.3,0.3,0.3),
    "starts_tree": False,
    "register_order": 0,
    "has_collection": False,
    "collection_name_attr": "name",
    "import_once": True,
    "evaluate_once": False,
    "register_once": False,
    "unregister_once": False,
    "imperative_once": False
}
```

**default_color**
> This is a tuple of three values from 0-1 which sets the default color of the node

**starts_tree**
> This defines if this node should be treated as the beginning of a node branch. This is True for nodes like Panel, Operator, Function, etc.

**register_order**
> This changes the order in which nodes are registered. If you need a certain custom node to be registered before another set this value. Nodes with a lower number will be registered first. For unregister the order will be inverted

**has_collection**
> If this is true there will be an automatically created collection which holds all nodes of this type. This is used in nodes like Run Operator where you need to be able to select one of the nodes in your node tree of a certain type. How you can access these collections will be explained below

**collection_name_attr**
> This is the attribute that your node will be shown with in your collection. By default this is the name of the node, meaning if you have a prop_search for selecting a node, the items will be named with the names of the nodes. The panel node for example has a string property named *label* which is also used for this property

**import_once**
> If this is True, the ```code_imports``` function will only be called once for this node type in all node trees

**evaluate_once**
> If this is True, the ```code_evaluate``` function will only be called once for this node type in all node trees

**register_once**
> If this is True, the ```code_register``` function will only be called once for this node type in all node trees

**unregister_once**
> If this is True, the ```code_unregister``` function will only be called once for this node type in all node trees

**imperative_once**
> If this is True, the ```code_imperative``` function will only be called once for this node type in all node trees

<br></br>

## Node Properties

Serpens nodes have certain properties that can be useful in development:

**uid**
> This is a unique identifier for this node. You might sometimes want to use this to make sure your generated python names are unique across different addons or different nodes

**node_tree**
> This is the node tree your node is currently in. You can use this at all times in your code

**addon_tree**
> Because Serpens addons consist of multiple node trees you sometimes need to access the main node tree. It stores information about the addon itself and is generally used as a starting point for some things. You can access it at all times with this property

**prop_types**
> This is a dictionary with property types as keys. It will give you the matching socket type idname. An example would be ```self.prop_types["BOOLEAN"]``` which would give you ```"SN_BooleanSocket"```
