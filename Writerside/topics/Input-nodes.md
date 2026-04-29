# Input nodes

Here you will find an overview of all supported nodes to import your OMERO data using J2O. This sections also covers the folder structure that is passed to JIPipe, which depends on the user input.

> Different folder structures require different handling  within the workflow. You can check out [the example at the end of the section](#example) for a less abstract explanation.
{style=warning}

> The `{variables}` below are taken from the selected OMERO objects. Understanding the naming convention enables advanced filtering from within a workflow.
{style=note}

## Image input
To use your OMERO image data as input for workflows executed with J2O, your workflows need to use one of the following input nodes:

![Image input nodes](../images/assets/images/InputStructure.png){width="700"}

> Giving your nodes custom names helps to identify their purpose!
> {style="note"}

### *Folder*

The *Folder* node allows users to select **a single dataset or screen plate** from OMERO. The folder that is passed to JIPipe depends on the selected data object:

![Folder node folder structure](FolderNode_FolderStructure.png){width="700"}

### *Folder list*

The *Folder list* node allows users to select **multiple datasets _or_ screen plates** from OMERO. The list of folders that is passed to JIPipe depends on the selected data object:

![Folder list node folder structure](FolderListNode_FolderStructure.png){width="700"}

### *Project user directory*

The *Project user directory* node allows users to select **multiple datasets or screen plates** from OMERO. The folder that is passed to JIPipe depends on the selected data object:

![Project user directory node folder structure](UserDirectoryNode_FolderStructure.png){width="700"}


## Non-image input
Using non-image inputs from OMERO in your workflows is supported via the following input nodes:

![File input nodes](../images/assets/images/FileNodes.png){width="700"}

> Giving your nodes custom names helps to identify their purpose!
> {style="note"}

### File
The *File* node allows users to select **a single file** from OMERO. The file is passed to JIPipe like this:

![](File.png)


### File list
The *File list* node allows users to select **multiple files** from OMERO. The files are passed to JIPipe like this:

![](FileList.png)

## Example
In this example we are using a *Folder list* node. We know from the documentation that if a user selects a dataset, JIPipe will receive a list of folders (datasets). However, when the user selects a screen plate, JIPipe receives a list of folders (plates) containing subfolders (wells). The workflow needs to handle both cases differently to access the images:

![](FolderListExample1.png)

Thanks to the folder naming convention, you can actually design your input to always support both datasets and plates:

![](FolderListExample2.png)

> The folders of plates will always start with `Plate_` which is why the filter works. The same logic can be applied for all `{variables}`! By [making filter parameters a reference parameter](Reference-parameters.md), you can even let the user change filters from within OMERO.
{style=note}
