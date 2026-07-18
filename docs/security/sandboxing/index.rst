.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Sandboxing
==========

Code sandboxing has evolved from a niche security tool into an indispensable
part of the infrastructure for modern applications. Two key trends have made
sandboxing essential for product development:

* AI and LLM applications

  :abbr:`LLMs (Large Language Models)` generate code that must be executed
  securely. AI agents, data analysis tools and UI frameworks need to execute
  untrusted, dynamically generated code. And as coding agents become
  increasingly capable of autonomously executing code, performing builds and
  interacting with the file system, unrestricted access to a development
  environment poses real risks, ranging as far as the disclosure of login
  credentials. Whilst some coding agents do allow permissions to be set – for
  example, automatically via a whitelist – these permissions remain vulnerable
  to the `Lethal Trifecta
  <https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/>`_ if your coding
  agent has access to private data, is exposed to untrusted content and is able
  to communicate externally. In such cases, a dedicated sandbox should be set up
  in which the agent’s code can run in isolated environments with restricted
  file system access, controlled network connectivity and limited resource
  usage.

* User-Programmable Platforms

  Many :abbr:`SaaS (Software as a Service)` applications, data tools and
  development platforms now allow users to submit their own code via plugins,
  custom scripts or data transformations. This requires secure isolation to
  prevent security breaches. The same need applies to :abbr:`CDEs (Cloud
  Development Environments)` and online :abbr:`IDEs (Integrated Development
  Environments)` such as `GitHub Codespaces
  <https://github.com/features/codespaces>`_, `Gitpod <https://gitpod.io/>`_ and
  `Coder <https://coder.com/>`_, which must isolate each user’s environment from
  the host infrastructure and other users.

Sandboxing should therefore be standard practice and no longer merely an
optional feature. There is now also a wide range of sandboxing options
available. Beyond the built-in sandbox modes of coding agents, there are various
options spanning the spectrum between short-lived and persistent solutions. In
addition to basic isolation, development teams should consider the practical
requirements for a productive sandbox. These include all components necessary
for development and testing, as well as secure and straightforward
authentication with external services. Development teams require port forwarding
as well as sufficient CPU and memory resources to handle the workloads of the
coding agents. Whether the sandbox should be ephemeral by default or persistent
for session recovery is a design decision that depends on the team’s priorities
regarding security, cost and the continuity of workflows.

Furthermore, the focus of sandboxing has shifted from being purely a security
tool to a platform feature designed to deliver powerful capabilities securely.
For example, `Hugging Face uses e2b’s sandboxing
<https://e2b.dev/blog/how-hugging-face-is-using-e2b-to-replicate-deepseek-r1>`_
for reinforcement learning pipelines, whilst `Groq uses e2b
<https://e2b.dev/blog/groqs-compound-ai-models-are-powered-by-e2b>`_ for
*compound AI* systems that combine LLMs with live code execution. This means
that sandboxes must not only be secure, but also fast, reliable and
user-friendly. Modern solutions are evaluated on the basis of their :abbr:`SDKs
(Software Development Kits)`, their execution speed and their integrability.

.. toctree::
   :hidden:
   :titlesonly:
   :maxdepth: 0

   technologies
   platforms
