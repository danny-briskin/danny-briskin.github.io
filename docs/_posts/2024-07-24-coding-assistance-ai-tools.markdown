---
layout: post
title: "Coding assistance AI tools"
date:   2024-07-24 07:40:30 -0400
categories: ai, copilot, coding assistance 
tags: ai, copilot, coding assistance 
---
![](/images/project_documentation.jpg)

Danny Briskin, Quality Engineering Practice Manager


# Introduction
In recent years, artificial intelligence has significantly transformed the landscape of software development by introducing AI coding assistance tools. These tools, powered by advanced machine learning algorithms, are designed to enhance productivity, reduce errors, and streamline the coding process. By providing real-time suggestions, auto-completions, and intelligent code analysis, AI coding assistants are revolutionizing the way developers write and optimize code. This technology not only accelerates the development process but also empowers developers to focus on more complex problem-solving tasks, thereby fostering innovation and efficiency. 

Are those tools really that powerful? Let's figure that out.

# AI powered tools 
There are plenty of tools that can be used for software development process, starting from idea generation to code generation and testing. Here is a list of most know ones
* Tools for ideas generation and concepts creation
** [ChatGPT](https://openai.com/chatgpt/) - a universal assistant
** [GlueCharm](https://gluecharm.com/) - user story creation tool
** [Frase](https://www.frase.io/) - website content generation
** [Miro](https://miro.com/product-overview/) - product diagramming and prototyping 
* Tools for coding assistance
** [Copilot](https://copilot.microsoft.com/) - code completion tool
** [ChatGPT models](https://openai.com/chatgpt/) - some models for code generation
** [Gemini](https://gemini.google.com/) - text generation tool
** [Vertex AI](https://cloud.google.com/vertex-ai) - code and text generation tool
** [Gemini Code Assist](https://cloud.google.com/products/gemini/code-assist) - code generation
** [Llama](https://github.com/meta-llama/llama/tree/main) - code and text generation
** [Tabnine](https://www.tabnine.com/) - code completion 
** [Codeium](https://codeium.com) - code and text generation

In this article I will concentrate on coding assistance tools only.

# Pros of AI tool usage
AI coding tools have become an integral part of modern software development, offering a range of benefits:
* **Affordability:** Many AI tools provide a trial period or even a free tier, making them accessible to a broad audience.
* **Deployment Options:** These tools often offer cloud-based solutions and on-premises versions to suit different organizational needs.
* **Ease of Use:** Installation is typically straightforward, and most tools support plugins for popular IDEs, integrating seamlessly into existing workflows.
* **Contextual Suggestions:** AI tools utilize predefined models and analyze your codebase, including comments, to provide context-aware suggestions.
* **Security:** Many AI tools are tested for security compliance and have relevant certifications.
* **Specific Use Cases:**
** **Function Documentation and Comments:** AI tools are extremely useful for generating javadoc, docstrings, and in-code comments, especially for non-native English speakers.
** **Code Predictions:** These tools are helpful for generating boilerplate code, but when the code is supposed to be more sophisticated, the usefulness of tools becomes questionable.
** **Code Completion:** AI code completion is useful but built-in IDE code completion tools are currently more robust and faster.
**	**Git Commit Messages:** Some tools can analyze changes between commits, though they are not very robust.
** **Code Explanation:** AI tools provide valuable insights for junior developers, making them very useful in this aspect.
** **Unit Test Generation:** Many tools excel in generating unit tests, offering great value there.
** **Code refactoring:** - tools are quite useful, but developer needs to put a lot of efforts to explain what is needed and the result is questionable in most cases.


# Cons of AI Tool Usage
Despite their advantages, AI coding tools have several drawbacks:
* **Immature Plugins:** IDE plugins need improvement to become more reliable and feature-rich.
* **Performance Issues:** The analysis required for suggestions can lead to slow performance, affecting productivity.
* **Data Privacy Concerns:** There is a risk of data leaks, as user inputs may be used for model training by AI tool creators.
* **Lack of Open Source:** Most tools are not open source, raising concerns about potential information leaks, even with on-premises solutions.
* **Training Data Quality:** The models are trained on unknown datasets, which may not adhere to best coding practices.
* **Limited of Precise Tuning:** While AI tools can use your codebase for suggestions, they often focus on the current file rather than the entire project.
* **Annoying Behavior:** AI tools may repeatedly suggest inappropriate code, causing frustration for developers.

# General AI Tools Concerns
* **Reliability:** AI tools can generate erroneous or unsafe code, necessitating thorough code reviews and undermining the idea of "machine-generated code." Complex tools may create code that contradicts programming principles.
* **Code Support and Maintenance:** AI-generated code may be overcomplicated, use uncommon approaches, and vary among developers, making collaboration challenging. Refactoring AI-generated code can be difficult.
* **Continuous Improvement:** AI is trained (at least it is supposed to be) on syntactically correct elements, but this doesn't guarantee optimal performance or maintainability.
* **Loss of Control:** Developers may feel disconnected from the code, leading to their skill deterioration and reduced ability to handle AI errors.
* **Habits and Motor Memory:** Experienced developers rely on motor memory for IDE tasks. AI tools disrupt this pattern, requiring more time for decision-making and returning to the original workflow.
* **Incorrect Suggestions:** AI tools may offer irrelevant code suggestions that appear correct, leading to potential coding errors that regular code completion tools would avoid.


# Summary

AI coding assistance tools have revolutionized software development by enhancing productivity, reducing errors, and streamlining the coding process. These tools provide real-time suggestions and intelligent code analysis, allowing developers to focus on complex problem-solving. AI tools offer several benefits, including affordability, ease of use, and contextual suggestions. They excel in specific tasks such as function documentation, code predictions, unit test generation, and code explanations, particularly aiding non-native English speakers and junior developers.

However, AI tools also have significant drawbacks. They can suffer from performance issues, data privacy concerns, and immature plugins. Most tools are not open source, leading to potential information leaks and questionable training data quality. AI-generated code can be unreliable, challenging to maintain, and often requires thorough review. Developers may feel a loss of control over their code, and AI tools can disrupt their workflow and motor memory. Additionally, incorrect suggestions can lead to coding errors that are not easily caught.

Overall, while AI tools can greatly assist in software development, their limitations and risks must be carefully managed to ensure reliable and efficient coding practices.