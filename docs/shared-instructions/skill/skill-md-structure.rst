.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

:file:`SKILL.md` structure
==========================

Every skill requires a :file:`SKILL.md` file with YAML frontmatter:

.. literalinclude:: ../../../.agents/skills/SKILL.md
   :caption: .agents/skills/SKILL-NAME/SKILL.md
   :language: yaml
   :linenos:

``name:``
    * Maximum 64 characters
    * Lowercase letters, numbers and hyphens only
    * No reserved words such as ``claude``

     examples include ``pdf2txt`` or ``rest-api``.

``description:``
    * Must not be empty
    * Maximum 1024 characters
    * Describe what the skill does and when it should be used. A good example is

      .. code-block:: yaml

         description: Generate descriptive commit messages by analyzing git diffs. Use when the user asks for help writing commit messages or reviewing staged changes.

Consistent terminology
    Choose a term and use it consistently throughout the skill; do not mix ``API
    endpoint`` with ``URL``, ``API route`` and ``path``.
Avoid Windows-style path notation
    Always use forward slashes (``/``) in file paths, even on Windows.
Avoid offering too many options
    Do not present multiple approaches unless necessary.

Guidelines for progressive disclosure
    #. To ensure the skill performs optimally, the :file:`SKILL.md` file should
       be less than 500 lines long.
    #. Split the content into separate files if you are approaching this limit.
    #. Use the patterns described in :doc:`directory-structure` to organise
       instructions, code and resources effectively.

Example
~~~~~~~

.. literalinclude:: ../../../.agents/skills/rest-api/SKILL.md
   :caption: .agents/skills/rest-api/SKILL.md
   :language: yaml
