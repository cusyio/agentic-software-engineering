.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Feedback loops
==============

Coding agents stop working once their task appears to be ‘complete’. Only when
the coding agents receive confirmation that your test suite, yout linter,
:abbr:`etc. (et cetera)` have run without errors is the feedback loop complete
and the task finished.

Python helps you with this by providing excellent error messages that can be
fed straight back to the coding agent; the more guidance – or even suggested
solutions – they contain, the better.

.. seealso::
   * :pep:`PEP 657 – Include Fine Grained Error Locations in Tracebacks <657>`

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

Example
-------

.. code-block:: md

   ## Documentation review process

   1. Write your documentation in accordance with the guidelines in `DOCSTRINGS.md`
   2. Check the documentation against the checklist:
      - Ensure consistency in terminology
      - Make sure the examples follow the standard format
      - Ensure that all required sections are present
   3. If any issues are identified:
      - Note down each issue with a specific reference to the relevant section
      - Revise the documentation
      - Review the checklist again
   4. Only proceed once all requirements have been met
   5. Finalise the document and save it

Review and Revise
-----------------

As with :doc:`test-driven development <python-basics:test/tdd>`, you should
first review the results of your skill. This is the only way to ensure that your
skill solves real problems rather than just imaginary ones:

#. Identify gaps

   Have your coding agent perform a representative task without the skill and
   document the specific error or missing context

#. Create Tests

   Develop several scenarios that test these gaps

#. Establish a baseline

   Measure the coding agent’s performance without the skill

#. Write minimal instructions

   The instructions should be as short as possible to close the gaps and pass
   the tests

#. Iterate

   Run tests, compare them with the baseline and refine the skill

#. Observe how your coding agent navigates through the skills

   When refining the skills, pay attention to how your coding agent actually
   uses them in practice. Pay particular attention to the following:

   Unexpected exploration paths
       Are your files being read in the order you expected?
   Overlooked links
       Does your coding agent follow the references you have specified?
   Excessive attention
       If the same file is being read repeatedly, you might want to move its
       contents to the :file:`SKILL.md` file.
   Ignored content
       If a nested file is never accessed, it is often unimportant or its
       significance is not sufficiently indicated.
