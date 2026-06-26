.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Slash commands
==============

Slash commands allow you to control coding agents quickly and efficiently using
the keyboard. Type ``/`` in the editor to open the slash pop-up, select a
command, and your coding agent will carry out actions such as switching models,
changing permissions, clearing the context or executing a specific workflow,
without you having to leave the input window.

Here are the links to the documentation for slash commands for three popular
coding agents:

* `Claude Code Commands <https://code.claude.com/docs/en/commands>`_
* `Curosr CLI Slash commands
  <https://cursor.com/en/docs/cli/reference/slash-commands>`_
* `Slash commands in Codex CLI
  <https://developers.openai.com/codex/cli/slash-commands>`_

Skills vs. Slash Commands
-------------------------

At first glance, slash commands and :doc:`skills <skill/index>` may seem
similar. However, they operate on completely different levels and address
different issues. The choice between them affects how much manual work you have
to do in each session.

The most obvious difference is that skills are triggered automatically depending
on what you’re currently doing, whereas slash commands must first be entered
manually.

Skills are process-oriented
~~~~~~~~~~~~~~~~~~~~~~~~~~~

They define the following:

#. What triggers the skill?
#. What steps should be followed once the skill is triggered?
#. What output or action should occur?

The first step is crucial: your coding agent analyses the context of your
current activity – which files are open, what is currently being written, what
the task description is – and matches this information against the available
skills. If there is a match, the skill is executed. This is precisely what makes
skills ideal for recurring, predictable tasks that need to be carried out
consistently without receiving a prompt every time.

Slash commands are explicit instructions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You type :samp:`/{COMMAND}`, and your coding agent carries out that command.
Nothing is executed that you haven’t explicitly instructed it to do.

For example, in Claude, you can use ``/simplify`` and ``/batch`` to effectively
tackle over-engineering in your code. And if you’re struggling with an
overloaded context window, you can simply use ``/compact`` to prevent `context
rot <https://www.understandingai.org/p/context-rot-the-emerging-challenge>`_.

Custom slash commands
---------------------

You can also create custom slash commands for long, frequently used prompts.
This allows you to enter just a short slash command instead of long
instructions. To do this, simply type :samp:`/{NEW_COMMAND}` in Claude Code.
Custom slash commands for Claude Code are stored in the
:file:`.claude/commands/` directory. Each file contains the text of the command.
When you enter the slash command, Claude reads the file and executes it. It is
at this point that the line between commands and skills begins to blur.

When should you use slash commands?
-----------------------------------

Use a slash command if the task is context-dependent, reactive or one-off. Here
are a few situations where slash commands are the right choice, as they should
not be triggered automatically but decided by you:

Reducing the context
    If a session has been running for a while, the context window is getting
    longer and longer, and delays or inconsistencies are occurring, you can use
    ``/compact`` in Claude Code to shorten the window.
Temporarily ignoring the context
    ``/btw`` is intended for situations where you want to ask Claude Code a
    question in the middle of a task without interrupting the workflow.
Debugging a specific output
    If you unexpectedly receive an error message, you can use ``/simplify`` just
    for that section.
Routine tasks
    If you want to check your entire codebase for specific patterns once a month
    using your coding agent, this is a case for a slash command, not a skill.
