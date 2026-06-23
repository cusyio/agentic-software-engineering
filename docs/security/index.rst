.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Security
========

As modern AI agents operate autonomously and can invoke tools, a multi-layered
security approach is essential to filter out malicious or accidental inputs and
prevent prompt injection from compromising your data and infrastructure. The
most effective AI security architectures can be divided into two main
categories:

:doc:`guardrails`
    monitor, sanitise and filter what an AI model reads and writes. They check
    incoming data for common jailbreak attempts, prompt injection or malicious
    code. Generated responses are also checked before being returned to ensure
    that the output is secure and compliant.
:doc:`sandboxing`
    protects your systems in the event that the guardrails are bypassed or fail.
    The coding agents are isolated to prevent malicious code from altering your
    environment or causing data to leak.

.. seealso::
   :doc:`Skill security <../shared-instructions/skill/security>`

.. toctree::
   :hidden:
   :titlesonly:
   :maxdepth: 0

   guardrails
   sandboxing
