---
title: "Prompt Template for a Single Technical Article"
linkTitle: "Prompt 001 - Single TechDoc"
description: "A reusable and robust prompt template for generating a single, high-quality technical article using AI language models."
weight: 10
categories: ["ai", "technology"]
tags: ["prompt-engineering", "single-template", "technical-documentation", "llm", "hugo", "markdown", "content-automation"]
---

Having a well-designed prompt template is a superpower when generating technical content with Large Language Models (LLMs). It ensures consistency, enforces quality standards, and dramatically accelerates the creation process.

This article documents the master template for generating a complete, standalone technical article. It's the ideal foundation for creating detailed documentation on a specific topic.

## The Master Template

This template is designed with placeholders (`[...]`) to be universally adaptable. Simply replace the text within the brackets to fit your needs.

```markdown
Act as an expert technical writer in [AREA_OF_EXPERTISE], specializing in creating clear and concise content for professionals and students preparing for the [CERTIFICATION_NAME_OR_CONTEXT] certification.

Your task is to generate a complete documentation article in strict Markdown format, optimized for Hugo with the Docsy theme, on the following topic: **[SPECIFIC_TOPIC_HERE]**

### Content Requirements:
1.  **Format:** Strict Markdown.
2.  **Front Matter:** Include a complete YAML front matter block at the beginning of the article with the following fields:
    - `title`: A clear and descriptive title for the article.
    - `linkTitle`: A shorter version for menus, if applicable (e.g., "[TOPIC] Command").
    - `description`: A 1-2 sentence summary of the article's content.
    - `weight`: Use the value `10`.
    - `categories`: Assign one or more relevant categories from the list: [LIST_OF_VALID_CATEGORIES].
    - `tags`: Assign at least three relevant and specific lowercase tags (e.g., `[tag1]`, `[tag2]`).
3.  **Focus:** The content must be practical and focus on the most important aspects for a professional using this technology. Avoid the topic's history or overly esoteric details.
4.  **Structure:** Organize the article with logical headings (##, ###). Start with a brief introduction, explain the main syntax, and then detail the most common use cases with examples.
5.  **Code Examples:** Include clear, functional examples within code blocks with the specified language (e.g., ```bash).
6.  **Tone and Style:** The tone should be professional, authoritative, and educational. Do not include personal opinions, greetings, or conversational text. Get straight to the point.
7.  **Language:** The final output must be in [DESIRED_LANGUAGE].

### Output Requirements:
Ensure that the ENTIRE output, including the front matter, is wrapped in a single markdown code block (```markdown) to facilitate copy-pasting without formatting errors.
```

## Example Usage: Article on `ip` for LFCS

To generate an article about the `ip` command in the context of the LFCS certification, we would fill out the template like this:

-   **[AREA_OF_EXPERTISE]:** `Linux Networking and DevOps`
-   **[CERTIFICATION_NAME_OR_CONTEXT]:** `LFCS (Linux Foundation Certified Sysadmin)`
-   **[SPECIFIC_TOPIC_HERE]:** `The ip command for network management`
-   **[LIST_OF_VALID_CATEGORIES]:** `["Networking", "Essential Commands"]`
-   **[DESIRED_LANGUAGE]:** `English`

## Deconstructing the Prompt

This prompt is effective because it's a set of precise instructions:

1.  **Establish the Persona:** `Act as...` defines the context, tone, and expected knowledge level.
2.  **Define the Task:** `Your task is to generate...` unambiguously sets the final goal.
3.  **Set Clear Constraints:** The `Content Requirements` section acts as a detailed style guide, controlling everything from file structure to content tone.
4.  **Control the Output Format:** The final `Output Requirements` instruction is a crucial meta-instruction that ensures the model's response is 100% immediately usable.

> **Next Step:** To scale production, learn to generate multiple articles in a single batch with the [Content Pipeline Template]({{< relref "prompt-template-multiples-topic-individual.md" >}}).
