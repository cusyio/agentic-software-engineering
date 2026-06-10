.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Setup
=====

You can configure Claude Code in the `CLAUDE.md
<https://code.claude.com/docs/en/memory#how-claude-md-files-load>`_ file. Most
other agents, however, use `AGENTS.md <https://agents.md/>`_. Generally
speaking, though, you shouldn’t simply copy an existing configuration file;

.. warning::
   Anthropic recommends a maximum of 200 lines; see `My CLAUDE.md is too large
   <https://code.claude.com/docs/en/memory#my-claude-md-is-too-large>`_.

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

Testing
-------

Many of our projects use :doc:`python-basics:test/tdd` with
:doc:`python-basics:test/pytest/index` and :doc:`python-basics:test/hypothesis`.
Furthermore, :doc:`mocking <python-basics:test/mock>` and :ref:`python-basics:monkeypatch-fixture` should be avoided.

.. literalinclude:: AGENTS.md
   :caption: AGENTS.md
   :language: md
   :lines: 22-30

Documentation
-------------

We use Google-style :doc:`python-basics:document/sphinx/docstrings` in all
functions and classes. We also typically write :doc:`doctests
<python-basics:document/doctest>` to verify the documentation.

.. literalinclude:: AGENTS.md
   :caption: AGENTS.md
   :language: md
   :lines: 32-34

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
