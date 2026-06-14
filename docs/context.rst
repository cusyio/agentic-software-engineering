.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Context Engineering
===================

Initially conceived as an optimisation tactic, Context Engineering has evolved
into a fundamental architectural aspect of modern AI systems. Unlike Prompt
Engineering, which focuses on phrasing, Context Engineering deliberately
concentrates on the AI’s information environment.

As agents are required to handle increasingly complex tasks, inserting raw data
into the context leads to `context rot
<https://www.understandingai.org/p/context-rot-the-emerging-challenge>`_ and a
deterioration in reasoning. To counteract this, we are moving away from static,
monolithic prompts towards the incremental disclosure of context: instead of
loading all the instructions and references an agent might need in advance, we
start with a lean index of available information – the coding agent determines
which prompts or contexts are relevant and retrieves only what is necessary,
thereby significantly improving the `signal-to-noise ratio
<https://en.wikipedia.org/wiki/Signal-to-noise_ratio>`_.

Strategies
----------

To ensure the coding agents remain as effective as possible, their context
should focus on the current problem. The following strategies can help you keep
the context to a minimum.

Manage the context proactively
    * Ensure that the current token usage is displayed at all times.
    * Clear the context when switching to another task. You usually have the
      option to save the context beforehand so that you can access it again
      later. If necessary, you can also provide instructions on what should be
      retained in the summary.

Reduce the overhead of the :term:`MCP` server
    * MCP tool definitions are usually deferred, so that only the tool names are
      available in the context until the coding agent uses a specific tool.
    * Use CLI tools where possible – they are still more context-efficient than
      MCP servers, as they do not add tool-specific entries. Coding agents can
      execute CLI commands directly.
    * Disable unused MCP servers.

Install language-specific plugins
    These plugins enable coding agents to navigate symbols precisely, rather
    than relying on text-based searches, thereby reducing unnecessary file reads
    when browsing through unfamiliar code. Installed language servers also
    automatically report type errors following changes, allowing coding agents
    to identify errors without having to run a compiler. In Claude Code, this is
    ``pyright-lsp`` for the Pyright Language Server.
Shift the work to hooks and skills
    * Custom hooks can pre-process data before passing it on to coding agents.
      Instead of having the coding agent search log files for the ``ERROR`` log
      level and return only the relevant lines—thereby reducing the context from
      tens of thousands of tokens to hundreds.
    * `Agent skills <https://agentskills.io/home>`_ provide domain knowledge so
      that the coding agent does not have to research it itself. A
      ``codebase-overview`` skill could, for example, describe your project’s
      architecture, key directories and naming conventions. When the coding
      agent calls the skill, it receives this context immediately, rather than
      having to spend tokens reading through multiple files to understand the
      structure.

Other techniques aim to further improve this ratio:

`Prompt caching <https://platform.claude.com/docs/en/build-with-claude/prompt-caching>`_
    pre-provides static instructions, which reduces costs and shortens the time
    to the first token.
Dynamic retrieval
    goes beyond basic :abbr:`RAG (Retrieval-Augmented Generation)` by selecting
    tools and loading only the necessary :term:`MCP` servers, thereby avoiding
    unnecessary context expansion.
`Context Graphs <https://trustgraph.ai/guides/key-concepts/context-graphs/>`_
    model institutional reasoning – such as policies, exceptions and precedents
    – as structured, queryable data. Context management techniques use stateful
    compression and sub-agents to summarise intermediate steps in long-running
    workflows.
