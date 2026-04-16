# Task concurrency
Per default, the Celery task queue will have a maximum of concurrently processed tasks equal to the number of CPU cores. If you want to limit the amount of concurrently running tasks and therefore the used resources, use the -c or --concurrency flag when starting the associated Celery worker. For example, to allow a maximum of 4 concurrently running task:

```bash
celery -A JIPipePlugin worker --loglevel=info -E --detach -c 4
```
> Remember to kill outdated worker processes!
{style="warning"}