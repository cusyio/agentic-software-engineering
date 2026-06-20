.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Skills vs. MCP
==============

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

    .. code-block:: md

       Read `skills/uv-tdd/SKILL.md` and then create a project structure

    This will work even though these tools and models have no built-in knowledge
    of Skills.

Skills are more secure
    The instructions can be executed in :doc:`secure programming environments
    <../../security>`.

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
