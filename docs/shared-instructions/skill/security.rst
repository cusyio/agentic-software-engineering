.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Security
--------

Skills should generally be created by you. Skills give coding agents new
capabilities that make them very powerful, but can also cause your coding agents
to call tools or execute code in ways that do not correspond to their intended
purposes.

Key security measures
~~~~~~~~~~~~~~~~~~~~~

* Check all files contained within the skill: :file:`SKILL.md`, scripts, images
  and other resources.
* Look out for unusual patterns such as unexpected network calls, file access
  patterns or operations that do not correspond to the stated purpose of the
  skill.
* Skills that retrieve data from external URLs pose a particular risk, as the
  retrieved content may contain malicious instructions. Even trusted skills can
  be compromised if their external dependencies change over time.
* Malicious skills can invoke tools (file operations, Bash commands, code
  execution) in a harmful manner.
* Skills with access to sensitive data may be designed to pass information on to
  external systems.
* Only use skills from trusted sources. Take particular care when integrating
  skills into production systems.
