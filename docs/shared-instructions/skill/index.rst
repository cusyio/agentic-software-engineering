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

Library Skills
--------------

`Library Skills <https://library-skills.io>`_ allows you to keep the skills for
libraries that contain their own integrated AI functions synchronised with the
latest version of the library. Libraries such as
:doc:`Python4DataScience:data-processing/apis/fastapi/index` contain a
:file:`.agents/skills/fastapi/SKILL.md` file, which can easily be adopted.

In Python, you can use ``library-skills`` with ``uvx library-skills``. This will

* check the dependencies you have defined in :file:`pyproject.toml` or
  :file:`package.json`
* check your project’s installation environment, for example, the :file:`.venv`
  directory for Python and :file:`node_modules` for Node.js
* identify the available skills for the libraries you have installed
* display the installation status of these skills
* ask which new skills you would like to install
* ask about the installation target (:file:`.agents/skills` or
  :file:`.claude/skills`)
* create a relative symbolic link for each skill you select and each
  destination, so that they can be managed with :doc:`Git
  <Python4DataScience:productive/git/index>`

pre-commit hook
~~~~~~~~~~~~~~~

You can run the same check using the
:doc:`Python4DataScience:productive/git/advanced/hooks/pre-commit` to detect
changes to the skills:

.. code-block:: yaml

   repos:
     - repo: local
       hooks:
         - id: library-skills-check
           name: library-skills check
           entry: uvx library-skills --check
           language: system
           pass_filenames: false
           files: ^(pyproject\.toml|uv\.lock|package\.json|package-lock\.json)$

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
