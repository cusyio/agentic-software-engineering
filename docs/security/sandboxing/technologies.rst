.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Sandboxing technologies
=======================

Different sandboxing technologies strike different balances between the four key
factors: security isolation, performance, start-up speed and compatibility. No
technology is perfect – each involves different trade-offs.

MicroVMs
--------

MicroVMs offer the highest level of security isolation. They use hardware
virtualisation to assign each sandbox its own kernel, its own memory space and
its own virtual devices. This creates a hardware-based boundary between guest
code and the host operating system, thereby avoiding the security
vulnerabilities associated with containers due to the shared kernel. The most
significant innovation is significantly faster start-up times, making MicroVMs
suitable even for short-lived workloads. As a result, they have largely rendered
obsolete the old dichotomy of slow, secure VMs versus fast, insecure containers,
as they combine the best of both worlds and have become the new standard for
secure, short-lived code execution.

.. _firecracker-microvm:

Firecracker
~~~~~~~~~~~

`Firecracker <https://firecracker-microvm.github.io>`_ was developed by
:abbr:`AWS (Amazon Web Services)` and released as an open-source project. The
Virtual Machine Monitor (VMM) uses :abbr:`KVM (Linux Kernel Virtual Machine)` to
create and manage resource-efficient micro-VMs. Its design philosophy is
minimalist: it deliberately omits all non-essential devices such as USB
controllers, graphics cards and sound cards, which drastically reduces the
potential attack surface and lowers the memory requirements of each micro-VM to
less than 5 MB.

Firecracker runs as a process in user space on a host machine and is controlled
via a `RESTful API <https://en.wikipedia.org/wiki/REST>`_. This API enables the
programmatic configuration of the micro-VM, including specifying the number of
vCPUs, the memory size, and the attachment of network interfaces or block
devices. This API-driven approach is crucial for automating the lifecycle of
sandboxes in cloud-native applications.

Firecracker’s standout feature is its start-up speed: the micro-VM can start up
in just 125 milliseconds and execute code in user space. This bridges the gap
between the slow start-up times of traditional VMs—often several seconds—and the
rapid start-up of containers, making it suitable for high-throughput, on-demand
workloads such as serverless functions. This capability enables services such as
`AWS Lambda <https://aws.amazon.com/lambda/>`_, `AWS Fargate
<https://aws.amazon.com/fargate/>`_, :ref:`e2b` and `Fly.io <https://fly.io/>`_
to provision isolated execution environments at scale.

To achieve `defence-in-depth
<https://en.wikipedia.org/wiki/Defense_in_depth_(computing)>`_, Firecracker uses
the `Jailer <https://github.com/firecracker-microvm/firecracker/blob/main/docs/jailer.md>`_
process, which utilises Linux `cgroups <https://en.wikipedia.org/wiki/Cgroups>`_
and namespaces to establish a secure environment in order to isolate the
:abbr:`VMM (Virtual Machine Monitor)` process itself. This provides a second
layer of security in the event that the virtualisation barrier is compromised.

.. _libkrun:

libkrun
~~~~~~~

`libkrun <https://github.com/libkrun/libkrun>`_ is a virtualisation solution
designed to create resource-efficient, :abbr:`KVM (Kernel Virtual
Machine)`-based virtual machines with minimal overhead. It forms the core
technology behind `microsandbox
<https://docs.microsandbox.dev/getting-started/introduction>`_. It enables
applications to embed secure sandboxing capabilities directly, thereby achieving
true hardware-level isolation with its own kernel and memory space, whilst
start-up times can compete with those of containers.

In addition to microsandbox, libkrun is used by `Podman <https://podman.io/>`_
and `crun <https://github.com/containers/crun>`_.

Containerisation – Isolation based on namespaces
------------------------------------------------

Containerisation is the most widely used approach to isolating applications. It
utilises Linux `namespaces <https://en.wikipedia.org/wiki/Linux_namespaces>`_
and `control groups (cgroups) <https://en.wikipedia.org/wiki/Cgroups>`_ to
create isolated environments that share the host kernel. Although containers do
not offer the strictest security boundaries, they do strike a good balance
between compatibility, performance and ease of use.

Docker-/OCI-Container
~~~~~~~~~~~~~~~~~~~~~

`Docker <https://www.docker.com>`_ and the broader ecosystem of the `Open
Container Initiative (OCI) <https://opencontainers.org>`_  represent the de
facto standard for containerisation. Containers bundle applications with their
dependencies whilst ensuring isolation at the process level through kernel
features.

Containers utilise several Linux kernel features for isolation:

Namespaces
    such as :abbr:`PID (Process Identifier)`, Mount, Network, User, :abbr:`UTS
    (Unix Time Sharing)` and :abbr:`IPC (Inter-process communication)` separate
    process trees, file systems and network stacks from one another
Control Groups
    limit and monitor the use of CPU, memory and I/O resources
Security modules
    `AppArmor <https://apparmor.net/>`_ or `SELinux
    <https://github.com/SELinuxProject/selinux>`_ provide additional access
    controls

Unlike VMs, containers share the host’s kernel, which makes them lighter but
potentially less secure.

The advantages include extremely fast start-up times (10–50 ms), minimal
resource usage, an extensive ecosystem of tools and images, excellent
compatibility with existing applications, and mature orchestration platforms
such as `Kubernetes <https://kubernetes.io/>`_. The shared kernel model enables
efficient use of resources and makes containers ideal for microservices
architectures.

However, the shared kernel also creates potential attack vectors, known as
*container escape vulnerabilities*. Most security incidents, however, result
from misconfigurations. Proper configuration, avoiding privileged containers and
using minimal base images significantly reduce the risks – which is why
platforms on which potentially malicious code can be executed favour micro-VMs
or :ref:`language-runtimes`.

Docker/OCI containers are therefore only suitable for deploying trusted
applications, development environments, CI/CD pipelines and microservices
architectures; they are not suitable for executing untrusted code from external
sources or for scenarios requiring maximum security isolation.

Nevertheless, `Docker Hub <https://hub.docker.com/>`_ and `Kubernetes
<https://kubernetes.io/>`_ are widely used as containers for development
environments, whilst `Gitpod <https://gitpod.io>`_ and `Coder
<https://coder.com>`_ are widely used as :abbr:`CDEs (Cloud Development
Environments)` for software development.

.. _language-runtimes:

Language Runtimes
-----------------

This is the lightest-weight form of sandboxing, in which isolation is enforced
not by the operating system or the hardware, but by the runtime environment
itself. This approach offers the fastest start-up times and the lowest resource
overhead, but is the most restrictive in terms of compatibility.

WebAssembly (WASM)
~~~~~~~~~~~~~~~~~~

The `WebAssembly <https://webassembly.org>`_ security model is based on two
fundamental principles:

Memory Safety
    WASM code is executed in a memory region that is completely isolated from
    the host process’s memory. Every memory access is automatically checked by
    the runtime environment, preventing buffer overflows from affecting the host
    or other WASM modules. The call stack is also managed by the runtime
    environment and is inaccessible to the WASM code, thereby neutralising
    conventional stack-smashing attacks.
Capability-based security
    A WASM module is initially inactive. It has no inherent ability to access
    the file system, the network or other external resources. To perform I/O
    operations, the host environment must explicitly grant these capabilities by
    passing functions (known as ``imports``) during instantiation. This *deny by
    default* strategy ensures that a module can only do what it has been
    explicitly authorised to do.

This technology is used on Docker and Kubernetes platforms:

`Fermyon Spin <https://spinframework.dev/>`_
    is a framework for building and running event-driven microservice
    applications with WebAssembly components
`WasmEdge <https://wasmedge.org/>`_
    is a resource-efficient, high-performance and extensible WebAssembly runtime
    environment for cloud-native, edge and decentralised applications
`Wasmtime <https://wasmtime.dev/>`_
    is a lightweight WebAssembly runtime environment that is fast, secure and
    standards-compliant

.. seealso::
   * `WebAssembly on Kubernetes
     <https://www.cncf.io/blog/2024/03/12/webassembly-on-kubernetes-from-containers-to-wasm-part-01/>`_
