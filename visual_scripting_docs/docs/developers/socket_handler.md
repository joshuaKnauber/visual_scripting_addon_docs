# Socket Handler
The socket handler holds a few functions that can help you to manage your nodes sockets. You can access it anywhere in your node with ```self.sockets```.

## Socket Types
There are different socket types that you can create. The names listed below are the socket types identifiers which we will use later:

* **STRING**: A string socket with a text field on the node. If this needs to be a valid python name then you can set ```your_socket.is_python_name``` to True and the addon will update the text to be valid python.
<br><br>

* **BOOLEAN**: A boolean socket with a toggle button on the node. You can set this to a checkbox by setting ```your_socket.show_toggle``` to False. You can also show the value as a text by setting ```your_socket.display_boolean_text``` to True.
<br><br>

* **INTEGER**: An integer socket. You can set ```your_socket.only_positive``` to True, to only allow for postive values.
<br><br>

* **FLOAT**: A float socket. You can set ```your_socket.use_factor``` to True, to have a factor slider from 0.0 to 1.0.
<br><br>

* **VECTOR**: A vector socket. You can set ```your_socket.is_color``` to True, to have a color displayed on your node. You can also set ```your_socket.use_four_numbers``` to True, to get an vector with four numbers.
<br><br>

* **DATA**: A data socket, which can have any of the previously mentioned sockets connected.
<br><br>

* **EXECUTE**: An execute socket, which is used to define the program flow.
<br><br>

* **LAYOUT**: An execute socket, which is used to define the flow of a layout.
<br><br>

* **OBJECT**: A socket for passing a singular data block between nodes. This is differentiated from a collection socket with the round input.
<br><br>

* **COLLECTION**: A socket for passing a collection of data blocks between nodes. This is differentiated from a single data block socket with the square input.
<br><br>

* **SEPARATOR**: A socket for separating your sockets. This is purely visual and does not have any use otherwise.
<br><br>


---
**create_input(node, socket_type, label, dynamic=False)**

This will create an input. You need to give it the node to create it on, which is usually _self_. It also requires the socket type which is one of the names above, as well as the label, which is the name displayed on the node. The dynamic parameter is optional. If it's set to True, it will dynamically add more sockets with the same appearance as you connect nodes to it.

```python
self.sockets.create_input(self, "STRING", "My Socket")
self.sockets.create_input(self, "FLOAT", "My Dynamic Socket", True)
```

This function also returns the created socket, so you can set the properties explained above.

---


---
**create_output(node, socket_type, label, dynamic=False)**

See _create_input_.

```python
self.sockets.create_output(self, "STRING", "My Socket")
self.sockets.create_output(self, "FLOAT", "My Dynamic Socket", True)
```

This function also returns the created socket, so you can set the properties explained above.

---


---
**remove_input(node, input_socket)**

This will remove the given socket from the node. The node parameter is usually set to _self_.

```python
self.sockets.remove_input(self, self.inputs[0])
```

---


---
**remove_output(node, output_socket)**

See _remove_input_.

```python
self.sockets.remove_output(self, self.outputs[0])
```

---


---
**change_socket_type(node, socket, socket_type, label="")**

This will turn the given socket into the given socket type. If you give a label then that label will be used instead of the old sockets label. The node parameter is usually set to _self_.

```python
self.sockets.change_socket_type(self, self.inputs[0], "STRING", label="I'm now a string!")
```

---