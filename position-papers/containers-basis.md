# Basis of Containers

The Open Container Initiative provides "the specification for Standard Containers defines":

````
"
configuration file formats
a set of standard operations
an execution environment
"
````
[Five Principles of Standard Containers]

These principles however, read very similarly to existing projects and existing capabilities.

## Systems level:
At a systems level what is a container? If it is an isolated environment for code execution, deployment and change management, how is this new? One can look at the resources isolated: network, processes, user isolation, CPU capacity, disk space, bandwidth and file system changes (packaging).

The variety of Unix implementations for these techniques seem extensive:
* __Networking__: individual interfaces, pf(4)/tc(8), ip-netns(8)
* __Processes__: [FreeBSD jails], zones, pid-namespaces
* __CPU Capacity__: ulimit, Solaris resource controls
* __Block Storage__: quotas, mount-namespaces; ionice(1), blkio cgroups control
* __File System Change__: [Image Packaging System] - pkg(8), Ubuntu [Snappy], Chef, Puppet, Ansible

The Unix kernel interface controls have evolved from simple multi-user isolation, in a single kernel and "view" of a system -- focusing on user system sharing -- to more advanced full-system isolation without resorting to VM's. Currently, many of the controls implement kernel level [Tagging], [resource refrence] mechanisms to allow for isolation and of course [plain cgroups] all as though separate OS images or kernels were in use. 

Other developments in the container space are packaging ideas. Previously significant work has focused on configuration management (Puppet, Chef), single application deployment formats (WARs, JARs) and now transactional packaging; however, different than VM packaging formats such as OVF, container packaging formats look to tackle a different meta level again; but do they?

Many propose Docker is a very easy way to ensure an application is redeployed afresh via a scortched earth removal of old versions. Packaging systems have been providing transactional packaging for sometime though, e.g. through [IPS actions] or [package sandbox location]. How does the rigor of packaging provide such a hinderance for Docker to succeed better?

[FreeBSD jails]: jail, section 6: https://docs.freebsd.org/44doc/papers/jail/jail-6.html
[resource reference]: jail, section 6: https://docs.freebsd.org/44doc/papers/jail/jail-6.html

[plain cgroups]: cgroups - https://www.kernel.org/doc/Documentation/cgroup-v1/cgroups.txt

[Tagging]: zones PSARC/2002/174 - https://us-east.manta.joyent.com/jmc/public/opensolaris/ARChive/PSARC/2002/174/zones-design.spec.opensolaris.pdf

[Five Principles of Standard Containers]: "The 5 principles of Standard Containers" https://github.com/opencontainers/runtime-spec/blob/4dfd127f0414213f6a34e604091dcc8a3d8fa504/principles.md

[Snappy]: http://www.markshuttleworth.com/archives/1434

[IPS actions]: https://java.net/projects/ips/sources/pkg-gate/content/doc/actions.txt

[Image Packaging System]: http://www.oug.org/files/presentations/ips-losug.pdf
