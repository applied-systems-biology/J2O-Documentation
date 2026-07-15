# For Developers

J2O is fully open-source and relies on feedback from the community to improve. If you have ideas feel free to open a pull request [on our GitHub page.](https://github.com/applied-systems-biology/J2O)

If you want to experiment with the code yourself, feel free to clone the repository and install the package with the -e flag. To help with debugging, set debug to true in the omero config once the plugin is installed:

```bash
omero config set omero.web.jipipe.debug true
```

Restart omero-web and the Celery worker afterward for the change to take
effect. With debugging enabled, the *debug()* helper function forwards its arguments to *console.log()* whose output can be inspected in the browser console. 
