# Export nodes
Here you will find an overview of all supported nodes to save your processed data back to OMERO using J2O. Image data will be saved to OMERO datasets while non-image data will be attached to the datasets. If your workflow already uses these to export data, you don't need to change them. Otherwise, you will need to replace unsupported export nodes with one of these:

![Supported export nodes](../images/assets/images/ExportNodes.png){width="700"}

> Giving your nodes custom names helps to identify their purpose!
{style="note"}

To be fully compatible, J2O expects the *file-path* parameter of these nodes to define **a path relative to an [output user directory](Create-user-directories.md#output-directories)**. You can use string concatenation or the PATH_COMBINE() function for that:

![Export nodes expression](../images/assets/images/ExportNodeExpression.png){width="700"}

> The *auto_file_name* expression constructs the filename from the annotations of the exported object. While convenient, we recommend developers to give the exported files customized names instead, as [the auto_file_name expression can cause problems](Troubleshooting.md#solution-1-change-the-file-name-creation-in-the-workflow).
{style="note"}