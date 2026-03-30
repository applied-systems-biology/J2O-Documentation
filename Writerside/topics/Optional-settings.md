# Optional settings

This section describes optional settings that can be used by system administrators to customize their J2O installation.

## Changing log and data storage paths
J2O by default uses the home directory of the omero-web user to handle temporary file transfers (e.g. /home/omero-web/j2o-files). You can change these directories using these commands:

```bash
omero config set omero.web.jipipe.tempdir "/path/to/tempdir"
omero config set omero.web.jipipe.logdir "/path/to/logdir"
```

The defined paths don't have to exist yet but will be created by the omero-web user if the permissions exist. Usually changing these should not be necessary. However, if you are experiencing permission errors or want to move the directories for any other reason you can do so. Remember to restart OMERO.web to apply those changes by running:
```bash
omero web restart
```

## Resource management
A major concern system administrators have when hearing about J2O is that using the database server for processing will eat up resources needed to facilitate the database operation. To prevent that, J2O offers system administrators some optional settings to limit the amount of RAM and CPU cores per workflow execution. Additionally, running the Celery worker with a concurrency flag limits the amount of parallel running tasks. Combined, these options can precisely control the resources that will stay untouched and available for the operation of your database. At a maximum, J2O will consume resources equal to the number of concurrent tasks times the set limits per task.

### CPU usage
The CPU usage needs to be limited using the podman options for cpu_period and cpu_quota. Linux system don't support giving the number of CPU cores directly. Instead, the number of available CPUs is the cpu_period divided by the cpu_quota. 

#### CPU period
The CPU period determines the length of the CPU scheduling window in µs. Per default, the scheduling time is set to be 100000 (100ms). If you would like to change that value, use the corresponding OMERO config command. For example, to set the CPU period to 100ms:

```bash
omero config set omero.web.jipipe.cpu_period 100000
```

#### CPU quota
The CPU quota determines how much CPU time a process can use in the given CPU period. Per default, this will be set to 200000 (200ms). Together with the default 100ms CPU period, this effectively limits the CPU processing power per task to 2 CPU cores. To change the CPU quota per task, use the corresponding OMERO config command. For example, to set the CPU quota to 200ms:

```bash
omero config set omero.web.jipipe.cpu_quota 200000
```

### RAM usage
Similar to the [CPU usage](#cpu-usage), the RAM usage per task can be limited using the podman option for mem_limit. The default value will be "8g" (8 gigabytes). The option accepts either integer values representing the memory limit in bytes or a string with a unit identification char (100000b, 1000k, 128m, 1g). If you use a string without identification char, the value is interpreted as bytes. To set the mem_limit, use the corresponding OMERO config command. For example, to set the memory limit to 8 gigabytes:

```bash
omero config set omero.web.jipipe.mem_limit "8g"
```

>Be mindful how much RAM you allocate per task, since the OOM killer will terminate a task that exceeds this limit.

### Task concurrency
Per default, the Celery task queue will have a maximum of concurrently processed tasks equal to the number of CPU cores. If you want to limit the amount of concurrently running tasks and therefore the used resources, use the -c or --concurrency flag when starting the associated Celery worker. For example, to allow a maximum of 4 concurrently running task:

```bash
celery -A JIPipePlugin worker --loglevel=info -E --detach -c 4
```