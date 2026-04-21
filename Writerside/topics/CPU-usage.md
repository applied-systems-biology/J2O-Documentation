# CPU usage

The CPU usage needs to be limited using the podman options for cpu_period and cpu_quota. Linux system don't support giving the number of CPU cores directly. Instead, the number of available CPUs is the cpu_period divided by the cpu_quota.

> Changing these settings requires a restart of OMERO web and the Celery workers.
{style="warning"}

## CPU period
The CPU period determines the length of the CPU scheduling window in µs. Per default, no scheduling time is set, which likely causes the OS to default to 100ms. If you would like to change that value, use the corresponding OMERO config command. For example, to set the CPU period to 200ms:

```bash
omero config set omero.web.jipipe.cpu_period 200000
```

## CPU quota
The CPU quota determines how much CPU time a process can use in the given CPU period. Per default, this is not set. To change the CPU quota per task, use the corresponding OMERO config command. For example, to set the CPU quota to 200ms:

```bash
omero config set omero.web.jipipe.cpu_quota 200000
```

> With the default 100ms scheduling time, a quota of 200ms effectively results in 2 cores being used per task.