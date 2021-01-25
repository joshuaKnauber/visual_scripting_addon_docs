# Generating code

These are the most important functions in your node. They are being used by the compiler to get the actual code that makes up the final addon. They all need to return a dictionary with the ```code``` key in it. This contains a string which makes up the code.

```python
def code_evaluate(self,context,touched_socket):
    variable = "variables"
    return {
        "code": f"""
                # this is multiline code
                # we use formatted strings to add {variable}
                # the leftmost character is used as the base level for indentation
                    # so to indent code you - well... - you indent it...
                """
    }
```

```python
def code_evaluate(self,context,touched_socket):
    variable = "string"
    return {
        "code": f"# this is an inline {variable}. It won't cause a linebreak in the final code"
    }
```

Formatted strings are used to put in properties from your node or the values of sockets. These are the functions that you can overwrite:

**code_evaluate(self, context, touched_socket)**
> This function is used to return the main code for the node. It is called for the start of the node tree and when requesting code from connected nodes. The touched socket is the socket which code was requested from. For data sockets this is always an output where as for execute and interface sockets this is always an input

**code_imperative(self, context)**
> This function is used to return imperative code. This will be added at the top of the addon file unrelated to anything else and won't be indented by other code

**code_imports(self, context)**
> This function is used to return the code needed for importing. Serpens addons import ```bpy, os, math``` by default so you don't need to import that again

**code_register(self, context)**
> This returns the code that is used to register this node in code. This code will be added to the addons ```register()``` function

**code_unregister(self, context)**
> This returns the code that is used to unregister this node in code. This code will be added to the addons ```unregister()``` function

<br></br>

## Socket Code
With the functions above you should be able to return code from a single node. But if you have inputs or outputs that need to interact with other nodes to get you code, you can use the following functions:

socket.**code(indents=0)**
> This is certainly the most used function in this category. You call it on the socket of *your* node that you want to get the code from. An example would be if you have an integer input. You can use formatted code to put the value you get from the socket into your code. You will then call ```self.inputs["My Integer Socket"].code()``` on your socket and will get the correct code. This takes into account the value set on the socket itself as well as any connected nodes.

>If you have a math node connected to your input it will automatically handle calling ```code_evaluate``` on that node and give you the code for the math operation. It will also take care of possible errors with different connected data types. If a boolean socket was connected to your integer input it will try to cast it into an integer.

> While you would always call this function on inputs for data, you always have to call it on your nodes outputs for interface and execute. If you have an execute output leading to the next node you will call ```self.outputs[0].code(5)``` to get the code. As you can see in the example node at the top of this page, you'll have to provide indents for this. These indents are the number of indents you can see in your node python file. This is used if the function returns multiple lines of code which is almost always the case for Program sockets.

> If you are having trouble understanding this function have a look at some of the simpler nodes Serpens has, they all use this a lot

socket.**by_name(indents=0, separator="")**
> This function acts like the code function but can be used for dynamic sockets. Dynamic sockets will give you a arbitrary amount of sockets with the same name which you would have to account for. This function lets you get all of that code from one socket. Call it on a socket with the name you want and it will return all the code you need. Apart from indents you can also give it a separator which you can for example use to separate all the given values by comma

socket.**by_type(indents=0, separator="")**
> This function does the same as the by_name function but for the socket type. It will give you the code for all sockets with the type of the socket you call it on

node.**list_code(value_list, indents=0)**
> If the two functions above don't work for you for some reason, you can use this function to achieve the same with any given code. You pass it a list of strings which represent your code and give it the indents that your code should have
