# Uninstall process

This page explains how to remove J2O from an existing OMERO.web installation. The instructions are divided into two parts. The first part removes the plugin itself. Redis and Podman are removed separately because these services may also be used by OMERO or other applications.

>Unless stated otherwise, run these commands in the omero-web virtual environment and as the omero-web system user.
{style="note"}

>Before uninstalling J2O, make sure that no workflows are still running. Removing the plugin while tasks are active may interrupt processing and leave incomplete results or containers behind.
{style="warning"}

## Step 1 – Stop the Celery worker

Check whether the J2O Celery worker is running and whether it has active tasks:

```bash
celery -A JIPipePlugin status
celery -A JIPipePlugin inspect active
```

After all tasks have finished, shut down the worker:
```bash
celery -A JIPipePlugin control shutdown
```

>If the worker is managed by systemd or another process manager, stop and disable the corresponding service instead. Otherwise, the worker may be started again automatically.
{style=warning}

## Step 2 – Remove OMERO config values
Open the OMERO configuration editor:

```bash
omero config edit
```

Remove "J2O" from omero.web.apps.

Also remove the following entry from omero.web.ui.right_plugins:

```bash
["J2O", "J2O/right_plugin_example.js.html", "jipipe_form_container"]
```

Save and close the editor.

## Step 3 – Uninstall J2O

Uninstall the Python package from the omero-web virtual environment:


```bash
pip uninstall J2O
```

You can check whether the package is still installed using:

```bash
pip show J2O
```

>Packages installed through requirements.txt are not removed automatically. Do not uninstall all packages from requirements.txt without checking them individually. Some dependencies may also be required by OMERO.web or other installed plugins.
{style=warning}

## Step 4 – Remove the source repository

If you no longer need the cloned source code, navigate to its parent directory and delete the repository with:

```bash
rm -rf J2O
```

>This permanently deletes all files and local changes inside the repository. Back up any modified code or configuration files before running the command.
{style=warning}

## Step 5 – Restart OMERO.web

Restart OMERO.web to apply the configuration changes:

```bash
omero web restart
```


The J2O tab should no longer appear in the right panel of the OMERO webclient. You may need to reload the page for the changes to take effect.

## Optional cleanup

The following steps are only necessary when Redis or Podman was installed exclusively for J2O. 

> Do not remove shared services that are used by OMERO or other applications.
{style="warning"}

### Remove the Redis cache configuration

Check the current OMERO cache configuration:

```bash
omero config get omero.web.caches
```

If you changed the caching backend specifically for J2O, restore the configuration that was used before the installation. If there was no previous custom cache setting and you want OMERO to return to its default configuration, remove the override:

```bash
omero config set omero.web.caches
```

Restart OMERO.web after changing the cache configuration:

```bash
omero web restart
```

>Do not remove or change the Redis configuration if other parts of your OMERO installation depend on it.
{style=warning}


If Redis was installed exclusively for J2O and is no longer needed, it can be stopped and removed as the root user:

```bash
sudo systemctl disable --now redis-server
sudo apt-get remove --purge redis-server
```

### Remove Podman

If Podman was installed exclusively for J2O, disable its user socket as the user for which it was enabled:

```bash
systemctl --user disable --now podman.socket
```

Podman can then be removed as the root user:

```bash
sudo apt-get remove --purge podman
```

If the omero-web user was added to the podman group specifically for J2O, remove the membership:

```bash
sudo gpasswd -d omero-web podman
```

The group membership change takes effect after the user starts a new login session.

## After the uninstallation

Check the following points to confirm that J2O was removed successfully:

- The J2O tab no longer appears in the OMERO webclient.
- "J2O" is no longer present in omero.web.apps.
- The J2O right-panel entry is no longer present in omero.web.ui.right_plugins.
- pip show J2O no longer reports an installed package.
- No J2O Celery worker is running.