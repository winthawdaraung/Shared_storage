# CHAPTER 1
# INTRODUCTION

## 1.1 Background and Significance

Software development involves multiple activities, including understanding requirements, examining existing source code, implementing changes, testing functionality, and reviewing the resulting modifications. These activities require developers to understand the context of a task, select appropriate tools, and evaluate the results of each action before proceeding. As large language models (LLMs) become increasingly capable of assisting with programming tasks, AI developer agents offer an approach to integrating these activities into a more coordinated development workflow.

An AI developer agent differs from a basic code-generation assistant in that it can be designed to interact with external tools, retrieve relevant information, execute development tasks, inspect intermediate results, and repeat actions when necessary. However, building such an agent requires more than connecting a language model to a source code repository. The surrounding system must manage task context, coordinate execution, control tool access, and handle the results returned by tools. These requirements make workflow orchestration and tool integration important aspects of AI-assisted software engineering.

The project, entitled "AI Developer Agent for GitLab," was undertaken during a cooperative education internship at TDET. The project focused on developing an AI-assisted workflow integrated with GitLab to support software development tasks. Before the official project was assigned, the development team investigated harness engineering to explore the systems and mechanisms surrounding AI agent execution.

The initial implementation used GitLab Runner to execute a workflow consisting of four stages: intake, clarification, implementation, and apply. The project subsequently evolved to incorporate tool and function calling, enabling the agent to interact with available tools as part of its execution process. The orchestration approach combined model-selected function calls with application-controlled logic, allowing tool execution and workflow progression to be coordinated by the system.

The project also explored the Model Context Protocol (MCP) as a mechanism for connecting the agent to external tools and information sources. The tools investigated included GitLab MCP, Filesystem MCP, RAGFlow MCP for knowledge retrieval, and custom tools. As development progressed, the team adopted the GitHub Copilot SDK, replacing the previous agent implementation.

The significance of this project lies in the engineering work required to coordinate AI-assisted software development activities within an existing development environment. This includes managing task context, supporting clarification, integrating tools, orchestrating implementation activities, and enabling the agent to inspect results and continue or retry actions as appropriate. The project also provides an opportunity to examine the practical considerations involved in evolving an AI agent architecture across successive prototypes and adopting a different agent SDK.

## 1.2 Research Objectives

The objectives of this project are as follows:

1. To design and develop an AI developer agent integrated with GitLab for assisting with software development workflows.
2. To implement workflow orchestration that coordinates task intake, clarification, implementation, and related development activities.
3. To integrate tool and function calling mechanisms that allow the agent to interact with external tools through a combination of model-selected actions and application-controlled logic.
4. To investigate and integrate MCP-based tools and knowledge retrieval capabilities to support the agent's access to relevant development resources.
5. To adopt and evaluate the GitHub Copilot SDK as the agent execution framework.
6. To evaluate the implemented workflow through functional testing and examination of its behavior during tool execution and iterative development tasks.

## 1.3 Scope of the Project

The project focuses on the design, implementation, and evolution of an AI developer agent within a GitLab-based software development environment.

The scope includes the following areas:

1. **Workflow architecture:** Designing the overall system architecture and coordinating the responsibilities of the different workflow stages.
2. **Task intake and clarification:** Handling incoming development tasks, preparing relevant context, and supporting clarification when task requirements need further explanation.
3. **Workflow orchestration:** Coordinating the agent's execution, including tool selection, tool invocation, processing returned results, and continuing or retrying actions when appropriate.
4. **Tool and function calling:** Integrating model-selected function calls with application-controlled execution logic.
5. **External tool integration:** Investigating and integrating GitLab MCP, Filesystem MCP, RAGFlow MCP for knowledge retrieval, and custom tools.
6. **Agent framework migration:** Replacing the previous agent implementation with the GitHub Copilot SDK.
7. **Testing and evaluation:** Examining the behavior of the implemented workflow and identifying limitations and opportunities for improvement.

The project was developed collaboratively. Individual responsibilities varied across development stages. The student's contributions primarily involved webhook handling, context intake, clarification, orchestration, and tool integration across the agent workflow. Parts of the student's orchestration implementation were incorporated into the merged system's clarification and implementation stages. Other development activities were undertaken collaboratively or by the other intern.

## 1.4 Methodology

The project followed an iterative software development process involving investigation, architectural design, implementation, integration, and testing.

The methodology consisted of the following activities:

1. **Technology investigation:** Investigating harness engineering and relevant technologies for developing and coordinating AI agent workflows.
2. **System design:** Designing the system architecture collaboratively and dividing implementation responsibilities among team members.
3. **Initial prototype development:** Developing a GitLab Runner-based workflow consisting of intake, clarification, implementation, and apply stages.
4. **Orchestration development:** Implementing and integrating tool/function-calling behavior using both model-selected actions and application-controlled logic.
5. **Tool integration:** Investigating MCP-based integration with GitLab, the filesystem, RAGFlow, and custom tools.
6. **Framework migration:** Adopting the GitHub Copilot SDK to replace the previous agent implementation.
7. **Testing and refinement:** Examining tool outputs and execution results, reviewing generated changes, and refining the workflow through iterative execution and retries where necessary.

The project evolved through successive prototypes, allowing the architecture and implementation approach to be revised as development requirements changed. Testing and evaluation were used to investigate the behavior of the resulting system and identify areas requiring further improvement.

## 1.5 Expected Benefits

The project is expected to provide the following benefits:

1. **Improved workflow coordination:** A structured approach to coordinating AI-assisted software development activities within GitLab.
2. **Tool-assisted development:** The ability to connect agent reasoning and execution with external development tools and information sources.
3. **Iterative task execution:** Support for inspecting intermediate results and continuing or retrying actions during development tasks.
4. **Improved access to development context:** Integration with repository-related tools and knowledge retrieval mechanisms to provide relevant information to the agent.
5. **A foundation for further development:** An extensible architecture that can be refined through additional testing, tool integration, and evaluation of agent execution frameworks.

The extent to which these benefits are achieved will be examined through the implementation and evaluation described in the subsequent chapters.
