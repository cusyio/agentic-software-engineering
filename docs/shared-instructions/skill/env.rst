.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Skills and the programming environment
======================================

The Skills mechanism relies entirely on the model having access to a file
system, tools for navigating it, and the ability to execute commands within that
environment.

This is the main difference between Skills and previous attempts to extend the
capabilities of LLMs, such as :term:`MCP`. Skills therefore offer several
advantages:

* they are powerful
* they are easy to create
* and LLMs can be made available in :doc:`secure programming environments
  <../../security/sandboxing/index>`
