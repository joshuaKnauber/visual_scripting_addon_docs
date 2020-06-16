# Setting up a node category

**Naming:**

To add a new node category, create a new folder in the nodes directory. The name of this folder will be used as the name of the category. Here the underscores will be replaced with spaces and the first letters of words will be made uppercase. However you can just use spaces in the name as well.

If you create a folder called _my_awesome_node_category_, the node category will be called _My Awesome Node Category_.

**Init file:**

To get the addon to recognize your category, you need to add a file called *__init_\_.py* inside the folder. This file can be empty but it needs to be there. However it's important to know that this is not required to be included in the final package, even if you create new categories. It's just needed during development.

**Notes:**

It's important to know that empty categories will not be shown. This means that after you have created your category, you need to add a node to it first for it to be shown in the add node panel.


