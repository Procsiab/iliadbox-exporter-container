# Iliadbox Exporter Container

[![Container Build](https://github.com/Procsiab/iliadbox-exporter-container/actions/workflows/build-container-publish-dockerhub.yaml/badge.svg)](https://github.com/Procsiab/iliadbox-exporter-container/actions/workflows/build-container-publish-dockerhub.yaml)

![Docker Image Version (latest by date)](https://img.shields.io/docker/v/procsiab/iliadbox-exporter-container?label=Latest%20tag%20pushed%20on%20Docker%20Hub)


Export Prometheus-friendly metrics through my fork of [trazfr/freebox-exporter](https://github.com/trazfr/freebox-exporter)'s code: this repository hosts just the configuration needed to build a working container image to deploy into my infrastructure.

## Iliadbox application authorization

Before the application will be able to read the metrics, a token must be generated to let it access the Iliadbox's API.

Start the container in the following way, on a machine connected to the Iliadbox's LAN:
```bash
podman run --rm -it -p 9091:9091 -v $(pwd):/config:Z docker.io/procsiab/iliadbox-exporter-container
```

After following the on screen instructions on your Iliadbox, and allowing the application named "prometheus-exporter" to *"Edit the Iliadbox Settings"* (Settings > Iliadbox Parameters > Simplified Mode > Access Management > Pencil icon > click the corresponding checkbox), you will be able to communicate with the API using this application.

At this time, a file called `auth_token.json` should have been created inside your current directory: stop the container with `CTRL + C` and keep the contents of that file: that is the application token that you will use to grant access the API for this application from now on.

## Run the container

To run the application, assuming that you have already performed a successful authentication and have saved the application token, use the sdame command as above:

```bash
podman run --rm -it -p 9091:9091 -v $(pwd):/config:Z docker.io/procsiab/iliadbox-exporter-container
```

### NOTE

All the API calls are performed only via LAN towards the Iliadbox by default, as hardcoded inside the exporter.

You can also override the default command to disable the debug logging level and the host metrics collection, by omitting the respective arguments.
