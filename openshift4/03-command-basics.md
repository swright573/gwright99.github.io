## Podman Command Basics

### Searching for and pulling images
For full details on searching for images via podman, see the official [Red Hat documentation](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/building_running_and_managing_containers/working-with-container-images_building-running-and-managing-containers). For my own purposes, I'll highlight and expand upon some of the tool's behaviours

NOTE: By default, podman will sequentially search for an image in all the repositories listed in `[registries.search]` section of `registries.conf`. Since I'm using a rootless implementation of podman, any change I make will need to be to the `~/.config/containers/registries.conf` file.

#### Searching for a specific image and/or official image
Running a generic search like `podman search rhel` will return a large amount of images:
```bash
INDEX       NAME                                             DESCRIPTION                                       STARS   OFFICIAL   AUTOMATED
docker.io   docker.io/library/alpine                         A minimal Docker image based on Alpine Linux...   6793    [OK]
docker.io   docker.io/anapsix/alpine-java                    Oracle Java 8 (and 7) with GLIBC 2.28 over A...   453                [OK]
docker.io   docker.io/byrnedo/alpine-curl                    Alpine linux with curl installed and set as ...   33                 [OK]
docker.io   docker.io/alpine/git                             A  simple git container running in alpine li...   146                [OK]
docker.io   docker.io/yobasystems/alpine-mariadb             MariaDB running on Alpine Linux [docker] [am...   69                 [OK]
docker.io   docker.io/mhart/alpine-node                      Minimal Node.js built on Alpine Linux             475
docker.io   docker.io/jfloff/alpine-python                   A small, more complete, Python Docker image ...   37                 [OK]
docker.io   docker.io/alpine/socat                           Run socat command in alpine container             58                 [OK]
docker.io   docker.io/hermsi/alpine-fpm-php                  FPM-PHP 7.0 to 7.4, shipped along with tons ...   25                 [OK]
docker.io   docker.io/hermsi/alpine-sshd                     Dockerize your OpenSSH-server with rsync and...   32                 [OK]
docker.io   docker.io/davidcaste/alpine-java-unlimited-jce   Oracle Java 8 (and 7) with GLIBC 2.21 over A...   13                 [OK]
docker.io   docker.io/frolvlad/alpine-glibc                  Alpine Docker image with glibc (~12MB)            247                [OK]
docker.io   docker.io/kiasaki/alpine-postgres                PostgreSQL docker image based on Alpine Linu...   45                 [OK]
docker.io   docker.io/zenika/alpine-chrome                   Chrome running in headless mode in a tiny Al...   24                 [OK]
docker.io   docker.io/etopian/alpine-php-wordpress           Alpine WordPress Nginx PHP-FPM WP-CLI             24                 [OK]
docker.io   docker.io/roribio16/alpine-sqs                   Dockerized ElasticMQ server + web UI over Al...   11                 [OK]
docker.io   docker.io/bushrangers/alpine-caddy               Alpine Linux Docker Container running Caddys...   1                  [OK]
docker.io   docker.io/spotify/alpine                         Alpine image with `bash` and `curl`.              11                 [OK]
docker.io   docker.io/mvertes/alpine-mongo                   light MongoDB container                           116                [OK]
docker.io   docker.io/cfmanteiga/alpine-bash-curl-jq         Docker Alpine image with Bash, curl and jq p...   6                  [OK]
docker.io   docker.io/bashell/alpine-bash                    Alpine Linux with /bin/bash as a default she...   18                 [OK]
docker.io   docker.io/davidcaste/alpine-tomcat               Apache Tomcat 7/8 using Oracle Java 7/8 with...   42                 [OK]
docker.io   docker.io/gliderlabs/alpine                      Image based on Alpine Linux will help you wi...   182
docker.io   docker.io/goodguykoi/alpine-curl-internal        simple alpine image with curl installed no C...   0                  [OK]
docker.io   docker.io/hermsi/alpine-varnish                  Dockerize Varnish upon a lightweight alpine-...   3                  [OK]
quay.io     quay.io/wire/alpine-deps                                                                           0
quay.io     quay.io/wire/alpine-builder                                                                        0
quay.io     quay.io/wire/alpine-intermediate                                                                   0
quay.io     quay.io/wire/alpine-prebuilder                                                                     0
quay.io     quay.io/0xff/alpine-sshd                                                                           0
quay.io     quay.io/wire/alpine-git                                                                            0
quay.io     quay.io/wire/alpine-helm                                                                           0
quay.io     quay.io/watchdogpolska/alpine-curl                                                                 0
quay.io     quay.io/tkaefer/wordpress-fpm-alpine                                                               0
quay.io     quay.io/yanxuehe/alpine-run-as-root                                                                0
quay.io     quay.io/giantswarm/alpine                                                                          0
quay.io     quay.io/opaqnetworks/alpine                                                                        0
quay.io     quay.io/codimd/server                            CodiMD container ===  [![Build Status](https...   0
quay.io     quay.io/karbon/alpine                                                                              0
quay.io     quay.io/aptible/alpine                           ![](https://quay.io/repository/aptible/alpin...   0
quay.io     quay.io/jitesoft/alpine                          # Alpine linux  [![Docker Pulls](https://img...   0
quay.io     quay.io/ukhomeofficedigital/alpine                                                                 0
quay.io     quay.io/signalfuse/collectd-alpine                                                                 0
quay.io     quay.io/kruczjak/docker-alpine-curl                                                                0
quay.io     quay.io/coreos/alpine-sh                                                                           0
quay.io     quay.io/kinvolk/alpine-dig                       A multi-arch Alpine image which contains the...   0
quay.io     quay.io/snapserv/base-alpine                                                                       0
quay.io     quay.io/cogentwebworks/alpine-s6                                                                   0
quay.io     quay.io/beyond/alpine                                                                              0
quay.io     quay.io/eschen42/alpine-cbuilder                 # A build environment for musl to target Alp...   0
```

In most cases, I want to use an official base image released by the maintaining organization. In such cases, I can use the `--filter=is-official` flag to only return 'official' images. Executing `podman search --filter=is-official alpine` cuts the previous list of 50 results down to one:
```bash
INDEX       NAME                       DESCRIPTION                                       STARS   OFFICIAL   AUTOMATED
docker.io   docker.io/library/alpine   A minimal Docker image based on Alpine Linux...   6793    [OK]     
``` 

If I wanted to see the full description, I can also include the `--no-trunc` flag. Executing `podman search --filter=-is-official --no-trunc alpine` returns:
```bash
INDEX       NAME                       DESCRIPTION                                                                                         STARS   OFFICIAL   AUTOMATED
docker.io   docker.io/library/alpine   A minimal Docker image based on Alpine Linux with a complete package index and only 5 MB in size!   6793    [OK]
```

"What is an official image?" you may ask. I had the same question and a strangely difficult time trying to find the answer. From what I can gather, an [official image](https://docs.docker.com/docker-hub/official_images/) is one that has gone through an extensive assurance process with the Docker Official Images team (which ultimately controls the repositories in which these images are published). The [docker search documentation](https://docs.docker.com/engine/reference/commandline/search/) notes that the `--is-official` flag looks for _'~images with the searched-for-name, at least 3 stars, and are official builds_'.  Based on some cursory searches I conducted, the returned results are always from the docker.io/library/ namespace, making me think that this must be a Docker-specific repository designation. Long-story short, I'm going to assume the image is ok if it looks something like `docker.io/libary/<image_name_here>`.

#### Search a specific registry for an image
Simply specify the registry before the image search term. For example, `podman search registry.redhat.io/postgresql-10` will only return results from the redhat registry:
```bash
INDEX       NAME                                           DESCRIPTION                                       STARS   OFFICIAL   AUTOMATED
redhat.io   registry.redhat.io/rhscl/postgresql-10-rhel7   PostgreSQL is an advanced Object-Relational ...   0
redhat.io   registry.redhat.io/rhel8/postgresql-10         This container image provides a containerize...   0
```

If we were to run the same command but to target the `docker.io` registry instead, we'd only get docker.io images. Running `podman search docker.io/postgresql-10 --limit 5` results in:
```bash
INDEX       NAME                                                      DESCRIPTION                                       STARS   OFFICIAL   AUTOMATED
docker.io   docker.io/centos/postgresql-10-centos7                    PostgreSQL is an advanced Object-Relational ...   18
docker.io   docker.io/crbrka/postgresql-10-postgis                    Postgresql-10 with PostGIS extension.             0
docker.io   docker.io/mcyprian/postgresql-10-fedora29                                                                   0
docker.io   docker.io/centos/postgresql-10-centos8                                                                      0
docker.io   docker.io/outtherelabs/postgresql-10-centos7-extensions   PostgreSQL Extensions Image                       0
```

#### Search a registry for ALL available images
I don't know why you wouldn't want to do this, but if you want to search for every possible available image on an image registry, you need to do the following:
1. Log in to the registry, (e.g. `podman login github.io`)
2. Execute your search, being sure to include a slash at the end (e.g. `podman search github.io/`)


#### Pulling images
Use the `podman pull` command to retrieve images from a remote registry. The command syntax looks like `podman pull <registry>[:<port>]/[<namespace>/]<name>:<tag>`.

While it is possible not to specify the registry, I think this is a good practice to follow as it makes it explicitly clear upfront where we are pulling our image from. For example, running `podman pull alpine` worked but I needed to check the image name to determien that I had pulled it from `docker.io/library/alpine` (the official Docker image). 

Rather than risk pulling the wrong image, I'll always be specifying the full image name during my initial pulls like `podman pull docker.io/library/alpine:latest`


### Interacting with local images
This section handles basic commands for interacting with images once you've copied them into your local image repository

#### List available images
To see the images in your local repository, type `podman images`. This will return something like:
```bash
REPOSITORY                TAG     IMAGE ID      CREATED       SIZE
docker.io/library/ubuntu  latest  9140108b62dc  2 days ago    75.3 MB
docker.io/library/alpine  latest  a24bb4013296  4 months ago  5.85 MB
```

##### Inspect on disk image
To see more details about the image we've downloaded, execute `podman inspect docker.io/library/alpine` (and pipe to `less` for easier reading). This will return something like:
```bash
[
    {
        "Id": "a24bb4013296f61e89ba57005a7b3e52274d8edd3ae2077d04395f806b63d83e",
        "Digest": "sha256:185518070891758909c9f839cf4ca393ee977ac378609f700f60a771a2dfe321",
        "RepoTags": [
            "docker.io/library/alpine:latest"
        ],
        "RepoDigests": [
            "docker.io/library/alpine@sha256:185518070891758909c9f839cf4ca393ee977ac378609f700f60a771a2dfe321",
            "docker.io/library/alpine@sha256:a15790640a6690aa1730c38cf0a440e2aa44aaca9b0e8931a9f2b0d7cc90fd65"
        ],
        "Parent": "",
        "Comment": "",
        "Created": "2020-05-29T21:19:46.363518345Z",
        "Config": {
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/sh"
            ]
        },
        "Version": "18.09.7",
        "Author": "",
        "Architecture": "amd64",
        "Os": "linux",
        "Size": 5849195,
        "VirtualSize": 5849195,
        "GraphDriver": {
            "Name": "overlay",
            "Data": {
                "UpperDir": "/home/deeplearning/.local/share/containers/storage/overlay/50644c29ef5a27c9a40c393a73ece2479de78325cae7d762ef3cdc19bf42dd0a/diff",
                "WorkDir": "/home/deeplearning/.local/share/containers/storage/overlay/50644c29ef5a27c9a40c393a73ece2479de78325cae7d762ef3cdc19bf42dd0a/work"
            }
        },
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:50644c29ef5a27c9a40c393a73ece2479de78325cae7d762ef3cdc19bf42dd0a"
            ]
        },
        "Labels": null,
        "Annotations": {},
        "ManifestType": "application/vnd.docker.distribution.manifest.v2+json",
        "User": "",
        "History": [
            {
                "created": "2020-05-29T21:19:46.192045972Z",
                "created_by": "/bin/sh -c #(nop) ADD file:c92c248239f8c7b9b3c067650954815f391b7bcb09023f984972c082ace2a8d0 in / "
            },
            {
                "created": "2020-05-29T21:19:46.363518345Z",
                "created_by": "/bin/sh -c #(nop)  CMD [\"/bin/sh\"]",
                "empty_layer": true
            }
        ],
        "NamesHistory": []
    }
]
```

Note that the $.GraphDriver key is of type 'overlay' and points to the directory we had specified in the `~/.config/containers/storage.conf` file (as part of the previous configuration effort).

Also of interest is the $.Config.Cmd key (and $.Config.Entrypoint if present). This lets us know that the container will drop you into a bash shell if you started it with `podman run` and did not overwrite it with a commandline argument. As another best practice, I'll always inspect the containers I download - even if they are official images - to ensure that nothing malicious immediately jumps out at me.  

The [Redhat documentation](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/building_running_and_managing_containers/working-with-container-images_building-running-and-managing-containers) has an interesting example involving `registry.redhat.io/rhel8/rsyslog` that involves an $.Labels.install value, but I've never encountered this workflow so I'll just make a note of it now and revisit later when I know more. 

#### Inspect image on remote registry
We installed Skopeo on our system for a reason and this was it! Instead of potentially wasted hundreds of MBs of download cap, just examine the image on the remote registry!
Using `skopeo inspect docker://docker.io/library/alpine` we get back a response like:
```bash
{
    "Name": "docker.io/library/alpine",
    "Digest": "sha256:185518070891758909c9f839cf4ca393ee977ac378609f700f60a771a2dfe321",
    "RepoTags": [
        "2.6",
        "2.7",
        "20190228",
        "20190408",
        "20190508",
        "20190707",
        "20190809",
        "20190925",
        "20191114",
        "20191219",
        "20200122",
        "20200319",
        "20200428",
        "20200626",
        "20200917",
        "3.1",
        "3.10.0",
        "3.10.1",
        "3.10.2",
        "3.10.3",
        "3.10.4",
        "3.10.5",
        "3.10",
        "3.11.0",
        "3.11.2",
        "3.11.3",
        "3.11.5",
        "3.11.6",
        "3.11",
        "3.12.0",
        "3.12",
        "3.2",
        "3.3",
        "3.4",
        "3.5",
        "3.6.5",
        "3.6",
        "3.7.3",
        "3.7",
        "3.8.4",
        "3.8.5",
        "3.8",
        "3.9.2",
        "3.9.3",
        "3.9.4",
        "3.9.5",
        "3.9.6",
        "3.9",
        "3",
        "edge",
        "latest"
    ],
    "Created": "2020-05-29T21:19:46.363518345Z",
    "DockerVersion": "18.09.7",
    "Labels": null,
    "Architecture": "amd64",
    "Os": "linux",
    "Layers": [
        "sha256:df20fa9351a15782c64e6dddb2d4a6f50bf6d3688060a34c4014b0d9a752eb4c"
    ],
    "Env": [
        "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
    ]
}
```
I'm not a seasoned Skopeo user yet, so I'm sure there is plenty more one can do. This command provides some basic details and allows you to see all the tagged versions of the image (in case you ever want something older than 'latest'). 

The Skopeo output looks different than what we saw when we inspected through podman, so I suspect we need to specify a version to inspect before we can replicate what we got from `podman inspect ...`.


#### Mount a volume to your container

#### Mount a running container to your local file system
This was something I didn't learn about during the training but saw during subsequent reading of the Red Hat docs. Apparently you can invoke a container in detached mode and then mount it your filesystem to explore it without having to connect to it via an `exec` command.

The Red Hat example:
```bash
# podman run -d registry.redhat.io/rhel8/rsyslog

# podman ps
CONTAINER ID IMAGE             COMMAND         CREATED        STATUS      PORTS NAMES
1cc92aea398d ...rsyslog:latest /bin/rsyslog.sh 37 minutes ago Up 1 day ago      myrsyslog

# podman mount 1cc92aea398d
/var/lib/containers/storage/overlay/65881e78.../merged

# ls /var/lib/containers/storage/overlay/65881e78*/merged
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
```

It looks like you can even check the installed packages once the container is mounted!
```bash
# rpm -qa --root=/var/lib/containers/storage/overlay/65881e78.../merged

redhat-release-server-7.6-4.el7.x86_64
filesystem-3.2-25.el7.x86_64
basesystem-10.0-7.el7.noarch
ncurses-base-5.9-14.20130511.el7_4.noarch
glibc-common-2.17-260.el7.x86_64
nspr-4.19.0-1.el7_5.x86_64
libstdc++-4.8.5-36.el7.x86_64
```