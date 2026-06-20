.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Agentic Software Development
============================

Unlike a chatbot, agentic programming environments such as Claude Code or Cursor
can not only answer questions, but also read your files, execute commands, make
changes and solve problems autonomously. This changes the way we work: instead
of writing code ourselves and asking the agentic programming environment to
check it, we now describe what we want, and the agent researches, plans and
implements it.

This tutorial covers approaches that have proven effective within our teams and
for data scientists who use coding agents across a wide variety of codebases and
environments.

However, most of our recommendations are based on one limitation: the coding
agents’ context window fills up quickly, and performance declines as it fills.
A context window contains your entire conversation, including every message,
every file that has been read in, and every command output. A single debugging
session or the exploration of a codebase can generate and consume tens of
thousands of tokens.

This is significant because LLM performance declines as the context fills up.
When the context window becomes full, coding agents start to ‘forget’ previous
instructions or make more mistakes. The context window is the most important
resource to manage. To see how a session fills up in practice, track token usage
continuously.

.. seealso::
   :doc:`context`

.. toctree::
   :hidden:
   :titlesonly:
   :maxdepth: 0

   context
   shared-instructions/index
   feedback-loops
   procedure
   security
   jupyter
   glossary
