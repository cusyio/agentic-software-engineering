.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Sandboxing
==========

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

.. include:: ../glossary.rst
   :start-after: start-containers:
   :end-before: end-containers:

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
