StatsD + Graphite + Grafana + Kamon Dashboard
---------------------------------------------

This image contains a fork of the Dockerfile
[kamon-io/docker-grafana-graphite](https://github.com/kamon-io/docker-grafana-graphite)
and I have made  some modifications to work together with the Raspberry
Monitoring from [abarbanell/rpym](https://github.com/abarbanell/rpym).
Even this README file is just a minor edit of the original from
the guys at [kamon-io](https://github.com/kamon-io). Thank you for the
great work!


This image has a sensible default configuration of StatsD, Graphite and
Grafana, and comes bundled with a example
dashboard that gives you the basic metrics currently collected by
[rpym](https://github.com/abarbanell/rpym) for both Raspberry Pi
Monitoring as well as monitoring of the docker host on which this image
will be running. In fact, it should work on any Linux system. There are
two ways for using this image:


### Using the Docker Index ###

This image is published under [my repository on the Docker
Index](https://index.docker.io/u/abarbanell/) and all you
need as a prerequisite is having Docker installed on your machine. The
container exposes the following ports:

- `80`: the Grafana web interface.
- `8125`: the StatsD port.
- `8126`: the StatsD administrative port.

To start a container with this image you just need to run the following command:

```bash
docker run -d -p 80:80 -p 8125:8125/udp -p 8126:8126 --name grafana-dashboard abarbanell/docker-grafana_graphite
```

If you already have services running on your host that are using
any of these ports, you may wish to map the container ports
to whatever you want by changing left side number in the `-p`
parameters. Find more details about mapping ports in the [Docker
documentation](http://docs.docker.io/use/port_redirection/#port-redirection).

This has the limitation that both logs and data directories inside the
container and thus it is difficult to upgrade the container without
loosing the data files. Therefore you can take a look at the "start"
file provided in the git repo and either use it directly or take the
commands like so:

```
docker run -d -v $(pwd)/logs:/var/log/supervisor -v $(pwd)/data:/opt/graphite/storage/whisper -p 80:80 -p 8125:8125/udp -p 8126:8126 --name rpym-grafana-dashboard abarbanell/docker-grafana-graphite
```


### Building the image yourself ###

The Dockerfile and supporting configuration files are available in our
[Github repository](https://github.com/kamon-io/docker-grafana-graphite).
This comes specially handy if you want to change any of the StatsD,
Graphite or Grafana settings, or simply if you want to know how that
image was built. The repo also has `build` and `start` scripts to make
your workflow more pleasant.


### Using the Dashboard ###

Once your container is running all you need to do is open your browser
pointing to the host/port you just published and play with the dashboard
at your wish. We hope that you have a lot of fun with this image and that
it serves it's purpose of making your life easier. This should give you
an idea of how the dashboard looks like when receiving data from one of
our toy applications:

![RPYM
Dashboard](http://blog.abarbanell.de/assets/img/2015/08/rpym-statsd.png)

More info can be found on my
[blog](http://blog.abarbanell.de/linux/2015/08/08/statsd-docker/)
