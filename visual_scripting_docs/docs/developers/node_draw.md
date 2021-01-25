# Draw node

To draw properties and other things on your node these functions should help you:

**draw_node(self, context, layout)**
> This function is the main draw function for the node. You can use it to display properties or other things on your node

**draw_node_panel(self, context, layout)**
> This function is used to draw additional elements in the N-Panel that are not displayed on your node itself. Serpens constrains the UI to the node for all internal nodes but you can use this for packages. You might want to inform the user in some form that you are using the N-Panel as this is not a Serpens standard
