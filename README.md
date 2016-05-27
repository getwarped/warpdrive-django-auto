# Django Hello World

This is a simple Django Hello World application.

It is a test for using ``warpdrive`` with no additional configuration. When using ``warpdrive`` directly, or via a ``warpdrive`` enabled Source to Image (S2I) builder, ``mod_wsgi`` will be used as the WSGI server and Apache will be automatically configured to handle hosting of any static assets.

This example can also be deployed using the default Python S2I builder in OpenShift. In the case of the OpenShift S2I builder for Python, the builtin Django development server is used.

## Deployment to an OpenShift Cluster

To deploy to an OpenShift cluster using the default Python S2I builder provided by OpenShift, you can use either the OpenShift web console, or the ``oc`` command line tool.

To install using the web console, select ``Add to project``, then enter ``python`` into the ``Filter by keyword`` search box. You can then select which version of Python you would like to use for the deployment. Enter the name ``django-auto`` for the application, enter the ``Git Repository URL`` of:

* https://github.com/GrahamDumpleton/warpdrive-django-auto.git

and select ``Create``. This will initiate the build and deployment and will automatically expose the application for public access.

To install using the ``oc`` command line, you can run the command:

```
oc new-app https://github.com/GrahamDumpleton/warpdrive-django-auto.git --name django-auto
```

This will initiate the build and deployment. Then run the command:

```
oc expose django-auto
```

This will expose the application for public access. You can see the host name assigned to the application in the web console, or by running the command:

```
oc describe route/django-auto
```

In this case of running ``oc new-app``, OpenShift will automatically detect that the source code repository contains a Python application and will select the Python S2I builder. It will use the latest Python version available on the OpenShift cluster, which at this time is likely going to be Python 3.4.

If you want to use Python 2.7, you should instead use the command:

```
oc new-app python:2.7~https://github.com/GrahamDumpleton/warpdrive-django-auto.git --name django-auto
```

When using the default S2I builders for Python, the ``warpdrive`` package that this example is provided as a test for will not actually be used. If you want to use a ``warpdrive`` enabled S2I builder, you should instead use the command:

```
oc new-app grahamdumpleton/warp0-centos7-python27~https://github.com/GrahamDumpleton/warpdrive-django-auto.git --name django-auto
```

This will automatically pull down the S2I builder image called ``grahamdumpleton/warp0-centos7-python27`` from Docker hub and use it instead.

## Deleting the Deployed Application

If you want to run multiple tests with the different variations, you can delete the application by running from the ``oc`` command line as:

```
oc delete all --selector app=django-auto
```

Alternatively, assign the application a different name. Be mindful that if you create too many instances under different names, if the OpenShift cluster you are using has resource limits enabled, you may hit the limit on how many you can deploy.

## Related Deployment Examples

For further examples of using ``warpdrive`` to deploy the same Django Hello World application see:

* [warpdrive-django-django](https://github.com/GrahamDumpleton/warpdrive-django-shell) - Django development server.
* [warpdrive-django-gunicorn](https://github.com/GrahamDumpleton/warpdrive-django-gunicorn) - Gunicorn WSGI server.
* [warpdrive-django-uwsgi](https://github.com/GrahamDumpleton/warpdrive-django-uwsgi) - uWSGI WSGI server.
* [warpdrive-django-waitress](https://github.com/GrahamDumpleton/warpdrive-django-waitress) - Waitress WSGI server.
* [warpdrive-django-shell](https://github.com/GrahamDumpleton/warpdrive-django-shell) - Django development server (launched explicitly from a shell script).

As above, for each of these examples use the command:

```
oc new-app grahamdumpleton/warp0-centos7-python27~https://github.com/GrahamDumpleton/warpdrive-django-<example>.git --name django-auto
```

but replace ``<example>`` in the URL with the appropriate name for the example in question.

The ``warpdrive`` base S2I builder used here was ``grahamdumpleton/warp0-centos7-python27``. The full list of available ``warpdrive`` based S2I builders is:

* ``grahamdumpleton/warp0-centos7-python27``
* ``grahamdumpleton/warp0-centos7-python34``
* ``grahamdumpleton/warp0-debian8-python27``
* ``grahamdumpleton/warp0-debian8-python35``

The ``warp0`` designation in the name is to indicate that these are currently experimental builders and so subject to change. The first version of the official builder names when made available will carry the ``warp1`` moniker instead of ``warp0``. The number will be incremented if subsequent versions need to be released which are not fully compatible.






