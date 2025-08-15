---
title: "A Template for Generating a Single Technical Article"
linkTitle: "Prompt 001 - Single TechDoc"
description: "A reusable and effective prompt template for generating a single, high-quality technical article using AI language models."
weight: 110
categories: ["ai", "technology"]
tags: ["prompts-engineering", "individual-template", "technical-documentation", "llm", "hugo", "markdown", "lfcs", "content-automation"]
---

Having a well-crafted prompt template is a superpower when working with Large Language Models (LLMs) to generate technical content. It ensures consistency, enforces quality standards, and dramatically speeds up the content creation process.

This article documents the foundational "master prompt" for generating a complete, self-contained technical article. This is the ideal template for creating in-depth documentation on a single topic.

## The Master Prompt Template

Here is the complete template. It is designed to be fed directly to an AI model to produce a structured, focused, and correctly formatted technical article.

```markdown
Act as a Linux and DevOps expert, specializing in creating technical content for certifications. Your task is to generate a documentation article in strict Markdown format, optimized for Hugo+Docsy.

The article must cover the following topic: [Insert Command or Technology Topic Here]

Content Requirements:
1.  **Format:** Strict Markdown. It must include a Hugo front matter section at the beginning with 'title', 'description', and 'weight'.
2.  **Focus:** The content must be concise, practical, and focus exclusively on the most relevant aspects for a certification exam like the LFCS (Linux Foundation Certified Sysadmin). Avoid overly esoteric details.
3.  **Structure:** Organize the article with clear headings. Explain the basic syntax and then the most important sub-operations with examples.
4.  **Examples:** Include clear and brief code examples inside bash code blocks (```bash).
5.  **Language:** The final output must be in [Desired Language].

Ensure that the entire final output is wrapped in a single fenced code block (```markdown) to avoid formatting issues when copying and pasting.
```

## Deconstructing the Prompt: Why It Works

This prompt is effective because it leaves very little to chance. Each instruction serves a specific purpose.

{{% alert title="1. Setting the Persona: 'Act as...'" color="success" %}}
This is the most critical part. By telling the AI to "Act as a Linux and DevOps expert...", we are setting the **context, tone, and knowledge base**. The model will adopt a professional, technical voice and draw from its training data related to that specific domain.
{{% /alert %}}

{{% alert title="2. Defining the Core Task: 'Your task is to generate...'" color="success" %}}
This clearly states the final deliverable: a documentation article. It immediately sets the scope of the output, preventing a conversational response or a simple list of facts.
{{% /alert %}}

{{% alert title="3. Establishing Clear Constraints: 'Content Requirements'" color="success" %}}
This section acts as a set of "guardrails" that guide the AI to produce the exact output needed.

- **Format**: Specifying "Strict Markdown" and the exact "front matter" fields is essential for Hugo compatibility.
- **Focus**: Defining the audience ("for a certification exam") and scope ("avoid overly esoteric details") prevents the AI from generating an overly long or irrelevant article.
- **Structure**: Requesting clear headings and a logical flow ensures the article is well-organized and easy for a human to read.
{{% /alert %}}

{{% alert title="4. The Final 'Wrapper' Instruction: 'Ensure that...'" color="success" %}}
This is a meta-instruction for chat interfaces. It asks the model to wrap its response in a code block, ensuring the raw text can be copied perfectly, preserving all formatting.
{{% /alert %}}

## Conclusion

By using a detailed, structured prompt like this one, you move from "asking" the AI for information to "instructing" it to perform a specific, repeatable task. This template can be easily adapted for different topics.

> **Next Step:** For generating multiple articles in a single batch, see the evolution of this template in [Prompt 002 - Tpl TechDoc topic multiples]({{< relref "prompt-template-multiples-topic-individual.md" >}}).
