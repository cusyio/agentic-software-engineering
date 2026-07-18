.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Platforms
=========

This section contains a detailed, structured analysis of each major platform.

.. _e2b:

e2b
---

`e2b <https://e2b.dev>`_ is an open-source cloud runtime environment
specifically tailored to the requirements of AI applications and autonomous
agents. It provides development teams with sandbox-based cloud environments
powered by :ref:`Firecracker micro-VMs <firecracker-microvm>`, enabling the
secure execution of AI-generated code. The platform places great emphasis on
offering development teams a seamless developer experience through its SDKs and
is designed to serve as the backend infrastructure for agent-based workflows.

.. csv-table:: GitHub Insights
    :header: "Stars", "Contributors", "Commit activity", "Licence"

    ".. image:: https://raster.shields.io/github/stars/e2b-dev/E2B",".. image:: https://raster.shields.io/github/contributors/e2b-dev/E2B",".. image:: https://raster.shields.io/github/commit-activity/y/e2b-dev/E2B",".. image:: https://raster.shields.io/github/license/e2b-dev/E2B"

e2b provides a `guide to self-hosting
<https://github.com/e2b-dev/infra/blob/main/self-host.md>`_ as well as Terraform
scripts for deploying the infrastructure on your GCP or AWS account; support for
Azure and Linux is to follow.

The sandboxes offer unrestricted file system I/O capabilities. The :abbr:`SDKs
(Software Development Kits)` for Python and JavaScript enable the programmatic
uploading and downloading of files, making it easy to provide context
information to an agent or retrieve artefacts from it. The environments are
persistent, meaning that changes to the file system and installed packages are
retained across multiple execution calls within a single session.

By default, the sandboxes have unrestricted internet access. Furthermore, any
service running within the sandbox (such as a web server) can be made accessible
to the public internet via a unique, secure URL provided by the e2b platform,
which facilitates use cases such as hosting generated web applications or
exposing APIs from within the sandbox.

e2b is extremely versatile and is suitable for both short-lived and long-running
workloads. The fast start-up time (~150–200 ms) is ideal for short-lived tasks
such as running a single code snippet for data analysis. However, sessions
lasting up to 24 hours are also supported, making it robust enough for complex,
stateful agent-based tasks, development environments or demanding reinforcement
learning training loops that require persistent state.

.. _microsandbox:

microsandbox
------------

`microsandbox <https://docs.microsandbox.dev/getting-started/introduction>`_ is
a self-hosted platform that focuses exclusively on ensuring maximum security
when executing untrusted code. It combines the hardware-level isolation of
:ref:`libkrun` -based micro-VMs with the start-up speed of containers (under 200
ms) and the complete control offered by self-hosting. It has been developed to
resolve the trade-off between security, speed and control without compromise.

.. csv-table:: GitHub Insights
    :header: "Stars", "Contributors", "Commit activity", "Licence"

    ".. image:: https://raster.shields.io/github/stars/superradcompany/microsandbox",".. image:: https://raster.shields.io/github/contributors/superradcompany/microsandbox",".. image:: https://raster.shields.io/github/commit-activity/y/superradcompany/microsandbox",".. image:: https://raster.shields.io/github/license/superradcompany/microsandbox"

microsandbox supports both persistent and short-lived file systems. In
project-based workflows (``msr``), file changes and installations within a
sandbox are automatically saved to a local directory :file:`./menv` on the host.
This allows you to pause and restart a sandbox without losing your work. For
one-off tasks, temporary sandboxes (``msx``) are also supported, which leave no
trace once they have run. This makes microsandbox extremely flexible and
suitable for both short-lived, stateless tasks and long-running, stateful
workloads. The ``msx`` command is designed for fast, short-lived executions,
whilst the project-based ``msr`` command, with its persistent state, is ideal
for ongoing development work or complex, multi-stage processes where the context
must be preserved.

In addition, the sandboxes can be configured for `incoming and outgoing traffic
<https://docs.microsandbox.dev/sdk/python/networking>`_ using published ports,
DNS and TLS interception, and the handling of confidentiality breaches.

.. _kata_containers:

Kata Containers
---------------

`Kata Containers <https://katacontainers.io>`_ is an open-source container
runtime environment that combines the speed of containers with the security of
virtual machines. It was launched in 2017 by the Open Infrastructure Foundation
and takes a unique approach to container security by running each container in
its own resource-efficient virtual machine. This hybrid approach addresses the
fundamental security concerns of traditional container runtime environments
whilst ensuring compatibility with the container ecosystem.

.. csv-table:: GitHub-Insights
    :header: "Stars", "Contributors", "Commit activity", "Licence"

    ".. image:: https://raster.shields.io/github/stars/kata-containers/kata-containers",".. image:: https://raster.shields.io/github/contributors/kata-containers/kata-containers",".. image:: https://raster.shields.io/github/commit-activity/y/kata-containers/kata-containers",".. image:: https://raster.shields.io/github/license/kata-containers/kata-containers"

Kata Containers creates a resource-efficient virtual machine for each container,
which guarantees hardware-based isolation whilst ensuring compatibility with the
container ecosystem. It supports multiple hypervisors, including `QEMU
<https://www.qemu.org/>`_, `cloud hypervisor
<https://www.cloudhypervisor.org/>`_ and `Firecracker
<https://github.com/firecracker-microvm/firecracker-containerd>`_.

Kata Containers enables full file system functionality within the container/VM
hybrid, with standard options for mounting container volumes and storage.
Network access also encompasses standard container networking models and
Kubernetes network integration. Overall, Kata Containers resolves the
container-versus-VM dilemma by providing both, thereby delivering the
operational benefits of containers (fast start-up, high density, orchestration)
whilst ensuring the security of VMs (hardware isolation, dedicated kernel). This
makes the solution particularly valuable for production environments where
untrusted workloads are run or where regulatory compliance is required.
