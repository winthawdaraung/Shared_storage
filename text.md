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

# CHAPTER 3
# SYSTEM DESIGN AND IMPLEMENTATION

## 3.1 Development Approach and System Evolution

The AI Developer Agent for GitLab was developed iteratively through several architectural approaches. The initial implementation used a stage-based workflow in which separate agent stages processed development tasks and transferred information through artifacts. Subsequent development introduced a custom orchestration mechanism that coordinated tool calls and controlled the execution of development activities. The project later incorporated MCP-based tool integration before transitioning to the GitHub Copilot SDK.

Although the internal architecture changed across these implementations, the GitLab webhook remained the common entry point for receiving relevant issue events, including events associated with the agent or its assignment. This allowed the system to initiate its workflow in response to activities in the GitLab environment.

The main development phases were:

1. Prototype 1.0: stage-based execution and artifact handoff.
2. Tool-calling implementation: custom orchestration and iterative tool execution.
3. Prototype 2.0: integration of orchestration into a multi-stage development workflow.
4. MCP integration: access to external and custom tools through MCP servers.
5. Copilot SDK migration: replacement of most of the previous custom agent implementation with the SDK-based approach.

These phases represent successive approaches to implementing the same project rather than independent projects.

## 3.2 Prototype 1.0: Stage-Based Architecture

### 3.2.1 Workflow Overview

The initial prototype used GitLab Runner to execute the agent workflow through separate stages. Each stage was responsible for a specific activity, and information was passed between stages through artifacts.

This architecture separated task intake from clarification and subsequent development activities. Instead of requiring every stage to independently retrieve all information from GitLab, the workflow allowed one stage to prepare context for the next stage.

### 3.2.2 Intake Stage

The intake stage was responsible for obtaining the context of a GitLab issue. The collected information included the issue title, description, comments, and other relevant issue details.

After collecting the information, the stage prepared an artifact containing the issue context. This artifact served as a handoff mechanism for subsequent stages, allowing them to continue processing the task using the collected information.

### 3.2.3 Clarification Stage

The clarification stage loaded the artifact generated by the intake stage and examined the available issue context. Its purpose was to clarify the task requirements and enrich the context with additional relevant information, including context from relevant source files.

Separating clarification from intake allowed the system to distinguish between collecting the original task information and interpreting or expanding that information before implementation.

### 3.2.4 Limitations of the Stage-Based Approach

The stage-based design provided a structured execution sequence and explicit information handoffs. However, the workflow was organized around separate stages rather than a unified tool-calling loop. Later development introduced an orchestration mechanism that coordinated tool execution and allowed the agent to respond to intermediate results, including validation failures.

The subsequent architecture therefore shifted the emphasis from transferring context between predetermined stages to coordinating multiple actions within an iterative execution process.

## 3.3 Custom Orchestration and Tool Calling

### 3.3.1 Orchestration Architecture

The tool-calling implementation introduced an orchestrator and a main tool-calling loop. This architecture allowed the agent to perform multiple development activities within a single stage rather than requiring every activity to be implemented as a separate GitLab Runner stage.

The implementation combined two mechanisms: model-selected function calls and application-controlled execution logic. The language model generated outputs and selected tools through function calling, while the orchestrator controlled the execution sequence, processed tool results, and determined subsequent actions.

This division of responsibility enabled the system to combine the flexibility of model-driven tool selection with explicit application-level control over workflow progression.

### 3.3.2 Iterative Execution Process

The orchestration process supported iterative execution. After a tool was called, the system examined the result and determined which action should follow. The next action depended on the current execution state, the outcome of the preceding operation, and the orchestration rules.

For example, when the agent invoked the `generate_changes` tool, the language model generated the proposed code changes. The orchestrator could then direct the workflow to `apply_changes`, followed by `run_validation`.

If validation succeeded, the workflow could proceed to a review operation. If validation failed, the orchestrator could invoke a debugging operation and retry the relevant steps. The orchestrator also enforced a retry limit to prevent the workflow from repeating failed operations indefinitely. When the retry limit was exceeded, control could be transferred to another stage according to the application's workflow logic.

This process enabled the agent to respond to execution results instead of following a completely predetermined sequence.

### 3.3.3 Division of Responsibilities Between the Model and Orchestrator

The implementation relied on both model-driven decisions and application-controlled logic.

The language model was responsible for generating code and selecting appropriate tools through function calling. The orchestrator managed tool execution, coordinated the sequence of actions, evaluated relevant results, and enforced workflow constraints such as retry limits.

This arrangement allowed the agent to adapt its actions to intermediate results while maintaining explicit control over the overall execution process.

## 3.4 Prototype 2.0: Integrated Development Workflow

### 3.4.1 Workflow Architecture

Prototype 2.0 integrated the agent into a six-stage workflow executed through GitLab Runner:

1. Intake and harness selection.
2. Clarification.
3. Implementation.
4. Apply.
5. Test.
6. Open merge request.

The intake and harness-selection activities were grouped into the first stage. Harness selection involved identifying relevant skills for the development task.

The clarification stage processed the task context and prepared the information required for implementation. The implementation stage handled code-generation activities, while the apply stage applied the proposed changes. The test stage performed testing, and the final stage handled opening a merge request.

This architecture combined the separation of responsibilities provided by a staged workflow with the more flexible execution mechanisms developed in the tool-calling implementation.

### 3.4.2 Integration of the Custom Orchestrator

The student's orchestration implementation was incorporated into the merged system's clarification and implementation stages. The integrated orchestrator operated with a restricted set of tools appropriate to each stage.

This approach allowed the system to reuse orchestration logic while preserving stage-specific responsibilities. Rather than giving every stage unrestricted access to all available tools, the implementation could expose only the tools needed for the activity being performed.

The integration demonstrated how an iterative tool-calling mechanism could be incorporated into a larger staged workflow.

## 3.5 MCP-Based Tool Integration

### 3.5.1 Motivation and Architecture

The next development phase explored the Model Context Protocol (MCP) as a mechanism for providing the agent with access to development tools and information sources.

Initially, the team adapted the existing tools so they could be registered and exposed through its own MCP server. This approach provided a common interface through which the agent could discover and use available capabilities.

The implementation subsequently explored external MCP servers alongside the team's custom MCP server. The orchestration approach remained similar to the earlier tool-calling implementation, but the mechanism for exposing and accessing tools was extended through MCP.

### 3.5.2 MCP Servers and Custom Tools

The MCP-based implementation explored the following integrations:

- **GitLab MCP:** Access to GitLab-related functionality.
- **Filesystem MCP:** Access to filesystem-related operations.
- **RAGFlow MCP:** Access to knowledge retrieval capabilities.
- **Custom MCP server:** Exposure of project-specific tools, including `discover_and_select_skills`, `run_validation`, and `execute_cmd`.

These integrations were intended to extend the agent's ability to access repository-related resources, retrieve relevant knowledge, select appropriate skills, and execute development operations.

The use of MCP provided a standardized mechanism for exposing tools to the agent. However, the availability of a tool did not by itself determine when the tool should be used. The agent's tool selection and the application's orchestration logic continued to play important roles in coordinating the workflow.

## 3.6 Migration to the GitHub Copilot SDK

The project subsequently adopted the GitHub Copilot SDK, replacing most of the previous custom agent implementation.

In the earlier implementations, the team had developed a custom orchestration mechanism and integrated tool-calling behavior directly into the application. In the SDK-based approach, the implementation relied on the Copilot SDK, prompts, and MCP servers to provide the agent's execution capabilities.

This transition changed the responsibilities of the surrounding application. Rather than maintaining the previous agent implementation in its entirety, the project adopted the SDK as the primary agent framework while using prompts and MCP integrations to provide the required task context and tool access.

The migration represented a change in the implementation approach and created a new basis for testing the agent workflow. Its practical effectiveness and limitations should be evaluated using the results obtained from the SDK-based implementation.

## 3.7 Individual Contributions

The student's contributions covered several aspects of the system's development.

First, the student worked on webhook handling, context intake, and clarification in the initial prototype. These components supported the reception of GitLab issue events, collection of task information, and preparation of context for subsequent processing.

Second, the student developed orchestration logic and tool/function-calling behavior for the agent workflow. This work covered the coordination of code generation, application of changes, validation, review, debugging, and iterative execution. The orchestrator combined model-selected tool calls with application-controlled transitions and retry limits.

Third, the student contributed to the integration of the orchestration implementation into the merged Prototype 2.0, particularly in the clarification and implementation stages, using stage-specific tool sets.

Finally, the student participated in the MCP-based approach and the transition to the Copilot SDK. These activities involved extending tool access through MCP servers and adopting a different framework for agent execution.

The project was undertaken collaboratively, with responsibilities distributed between the student and another intern. The contributions described in this section focus on the student's work, while the overall architecture and project outcomes reflect the team's combined efforts.
