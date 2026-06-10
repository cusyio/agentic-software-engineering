.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Context
=======

To ensure the coding agents perform to the highest possible standard, their
context should focus on the current problem. The following strategies can help
you keep the context concise.

Manage the context proactively
    * Ensure that the current token usage is displayed at all times.
    * Clear the context when you switch to a different task. You usually also
      have the option to save the context beforehand so that you can refer back
      to it later. If necessary, you can provide instructions on what should be
      retained in the summary.
Reduce the overhead of the :abbr:`MCP (Model Context Protocol)` server
    * MCP tool definitions are usually deferred, so that only the tool names are
      available in the context until the coding agent uses a specific tool.
    * Where possible, use CLI tools – they are still more context-efficient than
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
      level and return only the relevant lines, thereby reducing the context
      from tens of thousands of tokens to hundreds.
    * `Agent skills <https://agentskills.io/home>`_ provide domain knowledge so
      that the coding agent does not have to research it itself. A
      ``codebase-overview`` skill could, for example, describe your project’s
      architecture, key directories and naming conventions. When the coding
      agent calls the skill, it receives this context immediately, rather than
      having to spend tokens reading through multiple files to understand the
      structure.
