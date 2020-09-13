# Distribution

To distribute your package, create a zip file. The name of this file doesn't affect the package itself.


## Package Info

To distribute your package to others, you'll need to create a _package_info.json_ file.

The file should look like this:

```json
{
    "name": "Name Of Your Package",
    "description": "This is the packages description",
    "author": "Your Name",
    "nodes": [
    ]
}
```

* **name:** This is the name of your package which will be shown in the user preferences.

* **description:** This is the description of your package which will be shown in the user preferences.

* **autor:** You can put your name here to be shown in the user preferences.

* **nodes:** This is a list of all nodes that are contained in your package. This is explained below.

The package_info.json file goes in the root of your zip file. The node category folders with the nodes python files should also go in the zip file. The folder structure should look like this:

```python
.
├── package_info.json
├── Interface
│   └── new_node.py
├── New Category
│   ├── my_node.py
│   └── my_other_node.py
```

The **nodes** parameter in the package_json should be a list of strings. Those strings should be relative filepaths to the different nodes.<br>The example above would look like this:
```json
["Interface/new_node.py",
"New Category/my_node.py",
"New Category/my_other_node.py"]
```

As mentioned before, you do not need to add the \_\_init\_\_.py files to these category folders.

#Wrapping Up

Once you finished packing up your zip file you are ready to distribute it to other people! Once again, make sure to let us know if you create a package so we can promote it and add it to the marketplace for other people to find. This is purely benefitial for you, no matter if you sell the package or give it away for free.

If you have any other question, please let us know [on discord](https://discord.gg/NK6kyae) or [on twitter](https://twitter.com/joshuaKnauber).