.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Hooks
=====

Hooks are custom handlers – a script, an HTTP endpoint, an :term:`MCP` tool or a
short LLM prompt – that are triggered at a specific point in a coding agent’s
lifecycle. Hooks receive structured data about what is going to happen next and
can monitor, log, modify or block the process before execution continues.
`Claude Code <https://code.claude.com/docs/en/hooks>`_, `Cursor
<https://cursor.com/docs/hooks>`_ and `Codex
<https://developers.openai.com/codex/hooks>`_ provide such hooks.

A year ago, we still recommended analysing traffic from the outside via an LLM
gateway such as `OpenRouter <https://openrouter.ai/>`_, `Portkey
<https://portkey.ai/>`_ or `LiteLLM <https://www.litellm.ai/>`_.

.. seealso::
   * :doc:`cusyio:blog/ai-programming-tools`

However, with the advent of :term:`MCP` traffic, :doc:`slash commands
<slash-commands>`, and the reading and editing of local files – all of which
take place within the Coding Agent’s execution environment – the gateways no
longer had any visibility and lost their significance. Hooks, on the other hand,
have access to the full context of a session and are therefore well suited to
ensuring compliance with development guidelines for the Coding Agents.

The following diagram illustrates the execution of a single ``PreToolUse`` hook
when Claude Code executes a Bash command. The event is triggered
unconditionally; the selection is then narrowed down by the user-defined matcher
and the ``if`` filters. If both match, the hook command is executed and
determines the outcome. If either fails to match, the hook is skipped and the
tool continues:

.. mermaid::

   flowchart LR
       A[Coding agent runs<br><br><pre>bash rm -rf /tmp/build</pre>] --> B[PreToolUse fires<br><pre>bash rm -rf /tmp/build</pre><br><pre>tool_name: 'Bash'</pre>]
       B --> C{Your watcher<br><br>'Bash' match?}
       C -->|no| D[Hook not matched<br><br>Tool proceeds]
       C -->|yes| E{Your if<br><br>'bash rm *' match?}
      D --> |Tool call allowed| F[Coding agent continues]
       E --> |no| D
       E --> |yes| G[Your hook command<br><br><pre>block-rm.sh</pre>]
       G --> H[Blocked<br><br><pre>permissionDecision: 'deny'</pre>]
       H -->|Tool call blocked| F
       %%{init:{'themeCSS':'#flowchart-G-13 rect, #flowchart-H-15 rect, #L_G_H_0, #L_H_F_0 { stroke: red;}; #flowchart-D-5 rect, #L_D_F_0 { stroke: green;};'}}%%

Hooks are executed within the agent loop
    They occur between the coding agent’s decision to perform an action and the
    actual execution of that action. The ``PreToolUse`` hook sees the command
    string before it is executed, just as it sees the arguments of an
    :term:`MCP` tool call before the call is made. There is no traffic to
    mirror, no proxy and no certificate to trust.
Hooks capture the full context
    The structured payload includes the session ID, the working directory, the
    model, the tool name, the tool input and, often, the full path to the
    transcript.
Hooks can forward data to any location
    Most coding agents support multiple handler types. A hook can execute a
    script, send a ``POST`` request to an HTTPS endpoint, call a tool on a
    connected :term:`MCP` server, or evaluate a small LLM prompt inline.
Hooks can be combined
    Multiple hooks can be registered for the same event. They are executed in
    parallel without being aware of one another, and the results are then
    aggregated.
Hooks are fail-open
    Errors in the hooks are not considered critical. If the script returns an
    error, or if there are network issues or a timeout, the coding agent
    continues.

What problems do hooks solve?
-----------------------------

Preventing unwanted operations
    What constitutes an unwanted operation depends heavily on the project in
    question. The most common scenarios include:

    * blocking malicious shell commands before they are executed
    * denying tool calls that would write to a production database
    * removing sensitive information from prompts before they reach the model
    * preventing a coding agent from hard-coding login credentials.

    The pattern is the same in all these cases: a ``PreToolUse`` or
    ``UserPromptSubmit`` hook evaluates the input against a set of rules and
    returns a rejection decision if a match is found. The action is never
    executed.

    .. warning::
       Contrary to what is often claimed, however, hooks are not deterministic.
       They are therefore not really suitable for the mandatory enforcement of
       policies. Among other things, ‘fail-open’ allows models to bypass
       blocking hooks without this becoming apparent.

Logs and Audits
    You can use hooks to record every prompt, every tool call and every response
    in a central log service. Data that was previously, at best, isolated on
    individual workstations can be consolidated using a hook. This allows you,
    for example, to break down token usage by team, user or workflow to gain a
    clear picture of the internal use of the Coding Agent.
Code quality and security
    Hooks can be triggered by file changes using ``afterFileEdit``, making them
    ideal for running tools that check code quality and security. The coding
    agent thus receives immediate, structured feedback and can regenerate the
    faulty code during the same run.
Workflow automation
    Hooks can also handle other tasks: for example, I can run a formatter such
    as :doc:`Python4DataScience:productive/qa/ruff` whenever a file is changed,
    or insert the :doc:`Git <Python4DataScience:productive/git/index>` status at
    ``SessionStart``. Generally speaking, hooks allow you to extend the project
    with small workflow scripts that every team will need at some point.

Hooks vs. Skills
----------------

The key difference lies in who decides whether an action is taken. Hooks are
triggered by lifecycle events, whilst skills are triggered by the coding agent’s
own conclusions.

How do you implement a hook?
----------------------------

The configuration model is largely identical across the various coding agents. A
:doc:`Python4DataScience:data-processing/serialisation-formats/json/index` file
is located in the relevant directory. The file lists events, optional matchers
and the handler to be executed.

Below, the same hook for blocking ``rm -rf`` before it is executed is
implemented for each of the three coding agents mentioned above. The format of
the decision response and the field names differ, but the pattern is the same in
all three cases:

.. tab:: Claude Code

   .. code-block:: javascript
      :caption: .claude/settings.json

      {
      "hooks": {
        "PreToolUse": [
          {
            "matcher": "Bash",
            "hooks": [
              {
                "type": "command",
                "if": "Bash(rm -rf *)",
                "command": "$PROJECT_DIR/.claude/hooks/block-rm.sh"
              }
            ]
          }
        ]
      }
      }

.. tab:: Cursor

   .. code-block:: javascript
      :caption: .cursor/hooks.json

      {
      "version": 2026-1,
      "hooks": {
        "beforeShellExecution": [
          {
            "command": "$PROJECT_DIR/.cursor/hooks/block-rm.sh"
          }
        ]
      }
      }

.. tab:: Codex

   .. code-block:: javascript
      :caption: .codex/hooks.json

      {
      "hooks": {
        "PreToolUse": [
          {
            "matcher": "Bash",
            "hooks": [
              {
                "type": "command",
                "command": "$PROJECT_DIR/.codex/block-rm.sh",
                "timeout": 10
              }
            ]
          }
        ]
      }
      }

.. tip::
   Hooks are run from the project directory, but their execution environment may
   vary. Therefore, always use absolute paths for files referenced in hook
   commands.

.. tip::
   Don’t forget to make the scripts executable:

   .. code-block:: console

      $ chmod +x ~/.claude/hooks/block-rm.sh

.. tip::
   Always start a new session after making changes to the hook file, as the
   changes will not apply to sessions that are already running.

Unfortunately, each provider uses its own configuration format, its own event
vocabulary, its own input schema and its own format for decision responses. A
rule written in Claude Code to detect sensitive data is not the same script that
works for Cursor or Codex. A project that agrees on a set of hooks would
ultimately have to maintain separate implementations for each coding agent and
manually coordinate them with one another.

Typical hooks
-------------

Of the dozens of events provided by the major coding agents, the following five
cover the bulk of what we actually need:

``SessionStart``, ``sessionStart``
    is triggered **before** the coding agent carries out any further actions.
    This ensures that the hook’s output is given the same weight as an input,
    not just as a file. Furthermore, the hook is deterministic and is executed
    reliably. Finally, it can also be dynamic and incorporate :doc:`Git
    <Python4DataScience:productive/git/index>` status, environment variables,
    API calls or the runtime status.

    .. tip::
       However, you should not treat such a hook as a comprehensive
       documentation extract. If your hook outputs 3,000 tokens of context, you
       have replicated the :ref:`Agents.md <overloaded-agent-instructions>`
       problem by other means.

``UserPromptSubmit``, ``beforeSubmitPrompt``
    is triggered when a user submits an input to the agent. This acts as a
    filter for incoming data. A hook at this point can scan the input for
    sensitive information, such as that inserted from a :file:`.env` file,
    anonymise personal data before it reaches the model, or block inputs that
    match a ``deny`` pattern.
``PreToolUse``, ``preToolUse``
    is triggered before a tool called by the agent is executed. This is the
    checkpoint for outgoing actions. A hook at this point receives the name of
    the tool (``Bash``, ``Write``, ``Edit``, ``mcp__github__create_pr``), the
    arguments and the working directory. Almost all use cases for preventing
    unwanted operations in real time take place here.
``PostToolUse``, ``postToolUse``
    is triggered after a tool returns. This is where the tool’s output is
    checked. The most common use case is the detection of data exfiltration. A
    ``PostToolUse`` hook for ``Bash`` and ``Read`` examines the content and can
    decide whether the agent is permitted to use it later in the conversation.
``SessionEnd``, ``sessionEnd``
    is triggered when the agent completes a step or a session. This hook allows
    you to create a comprehensive observability concept. Here, a handler
    captures the full transcript and forwards it to a central logging service.
    Once the transcript is stored in a queryable system, it becomes possible to
    ask questions such as *‘In which session was this data accessed?’* or
    *‘Which prompts led the coding agent to make this incorrect assumption?’*
    Real-time blocking and retrospective analysis are based on the same data,
    which is simply used in different ways.

The other hooks are also useful for specific automations; however, the five
events mentioned above usually form the basis of our development guidelines. If
you configure these events and the data is forwarded to a central logging
service, the majority of these guidelines are already covered.

Key features of the various coding agents
-----------------------------------------

Claude Code
    offers the most comprehensive selection. It provides events covering the
    entire lifecycle, including `Setup
    <https://code.claude.com/docs/en/hooks#setup>`_, `WorktreeCreate
    <https://code.claude.com/docs/en/hooks#worktreecreate>`_/`WorktreeRemove
    <https://code.claude.com/docs/en/hooks#worktreeremove>`_, `TaskCreated
    <https://code.claude.com/docs/en/hooks#taskcreated>`_/`TaskCompleted
    <https://code.claude.com/docs/en/hooks#taskcompleted>`_, `TeammateIdle
    <https://code.claude.com/docs/en/hooks#teammateidle>`_ and `Elicitation
    <https://code.claude.com/docs/en/hooks#elicitation>`_. For projects relying
    on Claude Code, the potential to prevent undesirable operations, as well as
    for logging and auditing, is correspondingly higher.
Cursor
    relies primarily on introspection of agent loops. It is the only provider
    that offers `afterAgentResponse
    <https://cursor.com/docs/hooks#afteragentresponse>`_ and `afterAgentThought
    <https://cursor.com/docs/hooks#afteragentthought>`_, making not only tool
    calls but also the model’s intermediate results visible. Cursor also
    provides the most detailed hooks for file operations – `beforeReadFile
    <https://cursor.com/docs/hooks#beforereadfile>`_, `afterFileEdit
    <https://cursor.com/docs/hooks#afterfileedit>`_, `beforeTabFileRead
    <https://cursor.com/docs/hooks#beforetabfileread>`_ and `afterTabFileEdit
    <https://cursor.com/docs/hooks#aftertabfileedit>`_ – which simplifies the
    integration of code quality tools such as
    :doc:`Python4DataScience:productive/qa/ruff`.
Codex
    groups many aspects under the categories `PreToolUse
    <https://developers.openai.com/codex/hooks#pretooluse>`_ and `PostToolUse
    <https://developers.openai.com/codex/hooks#posttooluse>`_. Shell commands,
    :term:`MCP` calls and file operations are all handled via the generic tool
    events. Whilst this facilitates traceability, it shifts the workload to the
    matcher.
