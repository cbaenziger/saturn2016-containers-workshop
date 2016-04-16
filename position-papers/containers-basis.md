# Basis of Containers

The [Open Container Initiative provides][Five Principles of Standard Containers] "the specification for Standard Containers defines":
````
"
configuration file formats
a set of standard operations
an execution environment
"
````
These principles however, read very similarly to existing projects and existing capabilities. How have they been adopted, versus how containers aim to be adopted?

## Systems Level Primitives:
At a systems level what is a container? If it is an isolated environment for code execution, deployment and change management, how is this new? One can look at the resources isolated: network, processes, user isolation, CPU capacity, disk space, bandwidth and file system changes (packaging).

The variety of Unix implementations for these techniques seem extensive:
* __Networking__: individual interfaces, pf(4)/tc(8), ip-netns(8)
* __Processes__: [FreeBSD jails], [zones][PSARC/2002/174], pid-namespaces
* __CPU Capacity__: ulimit, Solaris resource controls
* __Block Storage__: quotas, mount-namespaces; ionice(1), blkio cgroups control
* __File System Change__: [Image Packaging System] - pkg(8), Ubuntu [Snappy], Chef, Puppet, Ansible

The Unix kernel interface controls have evolved from simple multi-user isolation, in a single kernel and "view" of a system -- focusing on user system sharing -- to more advanced full-system isolation without resorting to VM's. Currently, many of the controls implement kernel level [tagging][PSARC/2002/174], [resource reference][FreeBSD jails] mechanisms to allow for isolation and of course the various [cgroups implementations] all as though separate OS images or kernels were in use. 

Other developments in the container space are packaging ideas. A key of containers is [defining][appc app container executor filesystem spec] all that goes into a container image. Previously, significant work has focused on configuration management to provide idempotent configuration (e.g. Puppet, Chef) with no guarantee of reversibility -- but that of reproducibility. Similarly, for easy deployment and de-installation, single application deployment formats such as JARs and WARs have come about. Even more reproducible and bounded in affect are transactional packaging systems. Unlike the above which do not encapsulate an entire machine's configuration including disks, networks and bits on disk. VM packaging formats such as [OVF] and container packaging formats do [look][OCF hook developers] to tackle this broad meta level. Further, [community ecosystems][Docker Hub] of container images; a pattern seen with other [projects][Chef Supermarket].

Many propose containers are an easy way to ensure an application is redeployed afresh via a scorched earth removal of old versions. Packaging systems however have been providing transactional packaging for sometime though, e.g. through [IPS actions] or [package sandbox location][Snappy]. How does the rigor of packaging provide such a hindrance for containers to succeed better in deploying software?

## User Interface:

What makes containers easier to use than the various low-level predecessors they abstract? Here let us chiefly focus on Docker and Rkt or similarly automated [Solaris container][Solaris zone AI configuration] creation. The Open Container Initiative proposes standard operations and one can find Docker provides such operations in its [API][Docker API] (47 calls):
* Containers
  * List containers
  * Create a container
  * Inspect a container
  * Start a container
  * Stop a container
  * Restart a container
  * Kill a container
  * \[More...\]
* Images
  * Build image from a Dockerfile
  * Create an image
  * \[More...\]
* Misc
  * Create a new image from a container’s changes
  * Load a tarball with a set of images and tags into docker
  * Exec Create
  * \[More...\]

Containers propose to provide an easily automated configuration of many system components. This compared to [plain cgroups] is much easier to operate concisely abstracting the intricacies of low-level configuration. Further, deployment choices for containers provide CLI operations, not requiring one to build an [operation stack in a configuration management tool][chef-cgroups]. However, what role do deployment platforms for containers play? For example, one can deploy Docker images via Apache Mesos, Apache YARN, Docker Swarm and Google Container Engine. These offerings then beg the question of orchestration and if previous systems such as [Amazon CloudFormation] or [OpenStack Heat] can properly offer semantics for container deployment -- or are they obviated?

[Amazon CloudFormation]: http://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/Welcome.html
[OpenStack Heat]: https://wiki.openstack.org/wiki/Heat
[appc app container executor filesystem spec]: https://github.com/appc/spec/blob/530d24f228d3ed0003f4397d1f861f3611ea233f/spec/ace.md#filesystem-setup
[cgroups implementations]: http://www.haifux.org/lectures/299/netLec7.pdf
[FreeBSD jails]: https://docs.freebsd.org/44doc/papers/jail/jail-6.html
[plain cgroups]: https://www.kernel.org/doc/Documentation/cgroup-v1/cgroups.txt
[PSARC/2002/174]: https://us-east.manta.joyent.com/jmc/public/opensolaris/ARChive/PSARC/2002/174/zones-design.spec.opensolaris.pdf
[Five Principles of Standard Containers]: https://github.com/opencontainers/runtime-spec/blob/4dfd127f0414213f6a34e604091dcc8a3d8fa504/principles.md
[Snappy]: http://www.markshuttleworth.com/archives/1434
[IPS actions]: https://java.net/projects/ips/sources/pkg-gate/content/doc/actions.txt
[Image Packaging System]: http://www.oug.org/files/presentations/ips-losug.pdf
[OVF]: https://www.dmtf.org/standards/ovf
[OCF hook developers]: https://github.com/opencontainers/runtime-spec/blob/3eeb4ff068954540987d17c13fba354ff2b82725/README.md#hook-developers
[Solaris zone AI configuration]: https://docs.oracle.com/cd/E23824_01/html/E21798/glitd.html#ngz-ai-manifest
[Docker Hub]: https://hub.docker.com/
[Chef Supermarket]: https://supermarket.chef.io/
[chef-cgroups]: https://github.com/nathenharvey/chef-cgroups
[Docker API]: https://docs.docker.com/engine/reference/api/docker_remote_api_v1.20/
