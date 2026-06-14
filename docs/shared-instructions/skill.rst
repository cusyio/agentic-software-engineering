.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

``SKILL.md``
============

As coding agents evolve from simple chat interfaces towards autonomous task
execution, :doc:`context engineering <../context>` has become a critical
challenge. Agent Skills provide an open standard for modularising contexts by
bundling instructions, executable scripts and associated resources such as
:doc:`python-basics:test/tdd`. Whilst :doc:`agents` is loaded at the start of
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

Skills and the programming environment
--------------------------------------

The Skills mechanism relies entirely on the model having access to a file
system, tools for navigating it, and the ability to execute commands within that
environment.

This is the main difference between Skills and previous attempts to extend the
capabilities of LLMs, such as :term:`MCP`. Skills therefore offer several
advantages:

* they are powerful
* they are easy to create
* and LLMs can be made available in :doc:`secure programming environments
  <../security>`

Advantages of Skills over MCP
-----------------------------

Skills are more efficient
    `GitHub’s official MCP <https://github.com/github/github-mcp-server>`_, on
    the other hand, consumes tens of thousands of context tokens on its own, and
    as soon as a few more are added, the LLM is left with hardly any room to
    actually do any useful work.

    LLMs, on the other hand, know how to call :samp:`{CLI-TOOL} --help`, so we
    don’t need to use many tokens to describe its usage – the model can figure
    that out for itself later if necessary.

Skills can also be used with other models
    You can simply take a Skills folder and point Codex CLI or Gemini CLI to it
    using:

        Read `uv-tdd/SKILL.md` and then create a project structure

    This will work even though these tools and models have no built-in knowledge
    of Skills.

Skills are more secure
    The instructions can be executed in :doc:`secure programming environments
    <../security>`.

Skills are simpler
    :term:`MCP` is a complete protocol specification featuring hosts, clients,
    servers, resources, prompts, tools, samples, roots and three different
    transport protocols: `stdio
    <https://modelcontextprotocol.io/specification/2025-06-18/basic/transports#stdio>`_,
    `streamable HTTP
    <https://modelcontextprotocol.io/specification/2025-06-18/basic/transports#streamable-http>`_
    and, originally, `HTTP with SSE
    <https://modelcontextprotocol.io/specification/2024-11-05/basic/transports#http-with-sse>`_.
    Skills, on the other hand, are based on Markdown with a little YAML metadata
    and some optional scripts that can be executed in the respective
    environment. They are therefore much closer to the concept of LLMs, as you
    can simply enter text that the model interprets.

.. seealso::
   * `Skills compared to MCP
     <https://simonwillison.net/2025/Oct/16/claude-skills/#skills-compared-to-mcp>`_
     by Simon Willison, posted on 16th October 2025

Skill plugins
-------------

As their popularity has grown, the surrounding ecosystem has also expanded.
`Plugin marketplaces <https://code.claude.com/docs/en/plugin-marketplaces>`_ are
emerging as a way to version and share skills, and numerous projects are
exploring how to assess the effectiveness of skills. Nevertheless, you should
not use third-party skills without first verifying them, as they pose serious
`security risks within the software supply chain
<https://snyk.io/blog/toxicskills-malicious-ai-agent-skills-clawhub/>`_.

.. seealso::
   * `Discover and install prebuilt plugins through marketplaces
     <https://code.claude.com/docs/en/discover-plugins>`_
   * `Create plugins <https://code.claude.com/docs/en/plugins>`_
