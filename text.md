# ABSTRACT

The increasing use of large language models (LLMs) in software development has created opportunities to automate and coordinate tasks such as understanding requirements, generating code, testing, and reviewing changes. However, developing an AI developer agent requires more than code generation; it also requires effective context management, tool integration, workflow orchestration, and error handling.

This project, entitled **“AI Developer Agent for GitLab,”** was carried out during a cooperative education internship at TDET. The objective was to develop an AI-assisted software development workflow integrated with GitLab. The project evolved through several implementation approaches, beginning with a GitLab Runner-based pipeline that separated task intake, clarification, implementation, and application of changes into distinct stages. Subsequent development introduced a custom orchestration mechanism combining model-selected function calls with application-controlled execution logic, enabling the agent to process tool results, perform validation, and retry operations when necessary.

The project also integrated the Model Context Protocol (MCP) to provide access to GitLab, filesystem operations, knowledge retrieval through RAGFlow, and project-specific tools. In a later development phase, the GitHub Copilot SDK replaced most of the previous custom agent implementation. The resulting system was evaluated through functional and integration testing, including validation failure and retry scenarios, MCP tool usage, code generation, testing, and merge request creation.

The results demonstrated that the implemented agent could support the tested end-to-end workflow from receiving a GitLab issue to generating code, running tests, and opening a merge request. The project provided practical experience in AI agent orchestration, tool integration, and the development of AI-assisted software engineering workflows. Further quantitative evaluation is needed to assess the system’s efficiency, reliability, and code quality across a broader range of tasks.

**Keywords:** AI developer agent, GitLab, large language model, workflow orchestration, function calling, Model Context Protocol, GitHub Copilot SDK

# ACKNOWLEDGEMENTS

I would like to express my sincere gratitude to TDET for providing me with the opportunity to undertake this cooperative education internship and gain practical experience in software engineering and AI-assisted development.

I am particularly grateful to my workplace supervisor and the members of the development team for their guidance, feedback, and support throughout the internship. Their collaboration and technical discussions helped me deepen my understanding of AI agent development, workflow orchestration, and software engineering practices. I would also like to thank my fellow intern for the collaboration and shared effort in designing, implementing, and testing the AI Developer Agent for GitLab.

I would like to extend my appreciation to King Mongkut’s Institute of Technology Ladkrabang (KMITL), the Computer Engineering International Program, and the faculty members who supported my cooperative education experience. Their academic guidance and the knowledge I gained throughout my studies provided a valuable foundation for applying engineering concepts to a practical project.

Finally, I am thankful to my family and everyone who encouraged and supported me throughout this internship. Their support has been an important source of motivation during my academic journey.

This internship has been a valuable learning experience, allowing me to apply my technical knowledge, develop new skills, and gain a better understanding of the challenges involved in building AI-assisted software development systems.

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
# CHAPTER 2
# THEORETICAL BACKGROUND AND RELATED TECHNOLOGIES

## 2.1 Related Theories

### 2.1.1 Artificial Intelligence Agents and Large Language Models

Large language models (LLMs) are machine learning models capable of processing and generating natural language and other supported forms of input and output. In software engineering, LLMs can assist with understanding programming requirements, generating source code, explaining existing implementations, and suggesting solutions to technical problems.

An AI agent extends the use of an LLM by connecting model-generated decisions with actions performed through software tools and external systems. Instead of generating a response only once, an agent can follow a multi-step process in which it receives a task, examines available information, selects an action, observes the result, and determines what to do next.

For software development tasks, these actions may include retrieving source files, searching documentation, modifying code, executing commands, running tests, and examining validation results. The outputs of these actions can then inform subsequent decisions.

The effectiveness of an AI agent therefore depends not only on the language model but also on the mechanisms used to provide context, execute actions, manage intermediate results, and control the overall workflow.

In the AI Developer Agent for GitLab project, the language model provides code-generation and tool-selection capabilities, while the surrounding application or agent framework coordinates development activities. This separation allows the system to combine language-model capabilities with deterministic application logic and external development tools.

### 2.1.2 Harness Engineering

Harness engineering concerns the design and implementation of the surrounding software infrastructure that enables an AI agent to perform tasks within a controlled execution environment. In the context of software development agents, this infrastructure may include prompts, context preparation, tool definitions, execution mechanisms, workflow control, validation procedures, and error-handling logic.

A language model alone does not automatically provide reliable access to a repository, execute commands in a development environment, or manage the complete lifecycle of a software change. These capabilities require an application layer that connects the model to the appropriate tools and determines how the results are processed.

Several components can contribute to an agent harness:

1. **Context management:** Collecting and preparing task requirements, source code, comments, and other relevant information.
2. **Tool integration:** Providing the agent with defined operations for accessing repositories, files, knowledge sources, and execution environments.
3. **Workflow orchestration:** Coordinating actions and determining which operations may follow a particular result.
4. **Validation and feedback:** Examining outputs from testing and other verification processes.
5. **Error handling:** Supporting recovery, retries, and controlled termination when an operation cannot be completed successfully.

Harness engineering was investigated before the official project began. The subsequent development of the AI Developer Agent for GitLab addressed several of these concerns through context intake, clarification, custom orchestration, tool integration, and iterative execution.

The project also demonstrated different ways to implement the agent harness. The initial prototype used separate workflow stages and artifact handoffs, whereas later implementations explored a custom tool-calling loop and an SDK-based agent runtime.

### 2.1.3 Tool Calling and Function Calling

Tool calling enables an AI model to request the execution of operations provided by an application. A tool may retrieve information, modify files, execute a command, perform a validation task, or interact with an external service.

In function calling, the application defines available functions and their input parameters. The model can select an appropriate function and provide arguments, but the application is responsible for executing the requested operation and returning its result. The model can then use that result to produce another tool call or a final response [1].

A typical tool-calling interaction consists of the following steps:

1. The application provides the model with the task context and available tool definitions.
2. The model selects a tool and generates the required arguments.
3. The application executes the requested tool.
4. The tool returns its result to the application.
5. The result is provided to the model for further reasoning.
6. The process continues until the task reaches a defined completion condition or another control decision is required.

This interaction supports multi-step tasks because each tool result can influence the next action. However, tool calling alone does not define the complete workflow. Additional application logic may be required to enforce execution order, restrict available operations, validate results, and control retries.

In the AI Developer Agent for GitLab project, the custom orchestration implementation combined model-selected function calls with application-controlled decisions. For example, the model could generate code through a change-generation tool, while the orchestrator determined whether to apply the changes, run validation, proceed to review, or initiate debugging.

This combination allowed the agent to perform flexible tool selection while maintaining explicit control over the development process.

### 2.1.4 Workflow Orchestration and Iterative Execution

Workflow orchestration is the coordination of multiple operations to achieve a larger objective. In an AI-assisted software development system, orchestration determines how task intake, context preparation, code generation, modification, testing, review, and other activities are connected.

A fixed workflow follows a predetermined sequence of operations. This approach provides clear stage boundaries and makes it easier to separate responsibilities. However, software development tasks may require different actions depending on intermediate results. For example, a validation failure may require debugging and another implementation attempt rather than proceeding directly to review.

An iterative workflow addresses this requirement by allowing the next action to depend on the result of the previous operation. The workflow may continue, repeat an operation, select a recovery action, or terminate when a stopping condition is reached.

A simplified example is:

**Generate changes → Apply changes → Validate → Review**

If validation fails, the workflow may instead follow:

**Validate → Debug → Retry implementation or validation**

The orchestrator manages these transitions and can enforce a maximum retry limit. This limit helps prevent endless repetition when the agent cannot resolve a problem.

The project explored both fixed-stage orchestration and iterative execution. Prototype 1.0 separated activities into stages connected through artifacts. The custom tool-calling implementation coordinated several operations within an iterative loop. Prototype 2.0 subsequently incorporated the custom orchestrator into selected stages of a larger development workflow.

The later adoption of the GitHub Copilot SDK changed the implementation approach by transferring most of the previous custom agent implementation to an existing agent runtime.

### 2.1.5 Context Management and Retrieval-Augmented Generation

Context management involves collecting, organizing, and providing the information an agent requires to perform a task. For a software development agent, this information may include issue descriptions, comments, relevant source files, project conventions, documentation, and results from previous operations.

Incomplete or irrelevant context can make it difficult for an agent to understand the requested change or determine which parts of a repository should be examined. Context preparation is therefore an important part of the overall workflow.

Retrieval-Augmented Generation (RAG) is an approach that combines information retrieval with language-model generation. Instead of relying exclusively on information contained in the model's parameters, a RAG system retrieves relevant information from an external knowledge source and supplies it as context for generating a response.

A typical RAG workflow consists of three activities:

1. **Indexing:** Preparing documents or other knowledge sources for retrieval.
2. **Retrieval:** Searching the indexed information for content relevant to a query.
3. **Generation:** Providing retrieved information to a language model to support its response.

RAGFlow is a RAG-oriented platform designed to support document understanding and knowledge retrieval in LLM applications [2].

In the AI Developer Agent for GitLab project, RAGFlow MCP was integrated alongside GitLab MCP, Filesystem MCP, and custom tools. This integration provided the agent with access to a knowledge retrieval capability as part of its broader tool environment.

Context management also appeared in the initial intake and clarification stages. The intake stage collected the original issue information, while clarification enriched the task context with additional relevant information before subsequent development activities.

## 2.2 Related Technologies

### 2.2.1 GitLab and Continuous Integration and Continuous Delivery

GitLab is a platform that supports source code management and collaborative software development. Its CI/CD capabilities allow development workflows to be defined as pipelines containing jobs and stages. GitLab Runner executes CI/CD jobs according to the pipeline configuration, and job artifacts can transfer files between jobs [3], [4].

A CI/CD pipeline commonly includes activities such as building software, running tests, and preparing deliverables. These activities can be organized into stages with defined dependencies.

GitLab Runner was used in the initial implementations of the AI Developer Agent for GitLab. The first prototype used separate stages to process different parts of the agent workflow. Artifacts provided a mechanism for transferring issue context between the intake and clarification stages.

The later Prototype 2.0 integrated the workflow into a sequence of stages covering intake and harness selection, clarification, implementation, applying changes, testing, and opening a merge request.

The GitLab webhook served as the entry point for receiving relevant issue events. This allowed the application to respond to activity in GitLab and initiate the associated workflow.

These capabilities made GitLab and GitLab Runner suitable components for coordinating the agent's execution within an existing software development environment.

### 2.2.2 GitLab Webhooks and Issue Context

A webhook is a mechanism through which an application sends an HTTP request to another application when a specified event occurs. Webhooks allow systems to respond to events without continuously polling the source system for updates.

In a repository-based development workflow, relevant events may include issue creation, issue updates, or assignment of an issue to a user or agent, depending on the configured event subscriptions and filtering rules.

A webhook receiver processes incoming requests and extracts the information required to initiate a workflow. The receiver may then retrieve additional information from the source system if the event payload does not contain all the required context.

In the AI Developer Agent for GitLab project, webhook handling provided the entry point for relevant GitLab issue events. The intake process collected information such as the issue title, description, and comments and prepared the context for downstream processing.

The clarification stage subsequently loaded the prepared context and enriched it with relevant file information. This separation distinguished the initial collection of task information from the interpretation and expansion of the context required for implementation.

### 2.2.3 GitLab CI/CD Artifacts

GitLab job artifacts are files or directories produced by CI/CD jobs and made available for later use. They can contain build outputs, reports, logs, or intermediate data that subsequent jobs require [3].

Artifacts are particularly useful when a workflow is divided into multiple stages. A job can save its output as an artifact, and a later job can retrieve that artifact and continue processing the information.

In Prototype 1.0, artifacts were used to transfer issue context from the intake stage to the clarification stage. The intake stage prepared the issue information, and the clarification stage loaded that information before enriching it with relevant context.

This design provided an explicit handoff between stages and reduced the need for each stage to independently repeat the initial context-collection process.

The artifact-based approach was appropriate for the initial staged architecture. In contrast, the later tool-calling implementation performed multiple related operations within an orchestration loop, reducing the need to create a separate pipeline stage for every individual tool action.

### 2.2.4 Model Context Protocol

The Model Context Protocol (MCP) is an open protocol for connecting AI applications with external tools and context sources. It uses a client-server architecture in which an AI application communicates with MCP servers that expose defined capabilities. The protocol includes mechanisms for exposing tools, resources, and prompts, with communication structured around JSON-RPC [5].

MCP separates the interface for accessing external capabilities from the internal implementation of those capabilities. An MCP server may expose operations for interacting with a service, accessing files, or retrieving information. An MCP client can discover and invoke the capabilities provided by the server.

The protocol does not itself define how an AI application must reason about a task or coordinate an entire workflow. The application or agent runtime remains responsible for deciding how available capabilities are used.

This distinction is important because tool integration and workflow orchestration address different problems. MCP provides a standardized mechanism for exposing and accessing tools, while orchestration determines how those tools participate in the execution process.

The project initially explored exposing existing tools through a custom MCP server. It subsequently integrated external MCP servers for GitLab, Filesystem, and RAGFlow, together with a custom MCP server containing project-specific tools.

The custom tools included `discover_and_select_skills`, `run_validation`, and `execute_cmd`. These provided capabilities related to skill selection, validation, and command execution.

Successful integration of the external MCP servers allowed the agent to use the tools provided by these services within the development workflow.

### 2.2.5 GitLab MCP, Filesystem MCP, and RAGFlow MCP

The project used multiple MCP servers to provide access to different categories of functionality.

**GitLab MCP** provided access to GitLab-related tools. Such integration is relevant to an agent that needs to interact with issues and other repository-management operations during a development task.

**Filesystem MCP** provided access to filesystem-related operations. These capabilities are relevant to examining project files and working with repository content.

**RAGFlow MCP** provided access to knowledge retrieval capabilities. Retrieved information can help supply additional context that may not be present in the original issue description.

The team also developed a custom MCP server to expose project-specific capabilities. Its tools included skill discovery and selection, validation, and command execution.

These servers provided complementary capabilities rather than serving as interchangeable components. Their integration allowed the agent to access repository-related operations, filesystem resources, and knowledge retrieval through a common protocol.

The availability of these tools expanded the agent's capabilities, while the agent framework and its prompts determined how the tools were used during task execution.

### 2.2.6 GitHub Copilot SDK

The GitHub Copilot SDK provides a programmatic interface for integrating the Copilot agent runtime into applications and services. According to its official repository, the SDK exposes the runtime behind Copilot CLI and supports multiple programming languages. The SDK communicates with the Copilot CLI server through JSON-RPC [6].

The SDK supports an approach in which developers define agent behavior and integrate the runtime into their applications instead of implementing every part of the agent execution mechanism themselves.

This is relevant to AI developer agent systems because the application may need to coordinate prompts, repository context, external tools, and the surrounding development workflow while delegating much of the agent execution to an existing runtime.

In the AI Developer Agent for GitLab project, the Copilot SDK replaced most of the previous custom agent implementation. The updated approach used the SDK, prompts, and MCP servers to support the agent's execution and tool access.

The migration represented a change in the agent implementation architecture. The project retained its overall objective of processing GitLab development tasks, generating and testing code, and opening merge requests, while adopting a different framework for the agent's execution.

The SDK-based implementation was tested across the end-to-end workflow, including issue intake, MCP tool usage, code generation, testing, and merge request creation.

### 2.2.7 Related Work and Engineering Considerations

Existing technical documentation describes the individual mechanisms used in this project: GitLab documents pipeline execution and artifact handoffs; function-calling documentation explains how language models can request external operations; MCP defines a standardized protocol for tool and context integration; and the Copilot SDK provides a programmatic agent runtime [1], [3]–[6].

These technologies address complementary aspects of an AI developer agent. GitLab provides the development environment and pipeline infrastructure, function calling enables model-directed tool requests, MCP standardizes access to external capabilities, and an agent runtime coordinates model interaction and tool execution.

The project combined these mechanisms through successive implementations. The initial architecture emphasized separate stages and artifact-based handoffs. The custom tool-calling implementation introduced an iterative orchestration loop with validation and retry control. The MCP-based implementation extended access to external tools, while the final implementation adopted the GitHub Copilot SDK.

The resulting development process illustrates an engineering consideration in AI agent systems: the choice of tool-integration protocol, orchestration mechanism, and agent runtime affects how the overall workflow is implemented and maintained.

## References

[1] OpenAI, "Function Calling." OpenAI API Documentation. Available: https://developers.openai.com/api/docs/guides/function-calling

[2] RAGFlow, "Quickstart." RAGFlow Documentation. Available: https://ragflow.io/docs

[3] GitLab, "Job Artifacts." GitLab Documentation. Available: https://docs.gitlab.com/ci/jobs/job_artifacts/

[4] GitLab, "CI/CD Jobs." GitLab Documentation. Available: https://docs.gitlab.com/ci/jobs/

[5] Model Context Protocol, "Architecture Overview." Official MCP Documentation. Available: https://modelcontextprotocol.io/docs/learn/architecture

[6] GitHub, "GitHub Copilot SDK." GitHub Repository. Available: https://github.com/github/copilot-sdk
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

# CHAPTER 4
# RESULTS AND EVALUATION

## 4.1 Overview

The AI Developer Agent for GitLab evolved through several implementation approaches, including a stage-based workflow, a custom tool-calling orchestrator, an MCP-based architecture, and an implementation based on the GitHub Copilot SDK.

The evaluation focused on functional behavior, tool integration, workflow orchestration, and end-to-end execution. Testing covered the agent's ability to process development tasks, invoke tools, respond to execution results, handle validation failures, and continue or retry operations according to the workflow logic.

The results presented in this chapter are based on functional and integration testing. Quantitative performance measurements, such as execution time, code quality scores, and success rates across a measured dataset, were not established as part of the results described here.

## 4.2 Evaluation of the Custom Orchestration Mechanism

### 4.2.1 Tool Execution and Workflow Control

The custom orchestration implementation combined language-model-driven function calling with application-controlled workflow logic. This allowed the agent to generate code and select tools while the orchestrator coordinated execution and controlled subsequent actions.

Testing covered the main execution paths, including code generation, applying changes, validation, review, debugging, and retry handling. The orchestrator used the results of preceding operations to determine the next action.

### 4.2.2 Validation Failure and Retry Handling

The validation and retry mechanism was designed to handle unsuccessful execution attempts. When validation failed, the orchestrator could direct the agent to perform debugging activities and retry the relevant operations. When validation succeeded, the workflow could proceed to the review stage.

The orchestrator also enforced a retry limit. This prevented the workflow from repeating the same recovery process indefinitely and allowed control to move to another stage when the permitted retry limit was exceeded.

The identified execution scenarios were tested to examine whether the workflow followed the expected transitions. This testing covered both successful execution paths and failure-handling paths.

### 4.2.3 Evaluation Results

The functional testing demonstrated the behavior of the orchestration mechanism across the tested scenarios. The implementation provided a means of coordinating model-selected tool calls with application-controlled transitions, validation results, and retry limits.

The main outcome was the integration of iterative execution into the development workflow, allowing subsequent actions to depend on the results of preceding operations rather than relying exclusively on a fixed sequence.

## 4.3 Evaluation of MCP Integration

### 4.3.1 Integrated MCP Servers

The project integrated three external MCP servers: GitLab MCP, Filesystem MCP, and RAGFlow MCP. In addition, a custom MCP server exposed project-specific tools.

The GitLab and Filesystem integrations provided access to relevant development operations and filesystem resources, while RAGFlow supported knowledge retrieval. The custom MCP server exposed tools including `discover_and_select_skills`, `run_validation`, and `execute_cmd`.

### 4.3.2 Functional Integration Results

The integrated tools from all three external MCP servers were used successfully during testing. This demonstrated that the agent could access the integrated tool capabilities through the MCP-based architecture.

The custom MCP tools also supported project-specific activities, including skill discovery, validation, and command execution. Together, these integrations extended the resources available to the agent during software development tasks.

The results showed that MCP could be used as a common integration mechanism for connecting the agent to multiple tool providers and project-specific capabilities.

## 4.4 Evaluation of the GitHub Copilot SDK Implementation

### 4.4.1 Implementation Migration

The project replaced most of the previous custom agent implementation with the GitHub Copilot SDK. The updated approach relied on the SDK, prompts, and MCP servers to support agent execution and tool access.

This migration changed the implementation approach while retaining the project's broader objective of supporting AI-assisted software development within GitLab.

### 4.4.2 End-to-End Functional Testing

The SDK-based implementation was tested across the main development workflow. The tested capabilities included:

1. Receiving a development task originating from a GitLab issue.
2. Using available MCP tools during task processing.
3. Generating code changes.
4. Executing testing activities.
5. Completing the workflow through opening a merge request.

The system successfully performed these functions during testing. This demonstrated that the SDK-based implementation could support the principal workflow from task intake through implementation and testing to merge request creation.

### 4.4.3 Evaluation Results

The SDK-based implementation provided a working end-to-end workflow integrating GitLab issue handling, agent execution, MCP tool access, code generation, testing, and merge request creation.

Replacing most of the previous agent implementation also demonstrated an alternative approach to building the system around an existing agent SDK rather than maintaining the entire custom agent execution mechanism.

Further quantitative evaluation would be required to compare the SDK-based implementation with earlier prototypes in terms of execution time, resource consumption, code quality, and task completion reliability.

## 4.5 Summary of Results

The project achieved functional outcomes across the main areas of development. The custom orchestration implementation supported iterative tool execution and controlled retry behavior. The MCP-based architecture successfully integrated tools from GitLab MCP, Filesystem MCP, and RAGFlow MCP, together with project-specific custom tools. The Copilot SDK implementation subsequently supported the tested end-to-end workflow, from receiving a GitLab issue to opening a merge request.

The development process also demonstrated how the agent architecture could evolve from separate workflow stages to a custom orchestration mechanism, then to MCP-based integration and an SDK-based implementation.

These results provide a foundation for further evaluation of the agent's reliability, efficiency, and effectiveness on a broader range of software development tasks.
