.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Directory structure
===================

As your skill grows, you can bundle additional content into separate files,
which your Coding Agent will only load when necessary. For example, if you
specify in your :file:`SKILLS.md` file that the description for email validation
is located in :file:`./email-validation.md`, then this will only be taken into
account in that context.

The complete directory structure might then look like this:

.. code-block:: console

   rest-api/
   ├── SKILL.md               # Main instructions, loaded when triggered
   ├── email-validation.md    # Only loaded for Email validation
   ├── reference.md           # API reference (loaded as needed)
   └── scripts/
       ├── utility.py         # Utility script (executed, not loaded)
       └── validate_email.py  # Validation script

.. warning::
   Deeply nested references should be avoided, as they may not be read in at
   all, or only partially, when required.

Reference files
    For reference files longer than 100 lines, include a table of contents at
    the top. This ensures that your coding agent can recognise the full scope of
    the available information, for example:

    .. code-block:: md

       # API Reference

       ## Contents
       - Setup
       - Authentication
       - Core methods (create, read, update, delete)
       - Error handling

       ## Setup
       …

       ## Authentication
       …

:file:`scripts`
    When writing scripts for skills, handle their error states yourself and do
    not pass them on to your coding agents.
