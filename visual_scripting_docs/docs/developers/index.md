# Introduction

The addon allows you to create packages of nodes that can be installed by the user. You are free to develop these packages and share or sell them.

If you develop a package, please let us know so we can promote it.

This documentation will teach you how we suggest you develop a node package and what we require.

## What you can do

Your package contains custom nodes which can then be used to create addons. The basic idea is that each node returns a dictionary with the code structure. This will then be compiled by the visual scripting addon to create the final python code. This will be explained in more detail in this documentation.

## Development

The basic structure which you will develop is the following:

You will distribute a *.zip* file to others which can then be installed in the addons user preferences.

Your zip file can hold multiple folders. These folders are where the *.py* files for the nodes go. Each python file equals to one node. The folders correspond to the node categories. These are the lists that show up in the Node Add menu.

**Example:**
If you want to create a node in the _Interface_ category, you'd add a folder called Interface and a python file inside that folder. This will be where the code for your node goes.

You can't nest folders to create subcategories! If you create a folder with a name that doesn't exist yet, that category will be created.

Finally you will need a _package_info.json_ file which also goes in the zip file. This holds information on your package. This will be explained in more detail as well.