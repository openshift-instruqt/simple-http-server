Simple HTTP server
==================

This repository contains a sample implementation of a simple [Source-to-Image (S2I)](https://github.com/openshift/source-to-image) enabled container image for running a simple HTTP server.

Using the ``s2i`` Command Line Tool
-----------------------------------

To use the S2I builder image directly with the ``s2i`` command line tool, install ``s2i`` from:

* https://github.com/openshift/source-to-image/releases

and run:

```
s2i build <source-files> openshift-katacoda/s2i-http-server <image-name>
```

Replace ``<source-files>`` with a local file system path, or a URL for a hosted
Git repository which contains the files you want to be made available using
the HTTP server. Replace ``<image-name>`` with the name you wish to use for the container image created.

The source files from the local file system path or Git repository will be copied into the image created, at the location the HTTP server expects to find the files to be made available.

To start the web server, run:

```
docker run --rm -p 8080:8080 <image-name>
```

Using the Builder Image with OpenShift
--------------------------------------

If using OpenShift, to directly import the S2I builder image run:

```
oc import-image openshiftkatacoda/s2i-http-server --confirm
```

To create an application and deploy it from the command line, then run:

```
oc new-app s2i-http-server~<source-files> --name <application-name>
oc expose svc/<application-name>
```

Replace ``<source-files>`` with a URL for a hosted Git repository which contains the files you want to be made available using the HTTP server. Replace ``<application-name>`` with the name you wish to use for the application created.

In order to be able to use the S2I builder from the catalog available in the web console, instead run:

```
oc create -f https://raw.githubusercontent.com/openshift-katacoda/s2i-http-server/master/imagestream.json
```

Alternatively visit the URL for the raw imagestream JSON file and cut and paste the contents in to the "Import YAML/JSON" page of the web console under "Add to project".

The S2I builder should then be able to be found by searching for "s2i-http-server" in the catalog. In addition to being able to select it from the catalog, an application can still be created using ``oc new-app`` as shown above.
