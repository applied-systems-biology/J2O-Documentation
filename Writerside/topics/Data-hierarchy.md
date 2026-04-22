# Data hierarchy compatability

OMERO uses a specific model to organize its data. When running workflows on OMERO data using J2O, it is important that the workflow assumes a compatible data structure.

OMERO hierarchy is limited to one level of subfolders:
```
Project 
  └───Dataset1
  │     └───Images
  └───Dataset2
  │     └───Images
  └───...
```

> This means your workflow should at maximum assume one level of subfolders!
{style="warning"}

The actual data structure that is created by J2O depends on [the input nodes](Input-nodes.md) used in the workflow. 

- Using files or a singular dataset input (*Folder, File* and *File List* node):

```
NodeUUID
  └───Images/Files
```

- Using multiple datasets as input (*Folder List* and *Project user directory* nodes):

```
NodeUUID 
  └───Dataset1
  │     └───Images
  └───Dataset2
  │     └───Images
  └───...
```

> NodeUUID refers to the unique identifier of the used node. It is used by J2O to map which input goes where. This has the limitation that Project or singular dataset names from OMERO may get obscured and can't be used for filtering/annotation.
{style="note"}