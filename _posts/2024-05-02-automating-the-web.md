---
layout: post
title:  Automating The Web: Browser Agents for Web Automation
description: Creating browser agents for robotic process automation (RPA) and performant web research. 
date:   2024-05-02 15:01:35 +0300
image:  '/images/general/browser-agents.jpg'
tags:   [ai, research]
---

The World Wide Web has long been a hub of online activity. From commerce to social networking, entertainment to education, the web touches nearly every aspect of modern life. As the web has grown in scale and complexity over the decades, so has the need for tools and techniques to automate interactions with web services.

Robotic Process Automation (RPA) is revolutionizing the way businesses operate by employing software robots or AI-powered digital workers to automate repetitive, rule-based tasks. By mimicking human actions and interacting with IT systems, RPA tools can efficiently perform a wide range of activities, such as data entry, invoice processing, and customer onboarding. The technology's importance lies in its ability to streamline operations, reduce costs, improve productivity, and allow human workers to focus on higher-value tasks requiring creativity and decision-making skills. As organizations recognize the benefits of RPA in driving digital transformation, the demand for this technology is growing significantly.

![Statistics Chart](/images/general/browser-agents1.png)

The North American RPA market is projected to grow from $590.8M in 2020 to $703.4M by 2030, with a CAGR of 37.6% between 2022 and 2030, according to Grand View Research. This strong growth forecast highlights the increasing adoption of RPA across industries as businesses seek to optimize processes and reduce operational costs. Advancements in AI and machine learning will further enhance RPA capabilities, enabling the automation of more complex tasks. However, successful RPA implementation requires careful planning and change management to ensure seamless integration and collaboration between human workers and software robots.

As generative AI enables intelligent systems to extend software capabilities from simple classification to generative content, intelligent decision-making, and action-taking, new paradigms of web automation emerge. Browser agents, powered by large language models (LLMs), have the potential to autonomously navigate websites, extract information, fill out forms, and complete complex multi-step tasks while adapting to the dynamic nature of the modern web. These AI-driven agents have unprecedented potential to automate manual online processes, enhance accessibility, and unlock new possibilities for interacting with the vast resources of the web at scale.

For the past 5 months, I worked on designing better ways of automating the web: a general-purpose browser agent and a high-performance web-scraping browser agent.

First, I worked on creating a general-purpose browser agent that responds to natural language and executes tasks through a headless browser instance. It does this through a step-by-step decision-making and action-taking architecture, utilizing multi-modal context analysis to ideate, scope, and execute incremental browser actions. It’s capable of traversing the web, selecting web elements to interact with, executing incremental actions in favor of completing the overall task, and repeating this loop until the task has been adequately completed.

This system primarily uses GPT-4 Turbo and GPT-4 Vision for multi-modal context capabilities, and is modeled after the following architecture:

![Browser Agent Flowchart](/images/general/browser-agents1.png)

The architecture enables the browser agent to incrementally analyze the current web page, decide on the optimal next action, execute it, and repeat this perceive-think-act loop until the task is complete.

The agent first processes the user's natural language task description. It then enters a loop of perceiving the current page state via the browser Document Object Model (DOM) and a screenshot, using an LLM to analyze the multi-modal context and determine the next best action to progress the task, executing that action through the browser, and iterating until done. 

After making significant progress on this implementation, a few clear problems arose: (a) most valuable information and actions are auth-enabled, which requires effective cookie and proxy management and (b) this architecture was effective, but slow and expensive. Ultimately, the goal was to build an effective, commercially-viable tool to help automate the web, and that requires good use cases, performance, and unit economics.

That led to building a high-performance browser agent, tailored for holistic web-scraping and research. Though existing platforms such as [Perplexity](https://perplexity.ai) and [Exa](https://exa.ai) are high-quality ways of intelligently traversing the web, they do not address the use case of holistic web research. Perplexity primarily acts as a consumer search engine, requiring that it acts as a low-latency answering machine for the average search query. Exa is a fantastic developer tool to retrieve web results through neural search queries, but it’s modular for developer use cases rather than for research. Similar to [Arc Search](https://arc.net)’s “Browse For Me” feature, I wanted to create a web research tool that generated extremely holistic, long-form answers that compiled relevant search engine answers from various sources progressively and quickly. This could be used to help consumers understand current events and politics from multiple sources, politicians to conduct holistic campaign research, business owners to assist with competitive research, and anything else that requires traversing through multiple search results on the web.

This new browser agent is architected as follows: first, a user submits a search query which triggers results retrieval from multiple sources: Google and Exa. A headless browser instance is created to render the first page of search results and scrape all relevant links as sources. Recursively, each link is rendered, scraped, and synthesized as content that is progressively added to a data structure holding all of the content pertaining to the user query. Each page is also processed for relevant links based on the user query, and recursively processed and synthesized for natural “rabbit-holes”. This recursive processing is run in parallel and is aborted after a certain limit is reached, all while the synthesized content is being made transparent to the user for a progressive streaming experience. After web retrieval is complete, the processed results will be synthesized once more for the user, providing a clearly articulated answer.

The first version of this browser research agent has been built and optimized, awaiting testing from first users.

In conclusion, automating the web is becoming increasingly crucial in our digital world. As the web continues to expand in scale and complexity, manual interaction grows more inefficient and time-consuming. AI-powered browser agents have the potential to revolutionize how we engage with and extract value from the immense resources available online. From automating repetitive tasks to enabling comprehensive research, these intelligent agents can empower users in numerous ways. By leveraging advanced language models and adaptive architectures, browser agents can navigate the web with human-like understanding and flexibility.

If you're interested in trying out the early developments of these browser agents, please reach out to me at [https://calix.dev](https://calix.dev) for access!
