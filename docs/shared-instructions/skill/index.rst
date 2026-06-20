.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Skills
======

As coding agents evolve from simple chat interfaces towards autonomous task
execution, :doc:`context engineering <../../context>` has become a critical
challenge. Agent Skills provide an open standard for modularising contexts by
bundling instructions, executable scripts and associated resources such as
:doc:`python-basics:test/tdd`. Whilst :doc:`../agents` is loaded at the start of
every session, skills are only loaded on demand based on their descriptions,
which reduces token consumption and mitigates issues such as context window
exhaustion or agent instruction overload.

At the start of a session, coding agents can scan all available skill files and
read a brief description from the Markdown file for each one. This is very
token-efficient: each skill consumes only a few dozen extra tokens, with the
full details only being loaded when a task is requested that the skill can help
solve.

.. seealso::
   * `Equipping agents for the real world with Agent Skills
     <https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills>`_
   * `Agent Skills Documentation
     <https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview>`_
   * `Claude Skills Cookbook <https://github.com/anthropics/claude-cookbooks/tree/main/skills>`_
   * `Agent Skills Quickstart
     <https://agentskills.io/skill-creation/quickstart>`_
   * `Agent Skills Specification <https://agentskills.io/specification>`_
   * `Public repository for Agent Skills
     <https://github.com/anthropics/skills>`_

.. toctree::
   :hidden:
   :titlesonly:
   :maxdepth: 0

   env
   skills-mcp
   skill-md-structure
   directory-structure
   security
   plugins
