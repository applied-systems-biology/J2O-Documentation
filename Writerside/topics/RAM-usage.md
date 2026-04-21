# RAM usage

Similar to the [CPU usage](CPU-usage.md), the RAM usage per task can be limited using the podman option for mem_limit. The default will be unlimited RAM use. The option accepts either integer values representing the memory limit in bytes or a string with a unit identification char (100000b, 1000k, 128m, 1g). If you use a string without identification char, the value is interpreted as bytes. To set the mem_limit, use the corresponding OMERO config command. 

> Changing this setting requires a restart of OMERO web and the Celery workers.
{style="warning"}

For example, to set the memory limit to 8 gigabytes:

```bash
omero config set omero.web.jipipe.mem_limit "8g"
```

>Be mindful how much RAM you allocate per task, since the OOM killer will terminate a task that exceeds this limit.
{style="warning"}
