.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Glossary
========

.. glossary::
   :sorted:

   Agent Scan
       `agent-scan <https://github.com/snyk/agent-scan>`_ is a security scanner
       for agent ecosystems that detects local components – including
       :term:`MCP` servers and :doc:`skills <shared-instructions/skill/index>` –
       and identifies risks such as prompt injection, `tool poisoning
       <https://invariantlabs.ai/blog/mcp-security-notification-tool-poisoning-attacks>`__,
       toxic workflows, hard-coded secrets and insecure handling of credentials.
       It closes an emerging gap in the transparency of the agent supply chain

   Agent2Agent
   A2A
       `Agent2Agent <https://a2a-protocol.org/latest/>`_ is an open protocol
       that enables communication and interoperability between agent-based
       applications.

   FastMCP
       A Python framework that simplifies the setup, protocol handling and error
       management of an :term:`MCP` server by abstracting the complexity of the
       protocol and enabling development teams to define MCP resources and tools
       via intuitive Python :doc:`decorators
       <python-basics:functions/decorators>`. This abstraction allows teams to
       focus on business logic, resulting in clearer and more maintainable MCP
       implementations.

       Whilst FastMCP 1.0 is already integrated into the official `MCP Python
       SDK <https://github.com/modelcontextprotocol/python-sdk>`_, the MCP
       standard continues to evolve rapidly. You should therefore keep an eye on
       the release of version 2.0 and ensure that you keep pace with changes to
       the official specification.

       .. seealso::
          * `README.v2.md
            <https://github.com/modelcontextprotocol/python-sdk/blob/main/README.v2.md>`_
          * `Migration Guide: v1 to v2
            <https://github.com/modelcontextprotocol/python-sdk/blob/main/docs/migration.md>`_

   Model Context Protocol
   MCP
       open standard that defines how LLM applications and agents integrate with
       external data sources and tools, with the aim of significantly improving
       the quality of AI-generated results. MCP focuses on context and access to
       tools, thereby differing from the :term:`Agent2Agent` (:term:`A2A`)
       protocol, which governs communication between agents. It specifies
       servers (for data and tools such as databases, wikis and services) as
       well as clients (agents, applications and coding assistants). Frameworks
       such as :term:`FastMCP` have emerged, as has the `MCP Registry
       <https://blog.modelcontextprotocol.io/posts/2025-09-08-mcp-registry-preview/>`_
       for identifying public and proprietary tools. However, the protocol also
       has architectural flaws and has attracted `criticism
       <https://julsimon.medium.com/why-mcps-disregard-for-40-years-of-rpc-best-practices-will-burn-enterprises-8ef85ce5bc9b>`_
       for disregarding established RPC best practices. For production
       applications, development teams should carry out thorough security checks
       by mitigating :term:`Toxic Flows` with tools such as :term:`Agent Scan`
       and closely monitoring the authorisation module at runtime.

       .. seealso::
          * `What is the Model Context Protocol (MCP)?
            <https://modelcontextprotocol.io/docs/getting-started/intro>`_

   Toxic Flows
       With the emergence of agents that require extensive permissions, such as
       `OpenClaw <https://openclaw.ai>`_, development teams are increasingly
       deploying agents in environments where they are exposed to a `lethal
       trifecta <https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/>`_:

       #. Access to private data
       #. Exposure to untrusted content, and
       #. The ability to communicate externally.

       As capabilities grow, so does the attack surface, exposing systems to
       risks such as prompt injection and `tool poisoning
       <https://invariantlabs.ai/blog/mcp-security-notification-tool-poisoning-attacks>`__.

       Toxic flow analysis remains one of the most important techniques for
       investigating agent-based systems to identify insecure data paths and
       potential attack vectors. These risks are no longer limited to
       :term:`MCP` integrations; we have also observed similar patterns in
       :doc:`skills <shared-instructions/skill/index>`, where a malicious actor
       can package a seemingly useful function in such a way that it contains a
       hidden instruction to extract sensitive data. We strongly recommend that
       development teams working with agents conduct a toxic flow analysis and
       use tools such as :term:`Agent Scan` to identify insecure data paths
       before they are exploited.

   .. _start-context-strategies:

   Prompt caching
       pre-provides static instructions, which reduces costs and shortens the
       time to the first token.

       .. seealso::
          `Prompt caching
          <https://platform.claude.com/docs/en/build-with-claude/prompt-caching>`_

   Dynamic retrieval
       goes beyond basic :abbr:`RAG (Retrieval-Augmented Generation)` by
       selecting tools and loading only the necessary :term:`MCP` servers,
       thereby avoiding unnecessary context expansion.

   Context Graphs
       model institutional reasoning – such as policies, exceptions and
       precedents – as structured, queryable data. Context management techniques
       use stateful compression and sub-agents to summarise intermediate steps
       in long-running workflows.

       .. seealso::
          `Context Graphs
          <https://trustgraph.ai/guides/key-concepts/context-graphs/>`_

   .. _end-context-strategies:

   .. _start-containers:

   Sprites
       `Sprites <https://sprites.dev/>`_ is a stateful sandbox environment from
       `Fly.io <https://fly.io/>`_, developed using `Firecracker
       <https://firecracker-microvm.github.io>`_ microVMs for the isolated
       execution of coding agents.

       Whilst most sandboxes are short-lived – they are launched for a single
       task and then disappear again – Sprites provides persistent Linux
       environments with unlimited checkpointing and rollback capabilities. This
       enables development teams to take a snapshot of the entire environment
       state – including installed dependencies, runtime configuration and
       changes to the file system – and perform a rollback if an agent goes off
       the rails. This goes beyond what :doc:`Git
       <Python4DataScience:productive/git/index>` alone can restore, as it
       captures the system state that version control does not track.

   Development Containers
       `Development Containers <https://containers.dev>`_ provide a standardised
       method for defining reproducible, containerised development environments
       using the :file:`devcontainer.json` configuration file.

       Originally developed to provide teams with consistent development
       environments, dev containers have found a compelling new use case as
       isolated execution environments for coding agents. Running an agent in a
       dev container isolates it from the host’s file system, credentials and
       network, allowing teams to grant the agent extensive permissions without
       compromising the host machine.

       The `open specification <https://containers.dev/implementors/spec/>`_ is
       natively supported by `VS Code
       <https://containers.dev/supporting#visual-studio-code>`_ and VS
       Code-based tools such as Cursor.

   `DevPod <https://devpod.sh>`_
       extends Dev Container support via :abbr:`SSH  (Secure Shell)` to any
       editor or terminal workflow. Dev Containers follow an
       *ephemeral-by-default* approach, meaning that the container is recreated
       from the configuration each time it is started, which provides a clean
       security boundary, albeit at the cost of having to reinstall tools and
       dependencies.

   .. _end-containers:
