## Dockerfile One-Line Runner

[![dockerfile runner badge](https://api.travis-ci.com/stasmihailov/dockerfile-runner.svg?branch=master)](https://travis-ci.com/github/stasmihailov/dockerfile-runner)

Adds ability to build AND run Dockerfiles in one command via `docker script` command. Also cleans up
resources created during execution (container + image)

### Installation

```shell script
./install.sh
```
... or just download `docker-script` file and run this:
```shell script
chmod +x docker-script
# creates docker plugin directory
[[ -d ~/.docker/cli-plugins/ ]] || mkdir -p ~/.docker/cli-plugins/  
# puts plugin into plugin directory
mv docker-script ~/.docker/cli-plugins/ 
```

### Motivation

This plugin is an elementary effort to try make docker more accessible for basic prototyping, e.g. verifying scripts
on different linux distributions without deploying an entire CI environment

You can always use a simple shell script which combines `docker build` and `docker run` with additional resource cleanup
commands, but it is not at all transferable - it would be better to have a single reusable plugin

*(I believe that tools like this one might make Docker a casual tool rather than a strictly devops tool, which is what
it's generally considered to be, at least from my experience)*

### Examples

`docker script` command can be run in a directory containing a `Dockerfile`, just like `docker build` (execution results
are presented as comments):
```shell script
echo "FROM alpine
CMD echo Dockerfile executed on $(uname)" > Dockerfile

docker script .
# Dockerfile executed on Linux
```

Also `docker plugin` can execute Docker commands from stdin...
```shell script
docker script << 'EOF'
    from alpine
    RUN echo 'in RUN'
    CMD echo 'in CMD'
EOF
# in RUN
# in CMD
```

... or from string input:
```shell script
docker script "
    from alpine
    RUN echo 'in RUN'
    CMD echo 'in CMD'"
# in RUN
# in CMD
```

