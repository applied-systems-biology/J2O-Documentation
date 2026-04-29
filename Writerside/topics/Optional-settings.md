# Optional settings

This section describes optional settings that can be used by system administrators to customize their J2O installation.

## Enabling high content screen (HCS) plate access
OMERO by default disables binary access for plates. To enable users to select screen plates as input for J2O, system administrators need to set the binary access on in the OMERO server config. To set **global** access, run the following command **as the OMERO server user**:

```bash
# Enable HCS/Plate original-file downloads
omero config set -- omero.policy.binary_access +read,+write,+image,+plate
# Restart the OMERO server
omero admin restart
```

> Read the [official documentation page](https://omero.readthedocs.io/en/v5.6.11-1/sysadmins/config.html) for more info. You can also set these permissions for individual groups!

> This is a server setting, which means the actual OMERO.server instance needs to be restarted for this!
{style=warning}

## Changing data storage paths
J2O by default uses the home directory of the omero-web user to handle temporary file transfers (e.g. /home/omero-web/j2o-files). You can change these directories using these commands:

```bash
omero config set omero.web.jipipe.tempdir "/path/to/tempdir"
omero config set omero.web.jipipe.logdir "/path/to/logdir"
```

The defined paths don't have to exist yet but will be created by the omero-web user if the permissions exist. Usually changing these should not be necessary. However, if you are experiencing permission errors or want to move the directories for any other reason you can do so. Remember to restart OMERO.web to apply those changes by running:

> Changing these settings requires a restart of OMERO web.
{style="warning"}

## Resource management
> Resource management is only supported for cgroupsv2! Check the [troubleshooting page](Troubleshooting.md#resource-limits-are-not-enforced) if you run into issues.
{style="warning"}

A major concern system administrators have when hearing about J2O is that using the database server for processing will eat up resources needed to facilitate the database operation. To prevent that, J2O offers system administrators optional settings to control the resources used per workflow execution. This includes:

- [](CPU-usage.md)
- [](RAM-usage.md)
- [](Task-concurrency.md)
- [](GPU-acceleration.md)


Combined, these options can precisely control the resources that will stay untouched and available for the operation of your database. At a maximum, J2O will consume resources equal to the number of concurrent tasks times the set limits per task.

> Changing these settings requires a restart of OMERO web and the Celery workers.
{style="warning"}