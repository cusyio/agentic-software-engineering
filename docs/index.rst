.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Agentic Software Engineering
============================

Unlike a chatbot, agentic programming environments such as Claude Code or Cursor
can not only answer questions, but also read your files, execute commands, make
changes and solve problems autonomously. This changes the way we work: instead
of writing code ourselves and asking the agentic programming environment to
check it, we now describe what we want, and the agent researches, plans and
implements it.

    *“The more time I spend working with coding agents, the more convinced I am
    that they make software engineering even harder.*

    *We can do amazing things with them, but unlocking their full potential
    requires extraordinary discipline and knowledge.”*

– `Simon Willison, 24 September 2026
<https://simonwillison.net/2026/Sep/24/harder/>`_

As the use of coding agents increases, so too does the number of studies warning
against becoming overconfident when dealing with LLM-generated code. Whilst
there is ample evidence that these tools can accelerate development –
particularly when creating prototypes and on greenfield projects – the studies
show that `code quality <Python4DataScience:productive/qa/index>`_ can decline
over time.

GitClear’s 2024 study, `AI Copilot Code Quality
<https://www.gitclear.com/ai_assistant_code_quality_2025_research>`_, found that
code duplication and code churn had increased more than expected, whilst
refactoring activity in commit histories had declined. A similar trend is
evident in the Microsoft study `The Impact of Generative AI on Critical Thinking
<https://www.microsoft.com/en-us/research/publication/the-impact-of-generative-ai-on-critical-thinking-self-reported-reductions-in-cognitive-effort-and-confidence-effects-from-a-survey-of-knowledge-workers/>`_:
AI-driven confidence often comes at the expense of critical thinking.

Speeding up one part of the workflow increases the pressure on the other parts.
We found that the effective use of LLM agents requires a focus on `code quality
<Python4DataScience:productive/qa/index>`_, and that established practices such
as :doc:`test-driven development <python-basics:test/methods/tdd>` and
:term:`static testing <Static test procedures>` are becoming increasingly
important, particularly when integrated directly into coding workflows.

We humans remain responsible for what the software does and how it works, but we
use different skills to create the software. This is therefore about *agentic
software engineering* and not *vibe coding* – with vibe coding, people do not
look at the code, whereas with agentic programming they continue to engage with
the code and often examine it in detail.

So what will programming work look like in the future? What skills will be
required? At present, so-called *harness engineering* – which focuses on
:doc:`context engineering <context>` and :term:`static testing <Static test
procedures>` methods relating to LLMs – appears to be central.

This tutorial covers approaches that have proven effective within our teams and
for data scientists who use coding agents across a wide variety of codebases and
environments.

However, most of our recommendations are based on one limitation: the coding
agents’ context window fills up quickly, and performance declines as it fills.
A context window contains your entire conversation, including every message,
every file that has been read in, and every command output. A single debugging
session or the exploration of a codebase can generate and consume tens of
thousands of tokens.

This is significant because LLM performance declines as the context fills up.
When the context window becomes full, coding agents start to ‘forget’ previous
instructions or make more mistakes. The context window is the most important
resource to manage. To see how a session fills up in practice, track token usage
continuously.

.. seealso::
   :doc:`context`

This tutorial is intended as an introduction to agent-based software
development. For an introduction to Python, see the :doc:`python-basics:index`
tutorial; for the Python Data Science Stack, libraries such as :doc:`Python4DataScience:workspace/ipython/index`,
:doc:`Python4DataScience:workspace/numpy/index`,
:doc:`Python4DataScience:workspace/pandas/index`, and related tools, see the
:doc:`Python4DataScience:index` tutorial. In addition, we also offer the
`Jupyter tutorial <https://jupyter-tutorial.readthedocs.io/de/latest/>`_ and the
`PyViz tutorial <https://pyviz-tutorial.readthedocs.io/de/latest/index.html>`_,
as well as a guide to `data visualisation
<https://www.cusy.design/de/latest/viz/index.html>`_ in the `cusy Design System
<https://www.cusy.design/de/latest/index.html>`_.

All tutorials serve as seminar documents for our harmonised training courses:

+---------------+--------------------------------------------------------------+
| Duration      | Topic                                                        |
+===============+==============================================================+
| 3 days        | `Introduction to Python`_                                    |
+---------------+--------------------------------------------------------------+
| 2 days        | `Advanced Python`_                                           |
+---------------+--------------------------------------------------------------+
| 2 days        | `Design patterns in Python`_                                 |
+---------------+--------------------------------------------------------------+
| 2 days        | `Efficient testing with Python`_                             |
+---------------+--------------------------------------------------------------+
| 1 day         | `Software documentation with Sphinx`_                        |
+---------------+--------------------------------------------------------------+
| 2 days        | `Technical writing`_                                         |
+---------------+--------------------------------------------------------------+
| 3 days        | `Jupyter notebooks for efficient data science workflows`_    |
+---------------+--------------------------------------------------------------+
| 2 days        | `Numerical calculations with NumPy`_                         |
+---------------+--------------------------------------------------------------+
| 2 days        | `Analysing data with pandas`_                                |
+---------------+--------------------------------------------------------------+
| 3 days        | `Read, write and provide data with Python`_                  |
+---------------+--------------------------------------------------------------+
| 2 days        | `Cleanse and validate data with Python`_                     |
+---------------+--------------------------------------------------------------+
| 5 days        | `Visualising data with Python`_                              |
+---------------+--------------------------------------------------------------+
| 1 day         | `Designing data visualisations`_                             |
+---------------+--------------------------------------------------------------+
| 2 days        | `Create dashboards`_                                         |
+---------------+--------------------------------------------------------------+
| 3 days        | `Versioned and reproducible storage of code and data`_       |
+---------------+--------------------------------------------------------------+
| 4 days        | `Applied AI with Python`_                                    |
+---------------+--------------------------------------------------------------+
| Subscription  | `News from Python for data science`_                         |
| of 2 hours    |                                                              |
| per quarter   |                                                              |
+---------------+--------------------------------------------------------------+

.. _`Introduction to Python`:
   https://cusy.io/en/our-training-courses/introduction-to-python.html
.. _`Advanced Python`:
   https://cusy.io/en/our-training-courses/advanced-python.html
.. _`Design patterns in Python`:
   https://cusy.io/en/our-training-courses/design-patterns-in-python.html
.. _`Efficient testing with Python`:
   https://cusy.io/en/our-training-courses/efficient-testing-with-python.html
.. _`Software documentation with Sphinx`:
   https://cusy.io/en/our-training-courses/software-documentation-with-sphinx.html
.. _`Technical writing`:
   https://cusy.io/en/our-training-courses/technical-writing.html
.. _`Jupyter notebooks for efficient data science workflows`:
   https://cusy.io/en/our-training-courses/jupyter-notebooks-for-efficient-data-science-workflows.html
.. _`Numerical calculations with NumPy`:
   https://cusy.io/en/our-training-courses/numerical-calculations-with-numpy.html
.. _`Analysing data with pandas`:
   https://cusy.io/en/our-training-courses/analysing-data-with-pandas.html
.. _`Read, write and provide data with Python`:
   https://cusy.io/en/our-training-courses/read-write-and-provide-data-with-python.html
.. _`Cleanse and validate data with Python`:
   https://cusy.io/en/our-training-courses/cleanse-and-validate-data-with-python.html
.. _`Visualising data with Python`:
   https://cusy.io/en/our-training-courses/visualising-data-with-python.html
.. _`Designing data visualisations`:
   https://cusy.io/en/our-training-courses/designing-data-visualisations.html
.. _`Create dashboards`:
   https://cusy.io/en/our-training-courses/create-dashboards.html
.. _`Versioned and reproducible storage of code and data`:
   https://cusy.io/en/our-training-courses/versioned-and-reproducible-storage-of-code-and-data.html
.. _`Applied AI with Python`:
   https://cusy.io/en/our-training-courses/applied-ai-with-python.html
.. _`News from Python for data science`:
   https://cusy.io/en/our-training-courses/news-from-python-for-data-science.html

.. toctree::
   :hidden:
   :titlesonly:
   :maxdepth: 0

   context
   shared-instructions/index
   feedback-loops
   procedure/index
   security/index
   jupyter
   glossary
