# Manual installation

If you had troubles with the assisted installation or want to have a better overview of what operations are performed, this page will help you.

This plugin assumes that OMERO has been **set up for Ubuntu** as described in its [documentation](https://omero.readthedocs.io/en/stable/sysadmins/unix/install-web/web-deployment.html).

> If not told otherwise, these commands should be run in your [omero-web](https://github.com/ome/omero-web) virtual environment!
> {style="note"}

### Step 1 - Clone the repository
Clone the repository and navigate to the folder:
```bash
git clone https://asb-git.hki-jena.de/MWank/OMERO_JIPipe_Plugin.git
cd OMERO_JIPipe_Plugin
```

### Step 2 - Install J2O
Install the plugin using [pip](https://pip.pypa.io/en/stable/):
```bash
pip install .
```
Alternatively, if you want to experiment with the code, install it with the editable flag:
```bash
pip install -e .
```

### Step 3 - Install python requirements
Install the required python libraries using the requirements.txt:
```bash
pip install -r requirements.txt
```

### Step 4 - Setup podman
J2O relies on podman to launch containers. Install it and activate a user systemd socket **as the root user**:

```bash
apt-get update
apt-get -y install podman
systemctl --user enable --now podman.socket
```
>GPU acceleration requires podman 4.1 or higher. Ubuntu 22.04 does not provide this. Check the [GPU acceleration section](GPU-acceleration.md) to learn more!
{style="warning"}
 
If you get errors with podman after the installation, the cause may be that the omero-web user can't run podman. A possible solution is to add the omero-web user to the podman group:

```bash
sudo usermod -aG podman omero-web
```

### Step 5 - Setup redis as cache backend
>You may ignore this step if you have redis already setup as your caching backend
{style="note"}

First, install redis-server and start the service **as the root user**:
```bash
apt-get install -y redis-server
service redis-server start
```
Then, as the omero-web system user, edit the omero cache config to point to your redis server location:
```bash
omero config set omero.web.caches '{"default": {"BACKEND": "django_redis.cache.RedisCache", "LOCATION": "redis://127.0.0.1:6379/0"}}'
```
> Be sure your omero setup does not depend on other caching methods
{style="warning"}

### Step 6 - Edit OMERO config

Add "J2O" to the list of installed apps using:
```bash
omero config append omero.web.apps '"J2O"'
```

Add the plugin to the right panel plugins using:
```bash
omero config append omero.web.ui.right_plugins '["J2O", "J2O/right_plugin_example.js.html", "jipipe_form_container"]'
```
> Be sure not to add anything twice!
{style="warning"}

### Step 7 - Start a Celery worker
Start a celery worker to manage started tasks:
```bash
celery -A JIPipePlugin worker --loglevel=info -E --detach
```

Should you wish to terminate the workers associated with the plugin, simply run this in your omero-web environment:
```bash
celery -A JIPipePlugin control shutdown
```

> If you want to control the resources used by J2O, don't forget to [set the concurrency flag when starting the worker](Task-concurrency.md).
{style="note"}

### Step 8 - Restart omero web
Restart [omero-web](https://github.com/ome/omero-web) for the changes to take effect:
```bash
omero web restart
```

### After the installation
The J2O plugin should now be available in the OMERO webclient. To check if everything is functioning, first check if the J2O tab appears in the right panel of the webclient. After that, start a small demo workflow to see if Celery, redis and podman are functioning as expected.

> If there should be any problems, check the [troubleshooting page](Troubleshooting.md). If you can't find your issue there, feel free to contact us or open an issue in the [community](https://image.sc). You can also check the [FAQ section](Frequently-asked-questions.md) to see if your question has already been answered.