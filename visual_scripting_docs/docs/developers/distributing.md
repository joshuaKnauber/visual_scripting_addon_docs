# Distributing

To distribute your package, create a zip file. The name of this file doesn't matter.

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

* **name:** This is the name of your package which will be shown in the user preferences
* **description:** This is the description of your package which will be shown in the user preferences
* **autor:** You can put your name here to be shown in the user preferences
* **nodes:** This is a list of all nodes that are contained in your package. This is explained below

The package_info.json file goes right in the root of your zip file. The node category folders with the node python files should also go in the zip file. The folder structure should look like this:

**.zip file**<br>
|\_package_info.json<br>
|\category one<br>
|\_\_\_\_node_one.py<br>
|\_\_\_\_node_two.py<br>
|\category two<br>
|\_\_\_\_node_three.py<br>

The nodes parameter in the package_json should be a list of strings. Those strings should be relative filepaths to the different nodes.<br>An example could look like this:
```json
["category one/node_one.py",
"category one/node_two.py",
"category two/node_three.py"]
```

Once you finished packing up your zip file you are ready to distribute it to other people! Once again, make sure to let us know if you create a package so we can promote it.