---
title: "Pipeline Template for Generating Multiple Articles"
linkTitle: "Prompt 002 - Multi-Topic TechDoc Template"
description: "An advanced prompt template that instructs an AI to act as a pipeline, generating multiple separate Markdown articles from a list in a single run."
weight: 20
categories: ["AI", "Technical Documentation"]
tags: ["Prompt Engineering"]
---

While [Prompt 001]({{< relref "prompt-template-for-tech-docs.md" >}}) is perfect for one article at a time, we can scale the process to generate content in batches. This is incredibly efficient when you need to create multiple related documentation pages.

This article documents an advanced "pipeline" prompt. The goal is to give the AI a list of topics and have it generate a complete, individual Markdown article for **each** topic, all within a single response.

## The Master Pipeline Prompt

This template gives the AI a list of topics and a dynamic sub-template to apply to each one.

```markdown
Act as an automated content generation pipeline. You are an expert in [AREA_OF_EXPERTISE], specializing in creating technical documentation for the [CERTIFICATION_NAME_OR_CONTEXT] certification.

Your task is to take the following "Topic List" and, for each topic, generate a complete and individual documentation article in strict Markdown format, optimized for Hugo+Docsy.

### General Requirements:
1.  **Format:** Strict Markdown for each article.
2.  **Focus:** The content must be concise, practical, and relevant to a professional in the field.
3.  **Examples:** Include clear code examples in ```bash blocks.
4.  **Language:** The final output must be in [DESIRED_LANGUAGE].

### Topic List to Process:
```
[LIST_OF_TOPICS_SEPARATED_BY_NEWLINE]
```

### Article Template (To be used for EACH topic):
You must use this template as the foundation for each article. Fill in the placeholders dynamically.
"""
---
title: "[TITLE_GENERATED_FROM_TOPIC]"
description: "[1-2_SENTENCE_DESCRIPTION_GENERATED_FOR_THE_TOPIC]"
weight: [INCREMENTAL_NUMBER_STARTING_AT_10_AND_ADDING_10_FOR_EACH_ARTICLE]
categories: [[LIST_OF_VALID_CATEGORIES]]
tags: [[LIST_OF_3_RELEVANT_TAGS_GENERATED_FOR_THE_TOPIC]]
---

(Here, generate a concise but complete article on the current topic, following all general requirements. The structure should include an introduction, main syntax/usage, and 2-3 practical examples with clear headings.)
"""

### Pipeline Instructions:
1.  Iterate through each item in the "Topic List".
2.  For each topic, apply the "Article Template". Dynamically generate the content for the placeholders (`title`, `description`, `weight`, `tags`).
3.  Ensure each generated article is standalone and complete.
4.  If a topic in the list is ambiguous or you don't have enough information, skip it and add a note at the end of your response in a "Notes" section.
5.  Separate each complete article in your output with a clear and unique separator on a new line:
    `<!-- ARTICLE_SEPARATOR: [ORIGINAL_TOPIC_FROM_LIST] -->`
6.  Wrap the ENTIRE output, including all articles and separators, in a single markdown code block (```markdown).
```

## Deconstructing the Pipeline Prompt

1.  **Pipeline Persona:** `Act as a pipeline...` shifts the AI's mental frame from a "writer" to an "automated processor," ideal for repetitive tasks.
2.  **Separate Inputs:** The prompt clearly distinguishes between the **Data** (`Topic List`) and the **Logic** (`Article Template` and `Instructions`).
3.  **Dynamic Template:** Instructing the AI to dynamically generate the `description` or `tags` produces much more relevant results than a static template. Requesting an incremental `weight` ensures a correct default ordering in Hugo.
4.  **Explicit Loop Instructions:** The `Iterate...` and `For each topic...` instructions turn the prompt into a pseudo-algorithm that the AI can reliably follow.
5.  **Robust Separator:** Using an HTML comment as a separator is a best practice, as it's invisible in the final render and perfect for automatic or manual parsing.

## Suggested Workflow

1.  Fill in the placeholders in the master prompt (`[AREA_OF_EXPERTISE]`, `[LIST_OF_TOPICS]`, etc.).
2.  Submit the complete prompt to the AI.
3.  Receive the single response containing all the articles.
4.  Split the response using the `<!-- ARTICLE_SEPARATOR: ... -->` separator.
5.  Save each section into its own `.md` file. A filename based on the topic is recommended, for example, `ip-command.md`, `nmcli-command.md`, etc.
