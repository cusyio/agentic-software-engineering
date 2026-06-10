.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Verify
======

Coding agents stop working once their task appears to be ‘complete’. Only when
the coding agents receive confirmation that your test suite, linter, :abbr:`etc.
(et cetera)` have run without errors is the feedback loop complete and the task
finished.

Once the check exists, you should specify how strictly it controls the stop:

In a single prompt
    Ask the coding agent to perform the check and iterate within the same
    message. This currently works for every task.
Across a session
    In Claude Code, you can also set the check as a `/goal
    <https://code.claude.com/docs/en/goal>`_ condition. A separate evaluator
    checks it again after every move, and Claude continues working until it is
    fulfilled. This ensures that an unattended run is completed correctly even
    without your intervention. 
As a deterministic criterion
    A stop hook runs your test as a script and prevents the step from ending
    before it has passed.

    Have the coding agent provide evidence rather than simply claiming success;
    this could be the test output or a screenshot of the result. Checking the
    evidence is quicker than running the verification itself again, and also
    works for sessions you haven’t observed.

Through a second opinion
    A verification sub-agent or a dynamic workflow that checks its own results
    allows a new model to attempt to refute the result, so that the agent
    performing the work is not the one evaluating it.
