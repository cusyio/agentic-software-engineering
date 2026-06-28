.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

``AGENTS.md``
=============

     In particular, we show that the presence of a root :file:`AGENTS.md` file
     is associated with reduced token usage and faster task completion on real
     pull requests.

– `On the Impact of AGENTS.md Files on the Efficiency of AI Coding Agents
<https://arxiv.org/abs/2601.20404>`_

Cross-agent configuration
-------------------------

If you want to support multiple agents in your projects, you can simply use
``@AGENTS.md`` in :file:`CLAUDE.md` to reference the configuration in your
:file:`AGENTS.md` file.

.. seealso::
   `Claude Code Docs: AGENTS.md
   <https://code.claude.com/docs/en/memory#agents-md>`_

General approach
----------------

I usually like to have five proposed solutions put forward first, before
implementing the one that is likely to be the most effective:

.. literalinclude:: AGENTS.md
   :caption: AGENTS.md
   :language: md
   :lines: 6-7

.. _uv:

uv
--

Many agents typically use ``pip`` when installing packages or running scripts. A
`CLAUDE.md <https://code.claude.com/docs/en/memory#how-claude-md-files-load>`_
or :file:`AGENTS.md` file in your project’s root directory overrides this
default setting, so that :doc:`Python4DataScience:productive/envs/uv/index` is
used instead in every session. One possible configuration for an
:file:`AGENTS.md` file is:

.. literalinclude:: AGENTS.md
   :caption: AGENTS.md
   :language: md
   :lines: 10-12

.. seealso::
   * :ref:`python-basics:uv`
   * :doc:`Python4DataScience:productive/envs/uv/claude-cursor`

.. _code-quality:

Code quality and linting
------------------------

We usually check code quality and syntax using tools such as
:doc:`Python4DataScience:productive/qa/ruff`, `ty
<https://docs.astral.sh/ty/>`_, `prek <https://prek.j178.dev/>`_ and
:doc:`Python4DataScience:productive/qa/wily`.

.. literalinclude:: AGENTS.md
   :caption: AGENTS.md
   :language: md
   :lines: 14-16

Typing
------

Avoid excessive type conversions. If you find that type conversions occur
frequently in the code, the code should be refactored to use more appropriate
types. Ideally, type conversions should only be performed at interfaces with
external systems. Use :doc:`type hints <python:library/typing>` for all function
parameters and return types.

.. literalinclude:: AGENTS.md
   :caption: AGENTS.md
   :language: md
   :lines: 18-20

.. _testing:

Testing
-------

Many of our projects use :doc:`python-basics:test/tdd` with
:doc:`python-basics:test/pytest/index` and :doc:`python-basics:test/hypothesis`.
Furthermore, :doc:`mocking <python-basics:test/mock>` and :ref:`python-basics:monkeypatch-fixture` should be avoided.

.. literalinclude:: AGENTS.md
   :caption: AGENTS.md
   :language: md
   :lines: 22-30

.. _documentation:

Documentation
-------------

We use Google-style :doc:`python-basics:document/sphinx/docstrings` in all
functions and classes. We also typically write :doc:`doctests
<python-basics:document/doctest>` to verify the documentation.

.. literalinclude:: AGENTS.md
   :caption: AGENTS.md
   :language: md
   :lines: 32-34

.. _logging:

Logging
-------

We typically use :doc:`python-basics:logging/index` to gain insights into
errors. We do not want ``print`` statements in the code for debugging purposes.
However, we do not use logging to hide stack traces when an error occurs anyway.
Nor should :doc:`python-basics:control-flow/exceptions` be hidden. If an
exception is to be caught, it should at least be logged.

.. literalinclude:: AGENTS.md
   :caption: AGENTS.md
   :language: md
   :lines: 36-38

Command-line tools
------------------

With command-line tools, we usually want a ``–verbose`` flag that provides log
output useful for troubleshooting.

.. literalinclude:: AGENTS.md
   :caption: AGENTS.md
   :language: md
   :lines: 40-41

.. _overloaded-agent-instructions:

Overloaded instructions for agents
----------------------------------

Context files tend to accumulate code overviews, architectural explanations,
conventions and rules over time. Although each individual addition may be useful
in its own right, this often results in an overload of instructions for the
coding agent. The instructions become longer and sometimes contradict one
another. Models then tend to pay less attention to such content. As the volume
of instructions increases, so does the likelihood that important rules will be
ignored. Anthropic therefore recommends 200 lines as the upper limit; see `My
CLAUDE.md is too large
<https://code.claude.com/docs/en/memory#my-claude-md-is-too-large>`_. You can
also optimise instructions by adding highlights, such as *IMPORTANT* or *YOU
MUST*, to improve compliance.

We have observed that many teams also use coding agents to generate
:file:`AGENTS.md` files. However, our experience suggests that handwritten
versions appear to be more effective than those that have been generated. The
most effective instructions seem to be those that are added selectively and
reveal the context step by step, displaying only the instructions and
capabilities an agent needs for its current task.

.. seealso::
   * `Provide specific context in your prompts
     <https://code.claude.com/docs/de/best-practices#provide-specific-context-in-your-prompts>`_
