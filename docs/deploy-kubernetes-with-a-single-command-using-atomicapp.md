# Deploy Kubernetes with a Single Command Using Atomicapp

*Modified under MIT license from Project Atomic blog article [Deploy Kubernetes with a Single Command Using Atomicapp](http://www.projectatomic.io/blog/2015/08/fun-with-kubenetes-and-atomicapp/) ([source](https://github.com/projectatomic/atomic-site/blob/master/source/blog/2015-08-17-fun-with-kubenetes-and-atomicapp.html.md)) and relicensed under CC BY SA 3.0 Unported*

Kubernetes, the open source orchestration system for Docker containers, is a fast-moving project that can be somewhat complicated to install and configure, especially if you're just getting started with it.

For this document we're going to focus on getting installation down to a single command:

```
# atomic run jasonbrooks/kubernetes-atomicapp
```

The first part of the command, `atomic`, refers to the containerized application and system management tool that ships with all [Atomic Hosts](http://www.projectatomic.io/download/) and that allows an image provider to specify how a container image expects to be run. This tool takes advantage of the [support for adding custom metadata](https://docs.docker.com/userguide/labels-custom-metadata/), through labels, that debuted in docker 1.6.0.

If you aren't using an Atomic Host, the `atomic` tool is available in the CentOS 7 repository. Issue a `yum install atomic` command to get one of your own.

You can read all about atomic in [this post](http://developerblog.redhat.com/2015/04/21/introducing-the-atomic-command/) from Dan Walsh, but the short version is that labels allow image developers to fold their big-hairy-docker-run-commands into their Dockerfiles, and the atomic tool allows users to access these labels.

At the end of the command, we have `atomicapp`, which is a tool for packaging complex multi-container applications and services, along with their orchestration metadata, into a deployable container image. Atomicapp is a reference implementation of the [Nulecule specification](https://github.com/projectatomic/nulecule). 

You can learn more about Nulecule from one of [these](http://www.projectatomic.io/blog/2015/05/announcing-the-nulecule-specification-for-composite-applications/) [fine](http://rhelblog.redhat.com/2015/05/15/the-atomic-app-concept-it-all-starts-when-a-nulecule-comes-out-of-its-nest/) [blog posts](http://rhelblog.redhat.com/2015/06/23/announcing-yum-rpm-for-containerized-applications-nulecule-atomic-app/), or read on as we create our own Atomic App.

## kubernetes-atomicapp

Atomic Apps are pretty easy to build. The process consists of gathering up orchestration metadata from whichever providers you intend to support (`atomicapp` currently works with Kubernetes, Docker, and OpenShift), placing this metadata in the right directory structure, and filling out a Nulecule file with information about the images, parameters, and metadata you wish to use.

The Nulecule repository contains a project template that we can use for reference:

```
$ git clone https://github.com/projectatomic/nulecule.git
$ tree nulecule/spec/examples/
nulecule/spec/examples/
└── template
    ├── artifacts
    │   ├── provider1
    │   │   ├── pod.json
    │   │   └── service.json
    │   └── provider2
    │       └── file.json
    ├── Dockerfile
    └── Nulecule
```

For our Kubernetes app, we'll be using the Docker provider, with one metadata file for each of the three `docker run` commands we mentioned above:

```
$ mkdir -p nulecule/examples/kubernetes-atomicapp/artifacts/docker
$ echo "docker run --net=host -d gcr.io/google_containers/etcd:2.0.9 /usr/local/bin/etcd --addr=127.0.0.1:4001 --bind-addr=0.0.0.0:4001 --data-dir=/var/etcd/data" > nulecule/examples/kubernetes-atomicapp/artifacts/docker/kube-etcd_run
$ echo "docker run --net=host -d -v /var/run/docker.sock:/var/run/docker.sock gcr.io/google_containers/hyperkube:v0.21.2 /hyperkube kubelet --api_servers=http://localhost:8080 --v=2 --address=0.0.0.0 --enable_server --hostname_override=127.0.0.1 --config=/etc/kubernetes/manifests" > nulecule/examples/kubernetes-atomicapp/artifacts/docker/kube-master_run
$ echo "docker run -d --net=host --privileged gcr.io/google_containers/hyperkube:v0.21.2 /hyperkube proxy --master=http://127.0.0.1:8080 --v=2" > nulecule/examples/kubernetes-atomicapp/artifacts/docker/kube-svcproxy_run
```

Next, we need to customize our Nulecule file:

```
$ vi nulecule/examples/kubernetes-atomicapp/Nulecule

---
specversion: 0.0.2
id: kubernetes-atomicapp

metadata:
  name: Kubernetes Atomic App
  appversion: 0.0.1
  description: Atomic app for deploying a local, dockerized kubernetes
graph:
  - name: kubernetes-atomicapp
    artifacts:
      docker:
        - file://artifacts/docker/kube-etcd_run
        - file://artifacts/docker/kube-master_run
        - file://artifacts/docker/kube-svcproxy_run
```

Next, we'll create an `answers.conf` file to ensure that docker is picked up as our default provider -- otherwise, atomicapp will default to kubernetes.

```
$ vi nulecule/examples/kubernetes-atomicapp/answers.conf

[general]
namespace = default
provider = docker
```

Finally we need a Dockerfile, which we can copy, mostly unchanged, from the Nulecule template. We need to modify the Nulecule providers label to list Docker as our provider, and add in the answer file we created above:

```
$ cp nulecule/spec/examples/template/Dockerfile nulecule/examples/kubernetes-atomicapp/
$ vi nulecule/examples/kubernetes-atomicapp/Dockerfile

FROM projectatomic/atomicapp:0.1.3

MAINTAINER Your Name <email@example.com>

LABEL io.projectatomic.nulecule.specversion 0.0.2
LABEL io.projectatomic.nulecule.providers = "docker"

ADD /Nulecule /Dockerfile /answers.conf /application-entity/
ADD /artifacts /application-entity/artifacts
```

Now we can pack up our metadata, Nulecule file, and Dockerfile into an image:

```
# docker build -t YOURNAME/kubernetes-atomicapp nulecule/examples/kubernetes-atomicapp/.
```

You can now run your image, push it to a Docker registry to pull and run somewhere else, or you can just run this image already pushed to the Docker hub:

```
# atomic run jasonbrooks/kubernetes-atomicapp
```

*NOTE: Running Kubernetes in this way involves providing a container with access to the Docker daemon on the host machine, which SELinux prevents, so you'll need to put SELinux into permissive mode (`sudo setenforce 0`) for this experiment.*

*CAUTION: Running a production (non-experiment) machine in permissive mode is a serious security risk and should be avoided.*

From here, you can pick up with the original Kubernetes-on-Socker walkthrough, at the [**Test it out**](https://github.com/GoogleCloudPlatform/kubernetes/blob/release-1.0/docs/getting-started-guides/docker.md#test-it-out) step.
