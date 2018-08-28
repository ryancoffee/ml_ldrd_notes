# Kubernetes
## Contact David Aronchik at Google
David Aronchick <aronchick@google.com>

## Initial Setup by Omar/sys admin
first add the kubernetes key to the .kube/config file.
Then added to the acess file for psgru/pslucy/psjerry&pskevin
Ultil we have a group, then the group access would propagate.

-bash-4.2$ kubectl get nodes
NAME      STATUS    ROLES     AGE       VERSION
psgru     Ready     master    14d       v1.11.1
psjerry   Ready     <none>    14d       v1.11.1
pskevin   Ready     <none>    14d       v1.11.1
-bash-4.2$ 


## Building your Docker image

-bash-4.2$ ssh pslucy
-bash-4.2$ 
-bash-4.2$ 
-bash-4.2$ 
-bash-4.2$ docker images
REPOSITORY               TAG                 IMAGE ID            CREATED             SIZE
pshub01:5000/devtools7   latest              85991029056d        42 hours ago        2.24GB
centos7                  latest              30683c16b836        44 hours ago        1.96GB
centos                   latest              5182e96772bf        2 weeks ago         200MB
-bash-4.2$ 
-bash-4.2$ 
-bash-4.2$ df -lh
Filesystem            Size  Used Avail Use% Mounted on
/dev/mapper/vg0-root   20G  2.9G   18G  15% /
devtmpfs               63G     0   63G   0% /dev
tmpfs                  63G     0   63G   0% /dev/shm
tmpfs                  63G   19M   63G   1% /run
tmpfs                  63G     0   63G   0% /sys/fs/cgroup
/dev/mapper/vg0-tmp    10G   33M   10G   1% /tmp
/dev/mapper/vg0-var   200G  3.3G  197G   2% /var
/dev/md0             1019M  173M  846M  17% /boot
tmpfs                  13G     0   13G   0% /run/user/14282
tmpfs                  13G     0   13G   0% /run/user/9203
-bash-4.2$ docker images -a
REPOSITORY               TAG                 IMAGE ID            CREATED             SIZE
pshub01:5000/devtools7   latest              85991029056d        42 hours ago        2.24GB
<none>                   <none>              8506aa26c850        42 hours ago        2.24GB
<none>                   <none>              5c1444f16975        42 hours ago        1.96GB
centos7                  latest              30683c16b836        44 hours ago        1.96GB
<none>                   <none>              309d63cfe020        44 hours ago        1.79GB
<none>                   <none>              0a06c7d7534b        44 hours ago        1.21GB
<none>                   <none>              e0a81adf9c07        45 hours ago        1.02GB
<none>                   <none>              bff337bc1a2a        45 hours ago        944MB
<none>                   <none>              41350a071be6        45 hours ago        200MB
<none>                   <none>              b8edc66f0e6e        45 hours ago        200MB
<none>                   <none>              3dac43a5a6f3        45 hours ago        200MB
<none>                   <none>              36740ecdf5e5        45 hours ago        200MB
centos                   latest              5182e96772bf        2 weeks ago         200MB
-bash-4.2$ 



### Build then push to local Docker registry

pshub01 is the temporary Docker registry.

-bash-4.2$ hostname
pskube-lucy
-bash-4.2$ pwd
/reg/neh/home/coffee
-bash-4.2$ 
-bash-4.2$ 
-bash-4.2$ 
-bash-4.2$ docker images
REPOSITORY               TAG                 IMAGE ID            CREATED             SIZE
pshub01:5000/devtools7   latest              85991029056d        42 hours ago        2.24GB
centos7                  latest              30683c16b836        45 hours ago        1.96GB
centos                   latest              5182e96772bf        2 weeks ago         200MB
-bash-4.2$ docker images -a
REPOSITORY               TAG                 IMAGE ID            CREATED             SIZE
pshub01:5000/devtools7   latest              85991029056d        42 hours ago        2.24GB
<none>                   <none>              8506aa26c850        42 hours ago        2.24GB
<none>                   <none>              5c1444f16975        42 hours ago        1.96GB
centos7                  latest              30683c16b836        45 hours ago        1.96GB
<none>                   <none>              309d63cfe020        45 hours ago        1.79GB
<none>                   <none>              0a06c7d7534b        45 hours ago        1.21GB
<none>                   <none>              e0a81adf9c07        45 hours ago        1.02GB
<none>                   <none>              bff337bc1a2a        45 hours ago        944MB
<none>                   <none>              36740ecdf5e5        45 hours ago        200MB
<none>                   <none>              b8edc66f0e6e        45 hours ago        200MB
<none>                   <none>              3dac43a5a6f3        45 hours ago        200MB
<none>                   <none>              41350a071be6        45 hours ago        200MB
centos                   latest              5182e96772bf        2 weeks ago         200MB
-bash-4.2$ 


OK, here the <none> that show with docker images -a shows all, even <none> that are valid base layers for the diff builds.
docker images (without -a) then any <none> is a dead sub-layer.

pshub01:5000/devtools7 
When building the final to deploy, use the prefix pshub01:5000/ and that pushes to the pshub01 local registry onto port 5000 which is open and listening for docker pushes.

### Check registry is listening

Check quickly that the registry server is up and listening...
-bash-4.2$ 
-bash-4.2$ telnet pshub01 5000
Trying 172.21.49.203...
Connected to pshub01.
Escape character is '^]'.
^]
telnet> quit
Connection closed.
-bash-4.2$ 

-bash-4.2$ vim Dockerfile.rnc_addon
-bash-4.2$ 
-bash-4.2$ 
-bash-4.2$ 
-bash-4.2$ hostname
pskube-lucy
-bash-4.2$ docker build --rm -t pshub01:5000/simdev_rnc -f Dockerfile.rnc_addon .


Dependency Installed:
gpm-libs.x86_64 0:1.20.7-5.el7           vim-common.x86_64 2:7.4.160-4.el7   
vim-filesystem.x86_64 2:7.4.160-4.el7   

Complete!
Removing intermediate container 58318f781bbd
---> f93de58ab4e8
Step 5/5 : RUN debuginfo-install -y fftw-libs-double-4.3.3-8.el7.x86_64 glibc-2.17-196.el7_4.2.x86_64 libgcc-4.8.5-16.el7_4.2.x86_64 libgomp-4.8.5-16.el7_4.2.x86_64 libstdc++-4.8.5-16.el7_4.2.x86_64
---> Running in 1eca3ce4e7f2
Loaded plugins: fastestmirror, ovl
enabling epel-debuginfo
enabling centos-sclo-rh-debuginfo
enabling base-debuginfo
enabling centos-sclo-sclo-debuginfo
Could not find a package for: libstdc++-4.8.5-16.el7_4.2.x86_64
No debuginfo packages available to install
Removing intermediate container 1eca3ce4e7f2
---> be067ebdc971
Successfully built be067ebdc971
Successfully tagged pshub01:5000/simdev_rnc:latest
-bash-4.2$ docker images
REPOSITORY                TAG                 IMAGE ID            CREATED              SIZE
pshub01:5000/simdev_rnc   latest              be067ebdc971        About a minute ago   2.6GB
pshub01:5000/devtools7    latest              85991029056d        42 hours ago         2.24GB
centos7                   latest              30683c16b836        45 hours ago         1.96GB
centos                    latest              5182e96772bf        2 weeks ago          200MB
-bash-4.2$ 

Complete!
Removing intermediate container 7604f048cc6f
---> 21723bfb1c24
Successfully built 21723bfb1c24
Successfully tagged pshub01:5000/simdev_rnc:latest
-bash-4.2$ docker images
REPOSITORY                TAG                 IMAGE ID            CREATED             SIZE
pshub01:5000/simdev_rnc   latest              21723bfb1c24        20 seconds ago      3.73GB
<none>                    <none>              be067ebdc971        7 minutes ago       2.6GB
pshub01:5000/devtools7    latest              85991029056d        43 hours ago        2.24GB
centos7                   latest              30683c16b836        45 hours ago        1.96GB
centos                    latest              5182e96772bf        2 weeks ago         200MB


Now there is a danlgling <none> that we should remove.

-bash-4.2$ docker images
REPOSITORY                TAG                 IMAGE ID            CREATED              SIZE
pshub01:5000/simdev_rnc   latest              21723bfb1c24        About a minute ago   3.73GB
<none>                    <none>              be067ebdc971        9 minutes ago        2.6GB
pshub01:5000/devtools7    latest              85991029056d        43 hours ago         2.24GB
centos7                   latest              30683c16b836        45 hours ago         1.96GB
centos                    latest              5182e96772bf        2 weeks ago          200MB
-bash-4.2$ docker rmi be067ebdc971
Deleted: sha256:be067ebdc971f1f356d9d9abd72f76c3077b4e48cc59fa77f899f26cb581905f
Deleted: sha256:42973fcf7a6489ff4eb3616f129389de99a1e5be93c78fc8bac2f2babcfb5a56
-bash-4.2$ docker images
REPOSITORY                TAG                 IMAGE ID            CREATED              SIZE
pshub01:5000/simdev_rnc   latest              21723bfb1c24        About a minute ago   3.73GB
pshub01:5000/devtools7    latest              85991029056d        43 hours ago         2.24GB
centos7                   latest              30683c16b836        45 hours ago         1.96GB
centos                    latest              5182e96772bf        2 weeks ago          200MB
-bash-4.2$ 




-bash-4.2$ docker push pshub01:5000/simdev_rnc
The push refers to repository [pshub01:5000/simdev_rnc]
481a2fce3e29: Pushed 
931591c7d862: Pushed 
bc1c946afc09: Pushed 
3cf23e952566: Mounted from devtools7 
84ae2b6e217a: Mounted from devtools7 
81e68f0db326: Mounted from devtools7 
3779804ec1d1: Mounted from devtools7 
c8ee9ef009c6: Mounted from devtools7 
de2254633fb9: Mounted from devtools7 
1d31b5806ba4: Mounted from devtools7 
latest: digest: sha256:138f1bd58ccc062e510d6a26202fb37d4385d364c3ca119245d42e689f8c2d2a size: 2436
-bash-4.2$ 




if already running
-bash-4.2$ kubectl get pods
NAME        READY     STATUS    RESTARTS   AGE
simdevrnc   1/1       Running   0          7m
-bash-4.2$ kubectl delete -f simdevrnc
error: the path "simdevrnc" does not exist
-bash-4.2$ kubectl delete -f simdev_rnc.yaml 
pod "simdevrnc" deleted


-bash-4.2$ kubectl get pods
No resources found.
-bash-4.2$ kubectl create -f simdev_rnc.yaml 
pod/simdevrnc created
-bash-4.2$ kubectl get pods
NAME        READY     STATUS              RESTARTS   AGE
simdevrnc   0/1       ContainerCreating   0          22s
-bash-4.2$ kubectl get pods
NAME        READY     STATUS    RESTARTS   AGE
simdevrnc   1/1       Running   0          23s
-bash-4.2$ kubectl get pods
NAME        READY     STATUS    RESTARTS   AGE
simdevrnc   1/1       Running   0          25s
-bash-4.2$ 
-bash-4.2$ 
-bash-4.2$ 
-bash-4.2$ kubectl describe pods simdevrnc
Name:               simdevrnc
Namespace:          default
Priority:           0
PriorityClassName:  <none>

-bash-4.2$ 
-bash-4.2$ 
-bash-4.2$ kubectl exec -ti simdevrnc -- scl enable devtoolset-7 bash
[root@simdevrnc /]# gcc --version
gcc (GCC) 7.3.1 20180303 (Red Hat 7.3.1-5)
Copyright (C) 2017 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

[root@simdevrnc /]# 


apparently need to open the proxy within kubernetes as well as in docker





## Good Form For Docker Recipies 


OK, typically, try to execute the full installation of a given package as a single RUN command for Docker 
This allows it to handle incremental diff so that you don't always have to rebuild the wh0ole stack.





-bash-4.2$ less ~omarq/docker/Dockerfile.centos7 
-bash-4.2$ 


FROM centos
LABEL maintainer "Omar E. Quijano <omarq@slac.stanford.edu>"
ENV http_proxy http://psproxy:3128
ENV https_proxy https://psproxy:3128
ENV ftp_proxy $http_proxy

#INSTALL PACKAGES
RUN yum -y update && yum install -y \
	    epel-release \
	    @development \
	    zlib-devel \
	    openssl-devel \
	    openssl \
	    texlive \
	    atlas \
	    fftw \
	    fftw-devel \
	    fftw-static \
	    gnuplot \
	    gsl-devel \
	    hdf5 \
	    lapack \
	    numpy \
	    scipy \
	    unit \
	    lzip  \
	    opencv \
	    opencv-devel \
	    mlocate \
	    opencv-devel \
	    dkms \
	    libvdpau \
	    libffi-devel \
	    wget &&\
	    yum -y clean all

#PYTHON 3.7
RUN curl -L https://www.python.org/ftp/python/3.7.0/Python-3.7.0.tgz | tar -xz -C /root/
RUN cd /root/Python-3.7.0 && ./configure --enable-optimizations && make altinstall \
	&& rm -rf /root/Python*

#BOOST:
RUN curl -L https://dl.bintray.com/boostorg/release/1.68.0/source/boost_1_68_0.tar.gz | tar -xz -C /root/
RUN cd /root/boost_1_68_0 && ./bootstrap.sh --prefix=/opt/boost && ./b2 install --prefix=/opt/boost \
	&& rm -rf /root/boost*




-bash-4.2$ docker inspect centos7
[
{
	"Id": "sha256:30683c16b8368a9b6.....2c0c322e6d3e83083d",
		"RepoTags": [
			"centos7:latest"
		],
		"RepoDigests": [],
		"Parent": "sha256:309d63cfe54034bda99......ded385cdb9d81d633",
		"Comment": "",
		"Created": "2018-08-19T21:24:08.911814257Z",
		"Container": "5f1fe009a7a12a809c4aa97b84dfoksajfdo7c8bcc8f52e49def0ca812081be7a",
		"ContainerConfig": {
			"Hostname": "",
			"Domainname": "",
			"User": "",
			"AttachStdin": false,
			"AttachStdout": false,
			"AttachStderr": false,
			"Tty": false,
			"OpenStdin": false,
			"StdinOnce": false,
			"Env": [
				"PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
			"http_proxy=http://psproxy:3128",
			"https_proxy=https://psproxy:3128",
			"ftp_proxy=http://psproxy:3128"
			],
			"Cmd": [
				"/bin/sh",
			"-c",
			"cd /root/boost_1_68_0 && ./bootstrap.sh --prefix=/opt/boost && ./b2 install --prefix=/opt/boost \t&& rm -rf /root/boost*"
			],
			"ArgsEscaped": true,
			"Image": "sha256:309d63cfe020af18f8b.....d633",
			"Volumes": null,
			"WorkingDir": "",
			"Entrypoint": null,
			"OnBuild": null,
			"Labels": {
				"maintainer": "Omar E. Quijano <omarq@slac.stanford.edu>",
				"org.label-schema.build-date": "20180804",
				"org.label-schema.license": "GPLv2",
				"org.label-schema.name": "CentOS Base Image",
				"org.label-schema.schema-version": "1.0",
				"org.label-schema.vendor": "CentOS"
			}
		},
		"DockerVersion": "18.06.0-ce",
		"Author": "",
		"Config": {
			"Hostname": "",
			"Domainname": "",
			"User": "",
			"AttachStdin": false,
			"AttachStdout": false,
			"AttachStderr": false,
			"Tty": false,
			"OpenStdin": false,
			"StdinOnce": false,
			"Env": [
				"PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
			"http_proxy=http://psproxy:3128",
			"https_proxy=https://psproxy:3128",
			"ftp_proxy=http://psproxy:3128"
			],
			"Cmd": [
				"/bin/bash"
			],
			"ArgsEscaped": true,
			"Image": "sha256:309d63cfe020af18f8bb307.....81d633",
			"Volumes": null,
			"WorkingDir": "",
			"Entrypoint": null,
			"OnBuild": null,
			"Labels": {
				"maintainer": "Omar E. Quijano <omarq@slac.stanford.edu>",
				"org.label-schema.build-date": "20180804",
				"org.label-schema.license": "GPLv2",
				"org.label-schema.name": "CentOS Base Image",
				"org.label-schema.schema-version": "1.0",
				"org.label-schema.vendor": "CentOS"
			}
		},
		"Architecture": "amd64",
		"Os": "linux",
		"Size": 1959602881,
		"VirtualSize": 1959602881,
		"GraphDriver": {
			"Data": {
				"LowerDir": "/var/lib/docker/overlay2/1102791745c694f06bda0d29a1d6efed22b822fea6f1e0d6322c5be52070a434/diff:/var/lib/docker/overlay2/5535226a49db6336ac978193d89a4e16be55e78f55afb2c6806155b31023bc25/diff:/var/lib/docker/overlay2/f5a2244fdd7ca8bc44c0b37abce0f73d7d43d88859b98da5cd64435accf2c676/diff:/var/lib/docker/overlay2/9d8092b4a4ed134f600f50e046959300cddabe758778b46894493116ceb7528a/diff:/var/lib/docker/overlay2/93ca0f81e5f4adbd43c1c4899e08deb8e648b37d9377d0810b7fd01e8bc7c270/diff",
				"MergedDir": "/var/lib/docker/overlay2/99ffa5c80f02a7c6ab462adf02ee8863163756c841817faba60f3ce2f20292d1/merged",
				"UpperDir": "/var/lib/docker/overlay2/99ffa5c80f02a7c6ab462adf02ee8863163756c841817faba60f3ce2f20292d1/diff",
				"WorkDir": "/var/lib/docker/overlay2/99ffa5c80f02a7c6ab462adf02ee8863163756c841817faba60f3ce2f20292d1/work"
			},
			"Name": "overlay2"
		},
		"RootFS": {
			"Type": "layers",
			"Layers": [
				"sha256:1d31b5806ba40b5f67bde96f18a181668348934a44c92......f04cfb4e37a",
			"sha256:de2254633fb9104f1cf2edbfadc8acc36e5db689a2a5793b553034d9cf2a",
			"sha256:c8ee9ef009c61057d6ce05fc4b91f546de01.....6a38d325a827d8855d23cb",
			"sha256:3779804ec1d1d08ebc7eb0385c93fb25cd223f66.....271a089276ec2a2486",
			"sha256:81e68f0db326c6b472d6805903768ca4570426d4983b.....ebaddb7ebd17f4",
			"sha256:84ae2b6e217a28bb3be938bb58be6a820ca14cb4057a5b17......aa35f107b3"
			]
		},
		"Metadata": {
			"LastTagTime": "2018-08-19T14:24:10.976004687-07:00"
		}
}
]
-bash-4.2$ 

