.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Security
========

Some coding agents allow you to configure permissions within them, for example
automatically, via a whitelist, or within a sandbox. However, these permissions
remain vulnerable to the `Lethal Trifecta
<https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/>`_ if your coding
agent has access to private data, is exposed to untrusted content, and can
communicate externally.

In such cases, we define our own virtual environments and permissions so that
the coding agent cannot escape from this environment.

.. seealso::
   * Federal Office for Information Security (BSI): `Evasion Attacks
     on LLMs - Countermeasures in Practice
     <https://www.bsi.bund.de/SharedDocs/Downloads/EN/BSI/KI/Evasion_Attacks_on_LLMs-Countermeasures.html>`_
