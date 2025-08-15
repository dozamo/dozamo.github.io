---
title: "A Template for Generating Multiple Individual Articles"
linkTitle: "Prompt 002 - Tpl TechDoc topic multiples"
description: "An advanced prompt template that instructs an AI to act as a content pipeline, generating multiple, separate articles from a list of topics in a single run."
weight: 120
categories: ["ai", "technology"]
tags: ["prompts-engineering", "multiple-template", "pipeline-content", "automation", "batch-generation", "technical-documentation", "scalability", "llm"]
---

While [Prompt 001 - Single TechDoc]({{< relref "prompt-template-for-tech-docs.md" >}}) is perfect for creating one article at a time, we can evolve the concept to generate content in batches. This is incredibly efficient when you need to create several related but separate documentation pages.

This article documents an advanced "pipeline" prompt. The goal is to provide the AI with a list of topics and have it generate a complete, individual Markdown article for **each** topic in that list, all within a single response.

## The Master Pipeline Prompt

Here is the complete template. It works by giving the AI a list of topics and a sub-template to use for each one.

```markdown
Act as a content generation pipeline. Your task is to take the following list of topics and, for each topic, generate a complete and individual documentation article in strict Markdown format.

**Topics List:**
- crontab
- at
- telinit
- systemctl resource limits

**Article Template to use for EACH topic:**
"""
---
title: "Managing [TOPIC_TITLE_HERE]"
description: "A practical guide to using the [TOPIC_TITLE_HERE] command for task scheduling and system management."
weight: [A_UNIQUE_NUMBER_HERE]
---

(Generate a concise but comprehensive article about [TOPIC_TITLE_HERE] here, following the structure and focus of a technical certification guide. Include syntax, common use cases, and clear examples.)
"""

**Instructions:**
1.  Iterate through each item in the "Topics List".
2.  For each topic, use the "Article Template" to generate a full article. Replace the placeholders like `[TOPIC_TITLE_HERE]`.
3.  Ensure each generated article is complete and stands on its own.
4.  Separate each complete article in your output with a clear separator line, like this:
    `--- END OF ARTICLE ---`
```

## Deconstructing the Prompt: Why It Works

This prompt is more complex because it defines a programmatic workflow for the AI.

{{% alert title="1. Setting the Persona: 'Act as a content generation pipeline'" color="info" %}}
We've changed the persona from a simple "expert" to a "pipeline". This frames the task as a repeatable, automated process, which is exactly what we want.
{{% /alert %}}

{{% alert title="2. Providing Clear Data and a Template" color="info" %}}
Instead of a single topic, we provide two distinct inputs:
- A **Topics List**: The data the pipeline will process.
- An **Article Template**: The "mold" that will be used for each piece of data. This gives us incredible control over the final structure of each article.
{{% /alert %}}

{{% alert title="3. Explicit Looping Instructions" color="info" %}}
The core of the prompt lies in the explicit instructions: "Iterate through each item...", "For each topic, use the 'Article Template'...". This removes all ambiguity and tells the AI to perform a loop, which is a concept it understands well.
{{% /alert %}}

{{% alert title="4. Defining the Output Format" color="info" %}}
Requesting a clear separator (`--- END OF ARTICLE ---`) is crucial. It makes the AI's single, long response easy for a human to parse. You can simply copy and paste each section into its own `.md` file.
{{% /alert %}}

## Workflow

The intended workflow is:
1.  Send this complete prompt to the AI.
2.  Receive the single response containing all the generated articles.
3.  Copy the content for the "crontab" article and save it as `crontab.md`.
4.  Copy the content for the "at" article and save it as `at.md`.
5.  And so on for all topics.