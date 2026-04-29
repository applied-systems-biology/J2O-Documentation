# Make your workflow editable

If you want to allow your users to modify the parameters of a workflow from within the OMERO webclient before executing it, you will need to create reference parameters. 

> Note that complex parameters are **NOT** supported.
> Read the [section below](#complex-parameters) to learn more.
{style="note"}

To create a reference parameter, first navigate to the project overview. On the right panel, you will find References in the parameter tab:

![References](../images/assets/images/ReferenceParameters.png){width="700"}

After clicking on References, create a group if you have not done so before using the *+ Add* button. Then, select a group and add the parameters you want by clicking *+ Add parameter reference*. After that, you can give the reference a custom name and description that will be displayed to users in the OMERO webclient to help them understand what the parameter does in your workflow:

![References customization](../images/assets/images/ReferenceCustomization.png){width="700"}

> Parameters of nodes within group nodes cannot be assigned as references as they are immutable!
{style=warning}

## Complex parameters
Certain parameters in JIPipe have a unique data structure, making a general integration impossible. 


> If you do add a complex parameter to your references anyway, 
> J2O will **NOT** give the user an option to change its value.  
{style="warning"}


A parameter is considered complex when it has multiple input fields. You can recognize this after adding a parameter
to the references:

![Complex parameter example](ComplexParameter.png)

> The average object diameter parameter of the Cellpose prediction node
> has a checkbox and a numeric input field and is therefore considered **complex**.