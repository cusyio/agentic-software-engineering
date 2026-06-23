.. SPDX-FileCopyrightText: 2026 cusy GmbH
..
.. SPDX-License-Identifier: BSD-3-Clause

Guardrails
==========

The term *AI guardrails* first appeared in 2023, when ChatGPT launched its
consumer service. The initial versions contained numerous security and privacy
vulnerabilities, which ultimately prompted many projects to implement safeguards
to mitigate privacy issues, security risks and questionable content raising
ethical concerns.

However, anyone familiar with hash filters knows that matches with minor
alterations can be bypassed. For example, `Daphne Ippolito et al.
<https://arxiv.org/abs/2210.17546>`_ demonstrated that the Copilot filter for
copyright-protected code can be circumvented by changing the variable names to
French:

.. blacken-docs:off

.. code-block:: python
   :emphasize-lines: 1

   float Q_rsqrt( float number )
   {
     long i;
     float x2 , y;
     const float threehalfs = 1.5F;

     x2 = number * 0.5F;
     y  = number;
     i  = * ( long * ) &y;

.. blacken-docs:on

Copilot stopped generating text at this point.

However, the prompt using a French variable name bypasses the filter:

.. blacken-docs:off

.. code-block:: python
   :emphasize-lines: 1

   float Q_sqrt( float nombre )
   {
     long i ;
     float x2 , y;
     const float trois_moitie = 1.5 F;

     x2 = nombre * 0.5F;
     y  = nombre;
     i  = * ( lo ng * ) &y;
     i  = 0x5f3759df - ( i >> 1 )
     y  = * ( float * ) &i;
     y  = y * (trois_moitie - (x2*y*y));
     //y = y * (trois_moitie - (x2*y*y));

     return nombre * y;
   }

.. blacken-docs:on

The model was run with the *Block suggestions matching public code* option
enabled. The prompts are highlighted.

These filters are therefore only helpful in exceptional cases, and even then
only for very narrowly defined data protection issues. Like many input/output
filters, they are inherently vulnerable by design and do not scale well, as they
are still intended to operate within a large, non-deterministic system – a
solution that has always been inadequate.

.. tip::
   You should not rely on AI guardrails but instead integrate your own security
   measures before and after your API calls.

NVIDIA NeMo Guardrails
----------------------

`NVIDIA NeMo <https://www.nvidia.com/de-de/ai-data-science/products/nemo/>`_ ist
is an agent-oriented, open-source suite for optimising and controlling AI
agents. The `NeMo Guardrails Library
<https://docs.nvidia.com/nemo/guardrails/latest/about-nemo-guardrails-library/overview>`_
(→ `PyPI <https://pypi.org/project/nemoguardrails/>`_, → `GitHub
<https://github.com/NVIDIA-NeMo/Guardrails>`_) is a Python package for creating
programmable guardrails for LLM-based applications. It integrates with
`Llama 3.1 NemoGuard 8B Content Safety
<https://build.nvidia.com/nvidia/llama-3_1-nemotron-safety-guard-8b-v3>`_,
`Llama-Guard <https://docs.nvidia.com/nemo/guardrails/latest/configure-guardrails/guardrail-catalog/third-party/llama-guard>`_ `and many others
<https://docs.nvidia.com/nemo/guardrails/latest/configure-guardrails/guardrail-catalog/third-party>`_.
It is also designed to detect jailbreak attempts, verify tool integration and
log agent actions.

Cloud or AI providers
---------------------

Many AI model providers also make their own guardrails available. Whilst you
cannot influence these, you should still use them within these system
environments. However, some of them also provide separate guardrail models and
deterministic software tests that you can utilise.

Amazon Bedrock Guardrails
~~~~~~~~~~~~~~~~~~~~~~~~~

`Amazon Bedrock Guardrails <https://aws.amazon.com/bedrock/guardrails/>`_ offers
several guardrail models and software-based tests. For example, the Content
Filters provide two guardrail models: one trained on categories of toxic prompt
styles in multimodal inputs, and another trained on potential prompt injection
or jailbreak attacks.

.. seealso::
   * `Configure content filters
     <https://docs.aws.amazon.com/bedrock/latest/userguide/guardrails-content-filters-overview.html>`_
   * `Detect prompt attacks
     <https://docs.aws.amazon.com/de_de/bedrock/latest/userguide/guardrails-prompt-attack.html>`_

Anthropic Claude Code
~~~~~~~~~~~~~~~~~~~~~

Claude Code does not provide any explicit guardrails. Instead, users are
expected to formulate their own `streaming Rrefusals
<https://platform.claude.com/docs/en/test-and-evaluate/strengthen-guardrails/handle-streaming-refusals>`_.

.. warning::
   Prompt exfiltration is relatively straightforward; see `Effective Prompt
   Extraction from Language Models <https://arxiv.org/abs/2307.06865>`_.

Azure
~~~~~

Guardrails can be set up via the `Azure AI Foundry
<https://techcommunity.microsoft.com/blog/azureinfrastructureblog/guardrails-for-generative-ai-securing-developer-workflows/4505801>`_.

.. seealso::
   * `Prompt Shields
     <https://learn.microsoft.com/en-us/azure/ai-services/content-safety/concepts/jailbreak-detection>`_
   * `Spotlighting
     <https://learn.microsoft.com/en-us/azure/foundry/openai/concepts/content-filter-prompt-shields#spotlighting-preview>`_

OpenAI
~~~~~~

`OpenAI Guardrails <https://guardrails.openai.com/>`_ provides custom guardrail
models as well as guidance on how to configure these models.

.. seealso::
   * `github.com/openai/openai-guardrails-python
     <https://github.com/openai/openai-guardrails-python>`_

   * `gpt-oss-safeguard
     <https://huggingface.co/collections/openai/gpt-oss-safeguard>`_

     * `User guide for gpt-oss-safeguard
       <https://developers.openai.com/cookbook/articles/gpt-oss-safeguard-guide>`_

OpenGuardrails
~~~~~~~~~~~~~~

`OpenGuardrails <https://openguardrails.com>`_ has published two open models and
a training dataset on `Huggungface <https://huggingface.co/openguardrails>`_. In
addition, OpenGuardrails offers open-source software that can be used to
implement guardrails for both standard LLM/AI workflows and agent-based tasks.

.. seealso::
   * `OpenGuardrails: A Configurable, Unified, and Scalable Guardrails Platform
     for Large Language Models <https://arxiv.org/abs/2510.19169>`_

Guardrails won’t save you
-------------------------

Guardrails are useful, but they are also prone to failure. To ensure
comprehensive data security, multiple approaches are needed to address and
control risks. You should not assume that guardrails will prevent unsafe
behaviour; recent examples show the opposite:

.. figure:: sliong.png
   :alt: Son Luong (@sluongng): Codex just found a “workaround” of not having
         sudo on my pc…

   Source: https://x.com/sluongng/status/2060746160558543217
