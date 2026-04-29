# Create user directories

J2O utilizes JIPipe user directories to facilitate saving processed data to OMERO and using OMERO image data as input for custom scripts or expression nodes.

> To create user directories, navigate to the project overview and click the "Directories" or "User paths" button (depending on your version of JIPipe) and select "Add new directory".


![User directories button](UserPaths.png){width="700"}

## Output directories

To store processed data back to OMERO you will need to create at least one output user directory and use it in an [export node](Export-nodes.md). To mark a directory as output, include **"output"** in the **name** you set for the directory:

![](OutPutDirectoryExample.png){width="700"}

> Output directories are used to store both image and non-image data to OMERO
{style=note}

## Input directories

To use OMERO image data as input in custom scripts and JIPipe expression nodes, you need at least one input directory. To mark a directory as input, include **"input"** in the **name** you set for the directory:

![](InputDirectoryExample.png){width="700"}
