.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Spec-Driven Development
=======================

Development teams are increasingly facing challenges regarding predictability
and maintainability when requirements and context are stored exclusively in
short-lived chat threads. To address this issue, tools for spec-driven
development (SDD) have emerged over the past year. Although the definition of
the term is still evolving, it generally refers to workflows that begin with a
structured functional specification and then break this down, in several steps,
into smaller parts, solutions and tasks. The specification may be a single
document or a series of documents or structured artefacts that capture various
functional aspects.

We have already examined a number of tools that have interpreted spec-driven
development in different ways:

`Kiro <https://kiro.dev>`_
    guides development teams through three workflow phases – requirements,
    design and task creation.
`GitHub Spec Kit <https://github.github.com/spec-kit/>`_
    follows a similar three-stage process, but offers more comprehensive
    coordination, configurable prompts and ‘constitutional compliance’, in which
    immutable principles are defined.
`Tessl Framework <https://tessl.io>`_
    takes a more radical approach, in which it is not the code but the
    specification itself that becomes the maintained artefact.

However, the workflows for all three tools are complex and highly specific:
depending on the scope and nature of the task, these tools behave very
differently – some generate extensive specification files that are difficult to
review, whilst with others the :abbr:`PRDs (Product Requirements Documents)` or
user stories are very unclear.

.. seealso::
   * `Understanding Spec-Driven-Development: Kiro, spec-kit, and Tessl by
     Birgitta Böckeler
     <https://martinfowler.com/articles/exploring-gen-ai/sdd-3-tools.html>`_

OpenSpec
--------

`OpenSpec <https://openspec.dev>`_ is an open-source SDD framework that
introduces a lean specification layer and ensures that people and coding agents
agree on what is to be developed before code is generated.

The standard OpenSpec `OPSX workflow
<https://github.com/Fission-AI/OpenSpec/blob/main/docs/opsx.md>`_ is
characterised by a minimalist approach that is often reduced to the following
four actions, rather than phases: *Create, Implement, Update, Archive* – each of
which can be carried out at any time. OpenSpec focuses on specification changes
rather than requiring a complete specification to be defined from the outset,
making it well-suited to `brownfield
<https://en.wikipedia.org/wiki/Brownfield_(software_development)>`_ projects.

OpenSpec thus sets itself apart from the cumbersome and rigid :abbr:`BMAD
(Breakthrough Method for Agile Ai Driven Development)` method. Thanks to its
iterative approach and tool independence, OpenSpec could be a developer-friendly
framework, which we are currently evaluating further.

.. seealso::
   * `OpenSpec Documentation
     <https://github.com/Fission-AI/OpenSpec/blob/main/docs/README.md>`_
   * `Getting Started
     <https://github.com/Fission-AI/OpenSpec/blob/main/docs/getting-started.md>`_
   * `Core Concepts at a Glance
     <https://github.com/Fission-AI/OpenSpec/blob/main/docs/overview.md>`_
