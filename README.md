Simple HTTP server
==================

This repository contains a sample implementation of a [Source-to-Image (S2I)](https://github.com/openshift/source-to-image) enabled container image for running a simple HTTP server.

Two different ways are provided for building the S2I builder image. The first uses a ``Dockerfile`` to build the image from a generic S2I base image. The second uses a Python S2I builder to generate a new custom S2I image which overrides the existing behaviour of the Python S2I builder image.

The ``s2i`` command line tool mentioned in these instructions can be downloaded from:

* https://github.com/openshift/source-to-image/releases

Building the Image from the Dockerfile
--------------------------------------

To build the image from the ``Dockerfile``, checkout this source code repository and run ``docker build``.

```
docker build -t simple-http-server .
```

If wishing to build the image in OpenShift, you can use:

```
oc new-build --strategy=docker --name simple-http-server \
  --code https://github.com/openshift-katacoda/simple-http-server
```

Building the Image Using Python S2I
-----------------------------------

To build the image using the Python S2I builder, checkout this source code repository and run ``s2i build``.

```
s2i build . centos/python-27-centos7 simple-http-server
```

If wishing to build the image in OpenShift, you can use:

```
oc new-build --strategy=source --name simple-http-server \
  --code https://github.com/openshift-katacoda/simple-http-server \
  --image-stream centos/python-27-centos7
```

Using the ``s2i`` Command Line Tool
-----------------------------------

To use the S2I builder image created locally with the ``s2i`` command line tool run:

```
s2i build <source-files> simple-http-server <image-name>
```

Replace ``<source-files>`` with a local file system path, or a URL for a hosted Git repository which contains the files you want to be made available using the HTTP server. Replace ``<image-name>`` with the name you wish to use for the container image created.

The source files from the local file system path or Git repository will be copied into the image created, at the location the HTTP server expects to find the files to be made available.

To start the web server, run:

```
docker run --rm -p 8080:8080 <image-name>
```

Using the Builder Image with OpenShift
--------------------------------------

To create an application and deploy it from the command line when using OpenShift, run:

```
oc new-app simple-http-server~<source-files> --name <application-name>
oc expose svc/<application-name>
```

Replace ``<source-files>`` with a URL for a hosted Git repository which contains the files you want to be made available using the HTTP server. Replace ``<application-name>`` with the name you wish to use for the application created.

If you didn't want to use OpenShift to build the builder image, you can import it from Docker Hub using:

```
oc import-image openshiftkatacoda/simple-http-server --confirm
```

In order to be able to use the S2I builder from the catalog available in the web console, you will need to instead run:

```
oc create -f https://raw.githubusercontent.com/openshift-katacoda/simple-http-server/master/imagestream.json
```

Alternatively visit the URL for the raw imagestream JSON file and cut and paste the contents in to the "Import YAML/JSON" page of the web console under "Add to project".

The S2I builder should then be able to be found by searching for "simple-http-server" in the catalog. In addition to being able to select it from the catalog, an application can still be created using ``oc new-app`` as shown above.
