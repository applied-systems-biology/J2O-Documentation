# Frequently asked questions

This section will be used to answer the most common questions about J2O. We will try to keep the website up-to-date, but if you cannot find your answer here, check out the [community](https://image.sc/) or feel free to contact us directly.

## Won't heavy image processing eat up all my server's resources?

A major concern system administrators have with J2O is that running image processing on the OMERO server will destabilize the database operation. This is a valid concern and for that the plugin offers settings that let you control precisely the amount of RAM and CPU cores that will be used per workflow execution. Additionally, there are options for the Celery task queue that lets you limit the amount of parallel processed workflows. Combined, this lets you set a maximum resource use of the plugin, reserving the rest for database operations. For more details, check the [resource management section](Optional-settings.md#resource-management).