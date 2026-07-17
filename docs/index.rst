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
| Subscription  | `News from Python for data science`_                         |
| of 2 hours    |                                                              |
| per quarter   |                                                              |
+---------------+--------------------------------------------------------------+

.. _`Introduction to Python`:
   https://cusy.io/en/our-training-courses/introduction-to-python
.. _`Advanced Python`:
   https://cusy.io/en/our-training-courses/advanced-python
.. _`Design patterns in Python`:
   https://cusy.io/en/our-training-courses/design-patterns-in-python
.. _`Efficient testing with Python`:
   https://cusy.io/en/our-training-courses/efficient-testing-with-python
.. _`Software documentation with Sphinx`:
   https://cusy.io/en/our-training-courses/software-documentation-with-sphinx
.. _`Technical writing`:
   https://cusy.io/en/our-training-courses/technical-writing
.. _`Jupyter notebooks for efficient data science workflows`:
   https://cusy.io/en/our-training-courses/jupyter-notebooks-for-efficient-data-science-workflows
.. _`Numerical calculations with NumPy`:
   https://cusy.io/en/our-training-courses/numerical-calculations-with-numpy
.. _`Analysing data with pandas`:
   https://cusy.io/en/our-training-courses/analysing-data-with-pandas
.. _`Read, write and provide data with Python`:
   https://cusy.io/en/our-training-courses/read-write-and-provide-data-with-python
.. _`Cleanse and validate data with Python`:
   https://cusy.io/en/our-training-courses/cleanse-and-validate-data-with-python
.. _`Visualising data with Python`:
   https://cusy.io/en/our-training-courses/visualising-data-with-python
.. _`Designing data visualisations`:
   https://cusy.io/en/our-training-courses/designing-data-visualisations
.. _`Create dashboards`:
   https://cusy.io/en/our-training-courses/create-dashboards
.. _`Versioned and reproducible storage of code and data`:
   https://cusy.io/en/our-training-courses/versioned-and-reproducible-storage-of-code-and-data
.. _`News from Python for data science`:
   https://cusy.io/en/our-training-courses/news-from-python-for-data-science

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
