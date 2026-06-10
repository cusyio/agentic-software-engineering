.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Procedure
=========

If coding agents start programming straight away, this can result in code that
solves the wrong problem. In Claude Code, you can use planning mode to separate
exploration from execution.

.. note::
   Planning mode is useful, but it also involves extra effort. For tasks where
   the scope is clear and only a small change is required, such as correcting a
   typo or renaming a variable, ask your coding agent to do this straight away.

   The planning phase is most useful if you are unsure about the approach, if
   the change affects multiple files, or if you are unfamiliar with the code to
   be modified.

The recommended workflow comprises four phases: 

#. Explore

   In planning mode, the Coding Agent reads files and answers questions without
   making any changes, for example:

      Read :file:`src/cusy/tasks` and get a sense of how items are defined.

      Also take a look at how items are stored persistently.

#. Plan

   Ask the Coding Agent to draw up a detailed implementation plan, for example:

       If an *Owner* is specified, the status should automatically be changed
       from ``todo`` to ``assigned``.

#. Implement

   If necessary, exit Claude Code’s planning mode and let the coding agent do
   the programming, whilst it checks its work against the plan, for example:

       Implement tests for the :func:`assign` function in accordance with the
       plan. Run the test suite and ensure that these tests fail. Write
       :func:`assign` according to the plan and run the test suite again. If any
       test fails, fix the error in the function until all tests pass.

#. Commit

   Ask the coding agent to make a commit with a meaningful message and to create
   a pull or merge request.
