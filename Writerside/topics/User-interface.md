# User interface
If installed correctly, the J2O tab can be found **in the right panel** of the OMERO webclient:

![Right panel tab](RightPanelTab.png)

Below you will find a detailed explanation of the interface sections you will see when opening the plugin.

> Before you can see the full user interface, you need to upload at least one workflow to OMERO. Learn how to do that [in the respective section](Uploading-workflows-to-OMERO.md).
{style="note"}

## FILE SELECTION

J2O will load its content dynamically depending on the file you select at the top of this section. When checking **Enable output config**, the [output node configuration](#output-node-configuration) will be accessible.

![File selection section](FileSelectionSection.png)

> If there are no files to select, [start by uploading a workflow to OMERO](Uploading-workflows-to-OMERO.md).
{style="note"}
> 
## RUNNING JOBS
In this section you will find a list of all the JIPipe jobs currently running on the server that were initiated by the current user. They can be identified by the time and date of execution and the name of the associated workflow file. By clicking the **✖ next to the entry**, you can terminate the associated job after confirming this action.

![Job section](RunningJobsSection.png)

You can also **click on the name of a workflow** to open a pop-up window that will display a live stream of its logfile:

![Live log pop-up](LogPopUp.png){width="700"}

## NODE SUMMARY
In this section you will find an overview of the nodes detected in the associated workflow file. This can be used as a debugging tool to see whether the JIPipe pipeline was constructed correctly according to the [*Preparing JIPipe workflows* section](WorkflowDesign.topic) and J2O therefore automatically detects the right amount of nodes.

![Node summary section](NodeSummarySection.png)

## INPUT NODE CONFIGURATION
This section allows you to enter the IDs of the OMERO objects that you want to use as input for specific nodes. **Clicking the input field** will open a scrollable dropdown menu that lists all available objects available to your OMERO group. You can simply click a listed object to add it to the input field. You can also **search for specific objects by typing its name** into the input field. To remove an entry from the input field, click the **✖** next to the ID.

![Input node config section](InputNodeConfigSection.png)

## OUTPUT NODE CONFIGURATION
When checking **Enable output config** in the [file selection](#file-selection), the output configuration becomes available. Here, you can choose a destination project for each of your exporter nodes. The data that is being exported by the node will be saved to the selected project as a dataset. Below the project input, you can enter the name you want the saved dataset to have. 

![Output node config section](OutputConfigSection.png)

>If you don't enter anything, J2O will default to a project called "JipipeResultsDefault". The images will be saved within 
> that project in a dataset named after the workflow file and the start time of execution.
{style="note"}

## PARAMETER CONFIGURATION
This section contains the input fields of the parameters that are defined as reference parameters within the workflow file. Starting with JIPipe 6, nodes with a predefined set of valid options will have a dropdown menu to choose a value from. Other nodes will accept strings, integers or floats as input depending on the node type. When **hovering the ?**, the plugin will display a tooltip with the description of the respective parameter if it was set in the workflow's [project overview](https://jipipe.hki-jena.de/documentation/project-overview.html).

Below this section you will find the **Start J2O** button to execute the selected workflow file with all the changes you made in the webclient.

![Parameter config section](ParameterConfigSection.png)

## WARNINGS & ERRORS
To make warnings and errors clearly visible, a yellow warning box above and a red error box below the **Start J2O** button will be displayed if anything is out of order.

![Warnings and Errors](Warnings&Errors.png){width="700"}

## LOG WINDOW
Below the start button, you will find the log window. The window will livestream the JIPipe logfile of the most recently started workflow execution. This can be used to check on the progress of the execution or to debug problems within the workflow.

> If you have multiple jobs running you can always inspect the log livestream of a specific job by clicking its name in
> the [*Running Jobs* section](#running-jobs).
{style="note"}

![Log window section](LogWindowSection.png)

The log window will only ever display the content of the log file from the most recent job started by the user. To review old log files or to inspect errors of jobs that ran in the background, you can find the log files **under the attachment tab of the output dataset**. By clicking on the file, an automated download will be started:

![Old log files](FindLogFiles.png){width="600"}

