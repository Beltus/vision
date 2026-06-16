---
layout: single
published: true
title: "Key Insights from the Coursera IBM Agentic AI with LangChain and LangGraph"
collection: sc
author_profile: true
read_time: true
categories: [Blog]
excerpt: "
A breakdown of the key concepts from the IBM Agentic AI with LangChain and LangGraph course — including LangGraph's core components, agentic strategies, and Agentic RAG."
header:
    overlay_image: "https://beltus.github.io/vision/assets/images/galaxy.png"
    teaser: "https://cdn-images-1.medium.com/max/1200/1*cecsdvLJXw97A7f1w6T9pg.jpeg"
comments: true
toc:
toc_sticky:
---

# Key Insights from the Coursera IBM Agentic AI with LangChain and LangGraph

![](https://cdn-images-1.medium.com/max/1200/1*cecsdvLJXw97A7f1w6T9pg.jpeg)

## A Little Background

The [IBM Agentic AI with LangChain and LangGraph](https://www.coursera.org/learn/agentic-ai-with-langchain-and-langgraph/home/info) course is the 7th course of the [IBM RAG and Agentic AI Professional specialization](https://www.coursera.org/professional-certificates/ibm-rag-and-agentic-ai#courses), focusing mainly on how to build multi-agent systems using LangGraph.

It builds on the previous [IBM Fundamentals of Building AI Agents](https://www.coursera.org/learn/fundamentals-of-building-ai-agents?specialization=ibm-rag-and-agentic-ai) course on building AI agents using LangChain. 

If you're interested to know more about the previous course, [click here](https://medium.com/@beltusnkwawir/my-thoughts-on-coursera-ibm-fundamentals-of-building-ai-agents-5cc69ff108c8) for a 3-minute read on my personal thoughts after completing it.

While the previous course shows us how to use LangChain to build an autonomous AI agent, this course moves us one step forward towards Agentic AI.

The objective of this post is to highlight the key concepts covered in the course and provide simple, easy-to-understand definitions and explanations. It is grounded not only in the content of the course, but also in additional external sources and my own understanding.

So after reading this post, you will have an overview of the main skeleton of the course and knowledge of the central concepts that make it worth taking.

So let's dive in…

---

## What's the Difference Between AI Agent and Agentic AI?

**An AI Agent** is an autonomous system or single entity designed to accomplish a specific, narrow, well-defined task with minimal human intervention.

**Agentic AI** is a team of AI agents working together to achieve a goal. The agents coordinate tasks, exchange information, adapt roles dynamically, and share memory.

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*7IUhBqdEeTZurIRWCcpJlA.png)
Table: Differences between AI Agent and Agentic AI


LangChain and especially LangGraph are the 2 main frameworks used throughout the course to demonstrate how to build these multi-agent systems.

The course focuses for the most part on LangGraph framework.

So,

---

## What's LangChain and LangGraph?

**LangChain** is a Python framework for building applications around large language models (LLMs). It achieves this by executing a sequence of actions in a chain. Key components of LangChain are Memory, Prompt, LLM, Tools, and Agent.

**LangGraph** is a specialized library within the LangChain ecosystem for building stateful, multi-agent applications. It models complex agent workflows as graphs — so its core components are nodes, edges, and persistent state.

> If you remember one thing about LangGraph, it is this: **It follows a graph structure.**

---

## Core Components of LangGraph

![](https://miro.medium.com/v2/resize:fit:720/format:webp/0*KBZmV3QUNZ5CFxNV.png)

The course dives deep into building a LangGraph application using its core components:

- **State**: The central concept in LangGraph. It represents all the information that flows through your application workflow.
- **Nodes**: The core fundamental unit of action. A node represents a specific task, function, computation, or operation the AI agent needs to perform. It takes in state as input, performs some operation, and outputs updates to the state. Examples: fetch data from an API, process information, or generate a response.
- **Edges**: Define the connections between nodes and guide execution flow. Edges can be:
  - **Direct** — always go from node A to node B.
  - **Conditional** — choose the next node based on the current state. These are the decision-making points in the workflow.

> **NOTE:** Two special default nodes required in any LangGraph workflow are **START** and **END** — they trigger the beginning and end of a workflow.

---

## Agentic AI Approaches

The course covers 3 main agentic approaches:

### 1. Reflection Agents
![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*OFhnocS4bmtr02RJwMufQg.png)
Figure: Basic reflection agent [mechanics](https://www.blog.langchain.com/reflection-agents/)

Reflection strategy aims at improving the quality and accuracy of outputs generated by AI agents. Reflection agents use internal critique to refine outputs — they can evaluate their own output, identify weaknesses, and iteratively improve through feedback loops.

The basic reflection approach is made up of 2 LLM calls:
- A **generator** that responds to the user prompt.
- A **reflector** (like a critic or teacher) that analyzes the generator's response, identifies flaws, and provides constructive feedback.

This creates a generator-reflector loop that runs for a fixed number of iterations until a final polished response is produced.

Reflection agents are well-suited for content generation, creative, and open-ended tasks.

**Drawback:** Since the reflection step isn't grounded in any external process, the final result may not be significantly better than the original.

---

### 2. Reflexion Agent

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*PBX-v8Lk5FkAUOsmUFoydw.png)

Reflexion agent strategy builds on reflection agents by iteratively improving responses using self-critiques, external tools, and citations. A Reflexion agent is made up of:

- A **responder** that generates a response along with additional actions in the form of [search queries](https://www.blog.langchain.com/reflection-agents/).
- A **revisor** that explicitly critiques each response, grounds its criticism in external data, refines the response, integrates tool outputs, and adds references.

---

### 3. ReAct Agents

![](https://miro.medium.com/v2/resize:fit:720/format:webp/0*CDOmxQf9UMbL7ZRh.gif)
Figure: ReAct [Pattern](https://www.dailydoseofds.com/ai-agents-crash-course-part-10-with-implementation/)

**ReAct** stands for **Reasoning + Acting**. It is a strategy designed for tasks that require step-by-step reasoning, combining 3 key concepts:

- **Reasoning**: The agent thinks through the problem step by step.
- **Acting**: The agent uses external tools (e.g. search engines like Tavily, calculators, APIs) to gather information or perform actions.
- **Observing**: The agent processes the results of its actions and integrates them into its reasoning.

> **Bonus:** [Tavily](https://www.tavily.com/) is a search tool that can be configured and invoked to enhance AI responses with external, real-time data.

---

## Example Use-Cases of Multi-Agent Systems

- Automated market research reports
- Customer support automation
- Legal contract review

---

## Agentic Retrieval Augmented Generation (RAG)

**[RAG](https://www.datacamp.com/blog/what-is-retrieval-augmented-generation-rag?utm_cid=19589720821&utm_aid=152984010854&utm_campaign=230119_1-ps-other%7Edsa-tofu%7Eall_2-b2c_3-emea_4-prc_5-na_6-na_7-le_8-pdsh-go_9-nb-e_10-na_11-na&utm_loc=9197848-&utm_mtd=-c&utm_kw=&utm_source=google&utm_medium=paid_search&utm_content=ps-other%7Eemea-en%7Edsa%7Etofu%7Eblog%7Eartificial-intelligence&gad_source=1&gad_campaignid=19589720821&gbraid=0AAAAADQ9WsHLtN7xqinfmy6H8BDrYwFFm&gclid=Cj0KCQiAkPzLBhD4ARIsAGfah8jyk-wAoJm2Y4Lj2HJvo_cEqtZ49X3caFRVfps67Ambfeni_MKw3ZkaApA7EALw_wcB)** is a technique that enhances LLMs by integrating them with external data sources. Without RAG, the LLM is the only source of knowledge — leading to hallucinations, limited knowledge, and generic responses. RAG combines the generative capabilities of LLMs with precise information retrieval, producing responses that are more accurate, up-to-date, and relevant.

**Agentic RAG** enhances RAG by incorporating an intelligent decision-making component that selects the most relevant data source based on the query context. An LLM acts as the decision-making agent, boosting accuracy, adaptability, and real-world applicability across industries.

---

## Takeaways

- LangGraph represents agents as graphs of states and nodes.
- State (often message history) flows through nodes (functions or LLM calls) linked by edges with conditional logic.
- Multi-agent systems organize agents into specialized teams that can collaborate.
- LangGraph, CrewAI, AutoGen, and IBM BeeAI are frameworks for building, orchestrating, and managing multi-agent systems.
- Agentic RAG is RAG with an intelligent decision-making component that selects the most relevant data sources based on query context.

---

Here’s a cute little certificate I was awarded for completing the course


![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*lNed2XcKf6WvQDsw1TeaxA.png)

Thanks for reading! If you found this article useful, subscribe — I'll be sharing similar content as I dive into the next IBM RAG and Agentic AI course.

You can also connect with me on [LinkedIn](https://www.linkedin.com/in/beltus).

