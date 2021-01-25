# Distribution

To distribute your packages you need to pack your nodes into a zip file. This zip file should include all your python files in the correct node category folders as well as a package_info.json file. Make sure to include empty _\_init__.py files for any new categories.

```python
my_package.zip
|
├── package_info.json
|
├── Interface
│   └── new_node.py
|
├── New Category
|   ├──__init__.py
│   ├── my_node.py
│   └── my_other_node.py
```

## Package Info

The package_info.json should look as follows. You can copy this as a base:
```javascript
{
    "name": "The Name Of Your Package",
    "description": "A description of your package that shouldn't be longer than this",
    "author": "Your Name",
    "wiki_url": "https://www.yourpackagewiki.com",
    "package_version": [1,0,0],
    "serpens_versions": [[2,0,0]]
}
```


## Checklist

* Have you called auto_compile everywhere to make sure the compiler registers changes?
* Have you caught any possible errors that the user could encounter?
* Have you set proper and consistent names?


## Ways of distributing

When you are done with creating your package you are ready to distribute it.

Here's what you need to know:

* You can sell your package if you want
* You do not need to pay us anything for your sales
* Inform your customers that they need Serpens to use your product
* We are always happy to promote your package

Please message us on discord if you have a package that you want to share so we can add it to the package marketplace as well as let people know about it. This will help both of us to get more interest in Serpens and your packages.