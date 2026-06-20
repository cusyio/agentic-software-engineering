.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Security
========

.. seealso::
   :doc:`Skill-Sicherheit <security>`

Some coding agents allow you to set permissions within them, for example,
automatically, using a whitelist or within a sandbox. However, these permissions
remain vulnerable to the `Lethal Trifecta
<https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/>`_ if your coding
agent has access to private data, is exposed to untrusted content and can
communicate externally.

In such cases, we define our own sandbox so that the agents’ code runs in
isolated environments with restricted file system access, controlled network
connectivity and limited resource usage.

As coding agents are increasingly able to autonomously execute code, perform
builds and interact with the file system, unrestricted access to a development
environment poses real risks, ranging as far as the disclosure of login
credentials. Sandboxing should therefore be standard practice, rather than
merely an optional extension.

There is now a wide range of sandboxing options available. In addition to the
coding agents’ built-in sandbox modes, there are various options spanning the
spectrum between short-lived and permanent solutions:

`Sprites <https://sprites.dev/>`_
    is a stateful sandbox environment from `Fly.io <https://fly.io/>`_,
    developed using `Firecracker <https://firecracker-microvm.github.io>`_
    microVMs for the isolated execution of coding agents.

    Whilst most sandboxes are short-lived – they are launched for a single task
    and then disappear again – Sprites provides persistent Linux environments
    with unlimited checkpointing and rollback capabilities. This enables
    development teams to take a snapshot of the entire environment state –
    including installed dependencies, runtime configuration and changes to the
    file system – and perform a rollback if an agent goes off the rails. This
    goes beyond what `Git <Python4DataScience:productive/git/index>`_ alone can
    restore, as it captures the system state that version control does not
    track.

`Development Containers <https://containers.dev>`_
    provide a standardised method for defining reproducible, containerised
    development environments using the :file:`devcontainer.json` configuration
    file.

    Originally developed to provide teams with consistent development
    environments, dev containers have found a compelling new use case as
    isolated execution environments for coding agents. Running an agent in a dev
    container isolates it from the host’s file system, credentials and network,
    allowing teams to grant the agent extensive permissions without compromising
    the host machine.

    The `open specification <https://containers.dev/implementors/spec/>`_ is
    natively supported by `VS Code
    <https://containers.dev/supporting#visual-studio-code>`_ and VS Code-based
    tools such as Cursor.

`DevPod <https://devpod.sh>`_
    extends Dev Container support via :abbr:`SSH  (Secure Shell)` to any editor
    or terminal workflow. Dev Containers follow an *ephemeral-by-default*
    approach, meaning that the container is recreated from the configuration
    each time it is started, which provides a clean security boundary, albeit
    at the cost of having to reinstall tools and dependencies.

In addition to basic isolation, development teams should take into account the
practical requirements for a productive sandbox. These include all the
components required for development and testing, as well as secure and
straightforward authentication with external services. Development teams
require port forwarding as well as sufficient CPU and memory resources to handle
the workloads of the coding agents. Whether the sandbox should be ephemeral by
default or persistent to allow for session recovery is a design decision that
depends on the team’s priorities regarding security, cost and the continuity of
workflows.

.. seealso::
   * Federal Office for Information Security (BSI): `Evasion Attacks
     on LLMs - Countermeasures in Practice
     <https://www.bsi.bund.de/SharedDocs/Downloads/EN/BSI/KI/Evasion_Attacks_on_LLMs-Countermeasures.html>`_
