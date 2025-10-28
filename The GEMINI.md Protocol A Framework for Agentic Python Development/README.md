

# **The GEMINI.md Protocol: A Framework for Agentic Python Development**

## **Part I: The Philosophy of Agent-Centric Development**

### **Section 1: Principles of Modern Agentic Systems**

The integration of Artificial Intelligence into software development has precipitated a paradigm shift, moving beyond simple code completion to a more collaborative and autonomous model. This new approach, known as agentic development, redefines the relationship between human engineers and their tools. Understanding the core principles of this paradigm is essential for harnessing its full potential.

#### **Defining the Agentic Paradigm**

An agentic system is not merely a passive code generator responding to single-shot commands, a practice sometimes referred to as "vibe coding".

1 Instead, it is an active, intelligent participant in the development lifecycle, characterized by two key traits: **autonomy** and **goal-directed behavior**.

2 An AI agent can operate independently to make decisions based on available data and its environment, working towards a specified objective without requiring constant, step-by-step human intervention.2 This autonomy allows it to analyze a problem, formulate a strategy, execute actions, and adapt its approach based on the outcomes it observes.

#### **The Core Agentic Loop**

The fundamental workflow of an agentic system is an iterative, cyclical process often described as the **agentic loop**: plan \-\> act \-\> observe \-\> refine. This loop enables the agent to engage in multi-step reasoning, breaking down complex, open-ended problems into a sequence of manageable, concrete actions.4

1. **Plan:** The agent first analyzes the user's request and the current state of the environment (the codebase). It then formulates a high-level plan or a sequence of discrete steps to achieve the goal. For complex or ambiguous tasks, a critical part of this phase is for the agent to seek clarification from the human developer or propose a detailed plan for approval before proceeding with implementation. This "plan-first" approach prevents wasted effort and ensures alignment with the developer's intent.6  
2. **Act:** The agent executes the steps outlined in its plan by interacting with its environment through a predefined set of **tools**. These tools are the agent's Application Programming Interface (API) to the real world. In the context of software development, these tools invariably include capabilities for file system manipulation (reading, writing, and listing files), code execution within a controlled environment (often a Docker container for security), and running automated tests.4  
3. **Observe:** After each action, the agent receives feedback from the environment. This observation step is critical for the agent to assess its progress and determine the next action. The "ground truth" it receives comes directly from the results of its tool calls: the content of a file it read, the standard output and error streams from code it executed, and, most importantly, the pass/fail status of the project's automated test suite.4  
4. **Refine:** Based on the feedback observed, the agent refines its plan. If an action was successful, it proceeds to the next step. If an action resulted in an error—such as a syntax error during code execution or a failing test—the agent uses this information to debug and correct its approach. A test failure is not a terminal error but a crucial feedback signal that guides the agent's iterative refinement process toward a correct and robust solution.4

This iterative loop fundamentally re-envisions the role of automated testing. In traditional development workflows, tests are primarily a verification step performed *after* a developer has completed a unit of work. In an agentic workflow, tests become an integral, real-time *feedback mechanism* that actively steers the coding process. The agent's primary strength lies in its capacity for rapid iteration based on concrete, unambiguous feedback.5 The most reliable form of feedback on code correctness is the binary pass/fail status of an automated test.4 Consequently, a project that lacks a comprehensive, fast, and reliable test suite is fundamentally incompatible with an effective agentic workflow; the agent is, in effect, operating without its most critical sensory input. This positions Test-Driven Development (TDD), where a failing test is written first to define the goal, as the native and most effective methodology for agentic systems.

#### **The "Big Three" of Agent Interaction**

The success of any interaction with an AI agent hinges on the mastery of three core pillars: **Context**, **Prompt**, and **Model**.10 While the choice of model is often a given, the engineer's control over context and prompt is paramount. The prompt is the immediate instruction given to the agent, but the context is the vast body of information that informs the agent's understanding of the task. Engineering high-quality, persistent, and relevant context is the key to unlocking precise and effective agent performance. It is for this purpose that a dedicated guidance file is not merely a convenience, but a necessity.

### **Section 2: The Role of the GEMINI.md File**

To effectively manage the context provided to an AI agent, a standardized, machine-readable document within each repository is required. This document, which this protocol designates as GEMINI.md, serves as the foundational text that guides and constrains the agent's behavior.

#### **A Constitution for the Codebase**

The GEMINI.md file acts as the "README for agents".11 It is a dedicated, predictable, and machine-readable document that provides persistent context and instructions. Its existence eliminates the need for developers to repeat critical information—such as setup commands, architectural patterns, and testing procedures—in every prompt, thereby ensuring consistency and reducing the cognitive load on the human engineer.6

This concept of a dedicated agent guidance file is an emerging industry standard, with several precedents validating its utility:

* **AGENTS.md**: An open format designed to provide a unified configuration for any AI agent, covering setup commands, code style, and testing workflows.6  
* **.junie/guidelines.md**: Used by JetBrains' Junie agent, this file allows teams to specify technology choices, coding conventions, and anti-patterns to guide code generation.13  
* **claude.md**: Employed by Anthropic's Claude Code, this file provides persistent context, with project-specific files located in the repository root and personal defaults in the user's home directory.8

#### **Distinction from README.md**

It is crucial to distinguish the purpose of GEMINI.md from that of the traditional README.md. A README.md file is written for humans; its primary purpose is to provide a project overview, installation instructions for contributors, and usage examples.11 A GEMINI.md file, conversely, is written for machines. It contains the detailed, technical, and often verbose context that an AI agent requires to operate effectively but which would unnecessarily clutter a human-centric document.11

#### **Living Documentation**

The GEMINI.md file must not be a static, write-once artifact. It must be treated as **living documentation**, version-controlled alongside the source code and continuously updated and refined as the project evolves. As developers observe the agent's behavior—both its successes and its failures—they should use these observations to improve the guidelines within the file, iteratively enhancing the agent's performance over time.6

The establishment of GEMINI.md represents a significant evolution in the role of documentation. It transforms documentation from a passive, descriptive artifact intended for human consumption into an **active, prescriptive configuration file for a machine process**. In this new paradigm, the GEMINI.md file is an integral part of the application's control plane. Traditional documentation describes the *state* of the code for human understanding. An agent, however, requires instructions on *how to change* the code and *what rules to follow* during that change—a prescriptive, not descriptive, need. By placing these instructions in a standardized file, they can be automatically ingested by the agent at the start of every session, making the documentation a direct input to the development process itself.12 Consequently, an error, ambiguity, or omission in GEMINI.md is not merely a documentation bug; it is a potential **production bug** in the making, as it will directly lead the agent to generate incorrect or non-compliant code. Its creation, maintenance, and review demand the same level of rigor and attention to detail as production source code.

## **Part II: Engineering an AI-Friendly Codebase**

An AI agent's ability to comprehend and modify a codebase is directly proportional to the clarity, consistency, and predictability of that code. Engineering a codebase to be "AI-friendly" is not about compromising on sound software principles; rather, it is about rigorously applying them. These practices not only enhance agent performance but also result in code that is more maintainable, testable, and understandable for human developers.17

### **Section 3: Code Modularity and the Single Responsibility Principle (SRP)**

#### **The Foundation of Agentic Compatibility**

The single most important prerequisite for effective agentic coding is a codebase composed of small, focused, and highly modular components.18 An agent cannot effectively reason about or safely modify large, monolithic files with entangled responsibilities and complex side effects.17 Modularity breaks down complexity, allowing the agent to focus its attention on a manageable, relevant subset of the code.

#### **Single Responsibility Principle (SRP)**

The Single Responsibility Principle (SRP), which states that every module, class, or function should have only one reason to change, is paramount for agentic compatibility.19 This separation of concerns is critical. For instance, a Journal class should be solely responsible for managing entries, while a separate PersistenceManager class should handle the distinct responsibility of saving the journal to a file or database.21 This decoupling allows an agent tasked with modifying persistence logic to operate only on the PersistenceManager, without needing the context of journal entry management, and vice versa.

The adoption of SRP is not merely an architectural preference; it is a direct strategy for optimizing the performance and economic efficiency of agentic systems. To perform any task, an agent must be given the context of the relevant code.10 The size of this context is a literal, quantifiable metric, often measured in tokens, which directly impacts the cost and speed of the LLM's operation. If business logic is scattered throughout a 5,000-line "god object" file, that entire file must be loaded into the agent's context window. This is expensive, slow, and introduces a significant amount of noise that can confuse the agent. In contrast, if the same logic is encapsulated within a 100-line user\_profile\_service.py file according to SRP, only that small, relevant file needs to be provided. This drastic reduction in context size leads to faster, cheaper, and more accurate agent operations. Therefore, refactoring a codebase towards SRP is a primary lever for improving the efficiency of an agentic development workflow.

#### **Characteristics of AI-Friendly Modules**

To be maximally effective for AI agents, modules should exhibit the following characteristics:

* **Small and Focused:** Files should be kept short and dedicated to a single, well-defined concern.17 This reduces the cognitive (and token) load required for an agent to understand a specific piece of functionality.  
* **Low Coupling, High Cohesion:** Modules should be as independent as possible, interacting with other modules through stable, well-defined interfaces (low coupling). The functions and classes within a single module should be highly related and work towards a common purpose (high cohesion).19  
* **Explicit over Implicit:** Code should be explicit and self-evident. Complex metaprogramming, magic methods, or other implicit behaviors can obscure the flow of logic and confuse an agent's static analysis capabilities.17  
* **DRY (Don't Repeat Yourself):** Duplicated code should be avoided by refactoring it into centralized, reusable functions or classes. This makes the codebase more maintainable and ensures that an agent can discover and apply the correct, canonical implementation of a piece of logic.19

### **Section 4: Naming, Typing, and Docstrings as a Machine-Readable API**

For an LLM, which lacks true understanding and relies on pattern matching and statistical inference, the surface-level characteristics of code are of utmost importance. Clear naming conventions, comprehensive type hints, and detailed docstrings are not merely matters of "style." Together, they form a machine-readable API that communicates the *intent* and *contract* of the code, which is essential for the agent to use and modify it correctly.17

#### **Naming Conventions**

Consistency in naming is a powerful signal to an AI agent. A project should adopt and rigorously enforce a standard style guide, with **PEP 8** being the default for the Python community.24 The **Google Python Style Guide** is another robust alternative.27 The specific choice of guide is less important than its consistent application. Names should be clear, descriptive, and unambiguous, favoring verbosity over cryptic abbreviations. A function named handle\_user\_input is infinitely more useful to an agent than one named hndl(r).17

#### **Type Hinting**

The adoption of static typing via type hints is one of the most impactful practices for creating an AI-friendly codebase. Type hints serve as a formal contract, explicitly documenting the expected data types of function arguments, variables, and return values.17 This removes guesswork for the agent and allows static analysis tools like Mypy to catch a class of errors before runtime. It is crucial to use specific types from the typing module (e.g., List\[str\], Dict\[str, int\], Optional\[User\]) wherever possible. The overuse of typing.Any should be strictly avoided, as it effectively negates the benefits of type safety and forces the agent back into a state of ambiguity.31

#### **Docstrings**

Docstrings are a critical source of high-level context that an agent cannot infer from the code alone. They should explain the "why" behind a piece of code—its business purpose and rationale—not just the "what" that the code itself describes.17 To be maximally useful for automated tools and agents, docstrings should adhere to a standardized, parsable format such as **Google-style** or **NumPy-style**.34 A well-structured docstring provides a concise summary, a detailed description of each argument (including its name, type, and meaning), a clear explanation of the return value, and a list of any exceptions that the function may raise.35

#### **Critical Style Guide Rules for AI Readability**

While comprehensive style guides are valuable, a subset of rules has a disproportionately high impact on an AI agent's ability to comprehend and generate code. The following table highlights these critical guidelines, reframing them from subjective preferences to functional necessities for agentic development.

| Rule Category | Guideline | Rationale for AI Agents |
| :---- | :---- | :---- |
| Naming | Use descriptive, explicit names (e.g., user\_list not ul). | Reduces ambiguity and the need for the agent to infer meaning from context, leading to more accurate code generation and modification. |
| Naming | Use lower\_case\_with\_underscores for functions and variables. | Provides a consistent, predictable pattern that the agent can learn to both follow and generate, ensuring stylistic consistency. |
| Naming | Use CamelCase for classes. | Clearly distinguishes types (classes) from instances or functions in the agent's parsing of the code, which is crucial for correct object instantiation and type checking. |
| Typing | Provide type hints for all function signatures and return values. | Acts as a formal contract, telling the agent the exact input and output data types required, preventing a large class of type-related errors. |
| Typing | Avoid typing.Any wherever a more specific type is possible. | Any removes the safety net of static analysis, forcing the agent to guess. Explicit types lead to more deterministic and reliable behavior. |
| Docstrings | Use a standard, machine-parsable format (e.g., Google-style). | A structured format allows the agent to reliably extract metadata about parameters, return types, and the function's purpose, which is critical for correct usage. |
| Docstrings | Explain the "why" (business logic), not just the "what" (code logic). | Provides crucial domain-specific context that the agent cannot infer from the code's syntax alone, enabling it to make more intelligent decisions. |
| Layout | Maintain consistent 4-space indentation. | Python's syntax demands correct indentation. Strict consistency prevents parsing errors by the agent's internal tools and ensures generated code is valid. |
| Layout | Adhere to a maximum line length (e.g., 88 characters for Black). | Prevents the agent from generating overly complex, nested, or hard-to-read single lines of code, promoting clarity and maintainability. |

### **Section 5: Predictable Patterns, Error Handling, and Logging**

Consistency in high-level architectural patterns and low-level implementation details creates a predictable environment in which an AI agent can thrive. This includes standardizing approaches to application logic, error handling, and logging.

#### **Consistent Architectural Patterns**

AI agents excel at recognizing and working with established patterns.17 Whether a project uses class-based services, a functional programming style, or a specific design pattern like the repository pattern, applying it consistently across the codebase is crucial. This consistency allows the agent to infer the project's architectural intentions and generate new code that conforms to the existing structure, reducing the need for manual refactoring.1

#### **Robust Error Handling**

A robust error handling strategy is essential for building resilient applications and for providing clear feedback to an AI agent. Best practices include:

* **Catch Specific Exceptions:** Avoid overly broad except Exception: clauses, as they can mask bugs and hide the true source of an error. Instead, catch specific, anticipated exceptions (e.g., KeyError, FileNotFoundError) to handle failures gracefully and provide precise feedback.37  
* **Use Focused try Blocks:** The code within a try block should be minimal and focused on a single logical operation. This helps to pinpoint the exact line of code that caused an error, which is critical information for an agent's debugging process.37  
* **Provide Informative Error Messages:** Error messages should be detailed and provide context. This information can be captured by the agent during the observe phase of its loop, allowing it to understand why its previous action failed and how to correct it.39

#### **Structured Logging**

For an automated system, logs are a primary channel of communication. To make them useful for an agent, they must be machine-readable.

* **Adopt Structured Logging:** Instead of unstructured, plain-text log messages, use a structured format like JSON.40 Libraries such as structlog are specifically designed for this purpose, producing logs as consistent key-value pairs that are easy to parse.43  
* **Include Rich Context:** A structured log entry should contain not just a message, but also a wealth of contextual data, such as a timestamp, the log level (INFO, ERROR, etc.), an event name, and any relevant identifiers like a user\_id or request\_id.42

Structured logs and specific exceptions are not just for human operators; they are the **primary machine-readable feedback channel for the agent's observe step**. The quality of this feedback directly determines the effectiveness of the agentic loop. Consider an agent attempting to process user data. In a poorly designed system, a KeyError might be caught by a generic except Exception: block that simply logs "An error occurred." The agent observes this useless text and has no information about what failed or why, making it impossible to refine its approach.

In a well-engineered system, the same KeyError would be caught specifically, and a structured JSON log would be emitted: {"level": "error", "event": "user\_data\_lookup\_failed", "missing\_key": "email", "user\_id": 123}. The agent can parse this JSON. It now knows the exact nature of the error, the context in which it occurred, and the specific cause. This rich, structured feedback enables the agent to make a precise correction in its refine step, such as adding a check for the 'email' key before attempting to access it. This transforms a failure into a productive learning step, making the entire agentic cycle dramatically more effective.

## **Part III: Project Architecture and Environment**

A well-defined project structure and a reproducible environment are foundational to successful agentic development. They create a stable and predictable canvas upon which the agent can work, minimizing ambiguity and ensuring that code behaves consistently across all stages of the development lifecycle.

### **Section 6: Standardized Project Structure**

A consistent and logical project structure is essential for both human developers and AI agents to navigate a codebase efficiently and understand its organization.18 Adopting a standardized layout reduces cognitive overhead and eliminates common sources of error, particularly with Python's import system.

#### **Recommended Layout**

The following project structure is recommended as a modern standard that promotes clarity, scalability, and compatibility with contemporary Python tooling.

project\_root/  
├──.venv/                  \# Virtual environment (should be in.gitignore)  
├── src/  
│   └── project\_name/       \# Main application package  
│       ├── \_\_init\_\_.py  
│       ├── core/           \# Core business logic, models, services  
│       └── features/       \# Feature-specific modules  
├── tests/                  \# All automated tests  
│   ├── \_\_init\_\_.py  
│   ├── test\_feature\_a.py  
│   └── test\_feature\_b.py  
├── docs/                   \# Project documentation  
├── scripts/                \# Utility and automation scripts  
├──.gitignore  
├── GEMINI.md               \# Guidelines for AI agents  
├── pyproject.toml          \# Project metadata and dependencies (PEP 518\)  
└── README.md               \# Project overview for humans

#### **Rationale for Structure**

* **src/ Layout:** Placing the main application package inside a src directory is a crucial best practice. It prevents a common class of import path issues that can arise when the project root is on the PYTHONPATH, ensuring that the installed package is always used, rather than the local files. This makes the development environment more closely mirror the production environment.46  
* **Separate tests/ Directory:** Isolating test code from application code in a top-level tests/ directory is a standard convention that improves organization. It makes it clear what code is part of the application and what code is for verification, and simplifies the configuration of test runners like pytest.18  
* **pyproject.toml:** The adoption of pyproject.toml as the central configuration file is the modern standard for Python projects. It consolidates project metadata, build system requirements, and application dependencies in one place, and is used by leading tools like Poetry and modern versions of pip.47

### **Section 7: Ensuring Reproducibility: Dependency and Environment Management**

For an agentic workflow to be reliable, the development, testing, and deployment environments must be identical and reproducible. An agent cannot effectively debug code if its local environment differs from the continuous integration (CI) environment where tests are formally run. This consistency is the bedrock of reliable automation.48

#### **Virtual Environments**

The use of virtual environments is a non-negotiable best practice for all Python projects. A virtual environment creates an isolated space for a project's dependencies, preventing conflicts with other projects or the system-wide Python installation.18 Each project must have its own dedicated virtual environment.

#### **Dependency Management Tools**

To ensure that every developer and every automated process uses the exact same versions of all dependencies, a modern dependency management tool that generates a **lock file** is essential. A lock file records the precise versions of not only the direct dependencies specified by the developer but also all of their transitive (indirect) dependencies.

* **Poetry:** This tool is strongly recommended for modern Python projects. It provides a robust dependency resolver, uses the standard pyproject.toml file for configuration, and generates a poetry.lock file for perfect reproducibility. Furthermore, it offers a complete project management solution, handling virtual environment creation, dependency installation, and package building/publishing in a single, cohesive interface.52  
* **Pipenv:** Pipenv is another excellent choice that combines dependency management and virtual environment creation. It uses a Pipfile to declare dependencies and a Pipfile.lock to ensure reproducible builds.47  
* **pip with requirements.txt:** This is a more traditional approach. While functional, it lacks the sophisticated dependency resolution of tools like Poetry. If this method is used, it is imperative that the requirements.txt file contains fully pinned versions of all dependencies, typically generated via a command like pip freeze or managed with a tool like pip-tools. This approach is suitable for simpler projects but is less robust for complex applications.47

The choice of dependency manager has significant downstream effects on the reliability of an agentic workflow. A comparative analysis makes a compelling case for adopting a modern tool.

#### **Dependency Management Tool Comparison**

| Feature | Poetry | Pipenv | pip \+ requirements.txt |
| :---- | :---- | :---- | :---- |
| **Configuration File** | pyproject.toml (PEP 518 Standard) | Pipfile (Custom TOML Format) | requirements.txt (Plain Text) |
| **Lock File** | poetry.lock | Pipfile.lock | Not native (requires pip freeze or external tools) |
| **Dependency Resolution** | Advanced, fast, deterministic solver | Basic solver, can be slow and non-deterministic | None (installs top-down, can lead to conflicts) |
| **Virtual Env Management** | Built-in and automatic | Built-in and automatic | Manual (requires separate use of venv) |
| **Packaging/Publishing** | Fully integrated (poetry build, poetry publish) | Not supported | Requires separate tools (e.g., setuptools, twine) |
| **Recommendation** | **Strongly Recommended** | Acceptable | Legacy / Simple Projects Only |

This comparison clearly illustrates the advantages of a modern, integrated tool like Poetry. Its superior dependency resolution prevents conflicts that can derail an automated process, and its native handling of lock files and virtual environments ensures the consistency that agentic workflows demand. Standardizing on such a tool is a key architectural decision for improving the stability and reliability of AI-assisted development.

## **Part IV: The GEMINI.md Specification**

This section provides the prescriptive core of the protocol: a detailed, section-by-section template for the GEMINI.md file. This template should be placed at the root of every Python repository to serve as the primary guidance document for all AI coding agents.

### **GEMINI.md Template**

# **AI Agent Guidelines for project\_name**

This document provides essential context and instructions for AI coding agents operating on this repository. Adhere strictly to these guidelines to ensure code quality, consistency, and correctness. This file is the single source of truth for automated development tasks.

---

### **1\. Project Overview & Architecture**

* **Purpose:** This project is a that serves \[e.g., user profile data and manages authentication\].  
* **Core Technologies:** Python 3.11, FastAPI, Pydantic, SQLAlchemy, PostgreSQL, Poetry.  
* **Architectural Principles:** The application follows a \[e.g., clean architecture with distinct layers for API, services, and data access\]. All business logic must reside in the service layer.

---

### **2\. Setup & Execution Commands**

Use these precise, file-scoped commands for all development tasks. Project-wide commands should be used sparingly and only when explicitly required.

* **Install Dependencies:** poetry install  
* **Activate Virtual Environment:** poetry shell  
* **Format a File:** poetry run black path/to/file.py  
* **Lint a File:** poetry run ruff check path/to/file.py \--fix  
* **Type-Check a File:** poetry run mypy path/to/file.py  
* **Run a Single Test File:** poetry run pytest path/to/test\_file.py  
* **Run All Tests (Use Sparingly):** poetry run pytest

---

### **3\. Coding Conventions & Style Guide (Dos and Don'ts)**

#### **Do**

* Adhere strictly to the PEP 8 style guide, enforced by black and ruff.  
* Use pydantic for all data validation models (both for API requests/responses and internal data structures).  
* Use the httpx library for all external HTTP calls, wrapped in a client class found in src/project\_name/clients/.  
* Prefer simple, stateless functions with clear inputs and outputs over classes with complex internal state.  
* Ensure all new business logic is accompanied by unit tests with a minimum of 90% line coverage.  
* Write comprehensive docstrings in Google-style format for all public modules, classes, and functions.

#### **Don't**

* Do not add new dependencies to pyproject.toml without human approval.  
* Do not hard-code credentials, API keys, or other secrets. Use environment variables accessed via the src/project\_name/config.py module.  
* Do not implement business logic directly within API endpoint functions (controllers). Delegate to a service-layer function.  
* Do not use broad except Exception: clauses. Catch specific exceptions only.

---

### **4\. Project Structure Navigation**

This index points to key locations in the codebase.

* **API Routes:** Defined in modules within src/project\_name/api/routes/.  
* **Core Business Models (Pydantic):** Located in src/project\_name/core/models.py.  
* **Service Layer (Business Logic):** Implemented in modules within src/project\_name/services/.  
* **Data Access Layer (Repositories):** Found in src/project\_name/data/repositories/.  
* **Database Schema (SQLAlchemy):** Defined in src/project\_name/data/models.py.  
* **Configuration:** Managed in src/project\_name/config.py.

---

### **5\. API & Data Layer Guidance**

* All database interactions must be performed through repository classes defined in the data access layer.  
* Do not write raw SQL queries. Use the SQLAlchemy 2.0 ORM syntax exclusively.  
* The client for the external "Payments API" is implemented in src/project\_name/clients/payments.py. Refer to docs/api/payments.md for detailed endpoint specifications.

---

### **6\. Testing Philosophy & Examples**

* This project follows a **Test-Driven Development (TDD)** methodology. Before implementing a feature, write a failing test that defines the desired behavior.  
* **Unit Tests:** For examples of well-structured unit tests using pytest, refer to tests/unit/test\_user\_service.py. Use pytest.fixture for all test setup and teardown.  
* **Integration Tests:** For examples of API integration tests, copy the pattern found in tests/integration/test\_auth\_endpoints.py.  
* **Mocking:** Use the standard library's unittest.mock for all mocking and patching.

---

### **7\. Safety, Permissions & Escape Hatches**

#### **Permissions**

* **Allowed without prompt:** Reading and writing files within the src/ and tests/ directories. Running all format, lint, type-check, and test commands.  
* **Requires human approval:** Installing new dependencies (poetry add), running git push, deleting files, or modifying configuration files (pyproject.toml, GEMINI.md).

#### **When Stuck**

* If you are unsure how to proceed, are unable to fix a failing test, or receive an ambiguous instruction, **STOP**. Do not guess. Propose a detailed, step-by-step plan in a file named plan.md and await human approval.

---

### **8\. PR & Commit Guidelines**

* **Commit Message Format:** Adhere to the Conventional Commits specification. Example: feat(api): add user profile update endpoint.  
* **Pull Request (PR) Checklist:**  
  1. All linting, type-checking, and automated tests must pass in the CI pipeline.  
  2. The PR description must provide a clear summary of the changes and link to the relevant issue tracker ticket.  
  3. The diff should be small, focused, and represent a single logical change.

### **Section-by-Section Rationale**

* **1\. Project Overview & Architecture:** This section provides the agent with crucial domain context. By understanding the project's purpose and high-level architecture, the agent can make more intelligent decisions that align with the project's goals, rather than just performing syntactic manipulations.11  
* **2\. Setup & Execution Commands:** This is the agent's toolbox. Providing precise, **file-scoped** commands for linting, testing, and formatting is critical. It enables the agent to operate in a tight, efficient feedback loop, getting immediate results from its actions without the overhead of running slow, project-wide builds.6  
* **3\. Coding Conventions & Style Guide:** This is the "rules of the road." A clear list of "Dos and Don'ts" provides explicit, unambiguous constraints on the agent's behavior. This is where a team encodes its specific architectural preferences and anti-patterns, preventing the agent from introducing technical debt or violating established standards.6  
* **4\. Project Structure Navigation:** This section acts as a "tiny index" or map of the codebase. It drastically reduces the agent's need for exploratory file system commands (ls, find), allowing it to immediately locate the relevant code for a given task. This improves both the speed and accuracy of its operations.6  
* **5\. API & Data Layer Guidance:** This provides specific instructions for interacting with critical, stateful systems like databases and external services. It ensures the agent uses the correct abstractions (e.g., repositories, API clients) and avoids insecure practices like writing raw SQL or embedding credentials.6  
* **6\. Testing Philosophy & Examples:** This section ensures that the tests generated by the agent conform to the project's standards. By pointing to canonical examples, it instructs the agent on the preferred testing framework, structure, and patterns, thereby maintaining the quality and consistency of the test suite.6  
* **7\. Safety, Permissions & Escape Hatches:** This is arguably the most critical section for safe operation. It establishes clear guardrails, defining which actions are autonomous and which require human confirmation. The "When Stuck" instruction provides a robust failure mode, preventing the agent from taking destructive actions or getting stuck in unproductive, costly loops when it encounters a situation it cannot resolve.6  
* **8\. PR & Commit Guidelines:** This section automates adherence to the repository's contribution and version control standards. It ensures that the agent's output is not just functionally correct but also correctly formatted for integration into the team's workflow, including commit messages and pull request documentation.6

## **Part V: Recommendations and Advanced Strategies**

The GEMINI.md protocol provides a robust foundation for agentic development. However, its successful implementation requires a shift in team workflows and an understanding of more advanced agentic patterns for tackling complex problems.

### **Section 9: Implementing the Agentic Workflow**

Operationalizing this protocol involves more than simply creating the GEMINI.md file. It requires adopting new practices and a new mindset regarding the role of AI in the development process.

* **Iterative Refinement:** The GEMINI.md file should be subject to a continuous feedback loop. Teams must regularly review agent interactions, logs, and generated code to identify common mistakes, misunderstandings, or areas where the agent struggles. These observations should be used to refine the guidelines, iteratively improving the agent's performance and alignment with the team's expectations.6  
* **Human-in-the-Loop and Code Review:** Agent-generated code is not infallible and must be subjected to the same rigorous code review process as human-written code. The role of the senior developer evolves from that of a primary implementer to that of an architect, reviewer, and guide for the AI agent. Their expertise is applied to verifying the agent's approach, ensuring architectural integrity, and providing high-level direction.1  
* **Pair Programming Mindset:** The most effective mental model for interacting with an agent is to treat it as a junior pair programmer. The developer provides clear, high-level instructions, breaks down complex problems, reviews the agent's work in small increments, and offers specific, corrective feedback when the agent deviates from the desired path. This collaborative, back-and-forth workflow is highly productive and leverages the strengths of both the human (strategic thinking, domain knowledge) and the AI (speed, iteration, boilerplate generation).1

### **Section 10: Advanced Agentic Patterns**

For highly complex tasks that span multiple files or require parallel lines of investigation, more sophisticated agentic patterns can be employed.

* **Orchestrator-Workers and Subagents:** This pattern involves a high-level "orchestrator" agent that is responsible for breaking a complex problem down into smaller, independent sub-tasks. It then delegates each sub-task to a specialized "worker" subagent. Each subagent operates in its own context window, focusing solely on its assigned task. This is particularly effective for making coordinated changes across multiple files or for parallelizing research and implementation efforts.5 The use of subagents helps manage context window limitations by ensuring only relevant information is passed back to the orchestrator.56  
* **Parallelization Workflows:** Agents can be deployed in parallel to accelerate problem-solving or improve the robustness of solutions.5 Two common variations are:  
  * **Sectioning:** A task is broken into independent, non-overlapping sub-tasks that can be executed simultaneously by multiple agents. For example, implementing three different API endpoints could be assigned to three agents working in parallel.  
  * **Voting:** The same task is given to multiple agents, potentially with slightly different prompts or configurations. The diverse outputs can then be compared, evaluated, and combined to produce a more robust final solution. This is useful for tasks like reviewing code for security vulnerabilities, where multiple perspectives can uncover more issues.5  
* **The Future is Composable:** The agentic ecosystem is moving towards a model of composability, where agent capabilities can be packaged and shared as plugins, hooks, and marketplaces.57 Engineering leaders can leverage this trend to create and distribute standardized plugins across their organization. These plugins can enforce specific code review workflows, provide secure clients for internal tools, or encapsulate best practices, ensuring that all teams are working with an approved and consistent set of agentic capabilities.57

### **Conclusion**

The transition to agentic software development represents a profound opportunity to enhance developer productivity, improve code quality, and accelerate innovation. However, this transition cannot be managed on an ad-hoc basis. AI agents, for all their power, are not magic; they are tools that require clear, consistent, and machine-readable instructions to operate effectively and safely.

The GEMINI.md protocol outlined in this report provides a comprehensive framework for achieving this. By establishing a standardized guidance file, engineering an AI-friendly codebase built on principles of modularity and clarity, and adopting reproducible project environments, development teams can create the necessary conditions for AI agents to become reliable and effective partners in the software lifecycle.

The core recommendations are:

1. **Adopt a Standardized Guidance File:** Every Python repository must include a GEMINI.md file at its root, following the specification provided. This file should be treated as a critical, version-controlled configuration artifact.  
2. **Prioritize AI-Friendly Code Practices:** Emphasize modularity (SRP), explicit naming, comprehensive type hinting, and structured docstrings. These are not merely stylistic choices but functional requirements for effective agentic interaction.  
3. **Mandate Reproducible Environments:** Standardize on a modern dependency management tool like Poetry that enforces reproducibility through lock files. Every project must use isolated virtual environments.  
4. **Integrate Testing as a Feedback Loop:** A robust, fast, and comprehensive automated test suite is the primary feedback mechanism for an agent. A TDD-like workflow should be considered the default methodology.  
5. **Embrace the Human-in-the-Loop Model:** The developer's role shifts to that of an architect and reviewer. All agent-generated code must undergo rigorous human review before being integrated.

By implementing this protocol, organizations can move beyond experimentation and begin to systematically integrate AI agents into their core development workflows, unlocking a new level of efficiency and capability while maintaining the high standards of engineering excellence required to build robust and maintainable software.

#### **Works cited**

1. Best Practices I Learned for AI Assisted Coding | by Claire Longo \- Medium, accessed on October 28, 2025, [https://statistician-in-stilettos.medium.com/best-practices-i-learned-for-ai-assisted-coding-70ff7359d403](https://statistician-in-stilettos.medium.com/best-practices-i-learned-for-ai-assisted-coding-70ff7359d403)  
2. Building Agentic AI Systems in Python A Beginner's Guide \- Codewave, accessed on October 28, 2025, [https://codewave.com/insights/agentic-ai-systems-python-guide/](https://codewave.com/insights/agentic-ai-systems-python-guide/)  
3. The Agentic AI Handbook: A Beginner's Guide to Autonomous Intelligent Agents, accessed on October 28, 2025, [https://www.freecodecamp.org/news/the-agentic-ai-handbook/](https://www.freecodecamp.org/news/the-agentic-ai-handbook/)  
4. Let's build our own Agentic Loop, running in our own terminal, from scratch (Baby Manus) : r/AI\_Agents \- Reddit, accessed on October 28, 2025, [https://www.reddit.com/r/AI\_Agents/comments/1js1xjz/lets\_build\_our\_own\_agentic\_loop\_running\_in\_our/](https://www.reddit.com/r/AI_Agents/comments/1js1xjz/lets_build_our_own_agentic_loop_running_in_our/)  
5. Building Effective AI Agents \- Anthropic, accessed on October 28, 2025, [https://www.anthropic.com/research/building-effective-agents](https://www.anthropic.com/research/building-effective-agents)  
6. Improve your AI code output with AGENTS.md (+ my best tips), accessed on October 28, 2025, [https://www.builder.io/blog/agents-md](https://www.builder.io/blog/agents-md)  
7. Best practices for using AI coding Agents \- Augment Code, accessed on October 28, 2025, [https://www.augmentcode.com/blog/best-practices-for-using-ai-coding-agents](https://www.augmentcode.com/blog/best-practices-for-using-ai-coding-agents)  
8. Essential Claude Code best practices from Anthropic's Cal Rueb : r ..., accessed on October 28, 2025, [https://www.reddit.com/r/ClaudeCode/comments/1mebqnw/essential\_claude\_code\_best\_practices\_from/](https://www.reddit.com/r/ClaudeCode/comments/1mebqnw/essential_claude_code_best_practices_from/)  
9. Guide to Agentic AI – Build a Python Coding Agent with Gemini \- YouTube, accessed on October 28, 2025, [https://www.youtube.com/watch?v=YtHdaXuOAks](https://www.youtube.com/watch?v=YtHdaXuOAks)  
10. Principled AI Coding \- Agentic Engineer, accessed on October 28, 2025, [https://agenticengineer.com/principled-ai-coding](https://agenticengineer.com/principled-ai-coding)  
11. AGENTS.md, accessed on October 28, 2025, [https://agents.md/](https://agents.md/)  
12. AGENTS.md \- Builder.io, accessed on October 28, 2025, [https://www.builder.io/c/docs/agents-md](https://www.builder.io/c/docs/agents-md)  
13. Coding Guidelines for Your AI Agents | The IntelliJ IDEA Blog, accessed on October 28, 2025, [https://blog.jetbrains.com/idea/2025/05/coding-guidelines-for-your-ai-agents/](https://blog.jetbrains.com/idea/2025/05/coding-guidelines-for-your-ai-agents/)  
14. Junie Playbook \- JetBrains Guide, accessed on October 28, 2025, [https://www.jetbrains.com/guide/ai/article/junie/](https://www.jetbrains.com/guide/ai/article/junie/)  
15. Guidelines | Junie Documentation \- JetBrains, accessed on October 28, 2025, [https://www.jetbrains.com/help/junie/customize-guidelines.html](https://www.jetbrains.com/help/junie/customize-guidelines.html)  
16. How Anthropic teams use Claude Code, accessed on October 28, 2025, [https://www-cdn.anthropic.com/58284b19e702b49db9302d5b6f135ad8871e7658.pdf](https://www-cdn.anthropic.com/58284b19e702b49db9302d5b6f135ad8871e7658.pdf)  
17. Building an AI-Friendly Codebase \- Vincent Schmalbach, accessed on October 28, 2025, [https://www.vincentschmalbach.com/building-an-ai-friendly-codebase/](https://www.vincentschmalbach.com/building-an-ai-friendly-codebase/)  
18. How to Structure Python Projects \- Dagster, accessed on October 28, 2025, [https://dagster.io/blog/python-project-best-practices](https://dagster.io/blog/python-project-best-practices)  
19. Best Practices for Modular Code Design \- PixelFreeStudio Blog, accessed on October 28, 2025, [https://blog.pixelfreestudio.com/best-practices-for-modular-code-design/](https://blog.pixelfreestudio.com/best-practices-for-modular-code-design/)  
20. Optimizing Your Codebase for AI Coding Assistants | by Eden Ella | Bits and Pieces, accessed on October 28, 2025, [https://blog.bitsrc.io/how-to-design-a-codebase-optimized-for-ai-coding-assistants-e760569ae7b3](https://blog.bitsrc.io/how-to-design-a-codebase-optimized-for-ai-coding-assistants-e760569ae7b3)  
21. Python Single Responsibility Design Pattern (Code Example) \- DEV Community, accessed on October 28, 2025, [https://dev.to/cleancodestudio/python-single-responsibility-design-pattern-code-example-1e1k](https://dev.to/cleancodestudio/python-single-responsibility-design-pattern-code-example-1e1k)  
22. LLMs seem to be flawed at the core for real programming \- Help Needed \- ChatGPT, accessed on October 28, 2025, [https://community.openai.com/t/llms-seem-to-be-flawed-at-the-core-for-real-programming-help-needed/1128555](https://community.openai.com/t/llms-seem-to-be-flawed-at-the-core-for-real-programming-help-needed/1128555)  
23. General guidelines and best practices for AI code generation \- GitHub Gist, accessed on October 28, 2025, [https://gist.github.com/juanpabloaj/d95233b74203d8a7e586723f14d3fb0e](https://gist.github.com/juanpabloaj/d95233b74203d8a7e586723f14d3fb0e)  
24. How to Write Beautiful Python Code With PEP 8 – Real Python, accessed on October 28, 2025, [https://realpython.com/python-pep8/](https://realpython.com/python-pep8/)  
25. PEP 8 : Coding Style guide in Python \- GeeksforGeeks, accessed on October 28, 2025, [https://www.geeksforgeeks.org/python/pep-8-coding-style-guide-python/](https://www.geeksforgeeks.org/python/pep-8-coding-style-guide-python/)  
26. PEP-8: Python Naming Conventions & Code Standards \- DataCamp, accessed on October 28, 2025, [https://www.datacamp.com/tutorial/pep8-tutorial-python-code](https://www.datacamp.com/tutorial/pep8-tutorial-python-code)  
27. Python style guidelines \[go/cros-pystyle\] \- The Chromium Projects, accessed on October 28, 2025, [https://www.chromium.org/chromium-os/developer-library/reference/style-guides/python/](https://www.chromium.org/chromium-os/developer-library/reference/style-guides/python/)  
28. Python Style Guide \- Google Code, accessed on October 28, 2025, [https://code.google.com/archive/p/soc/wikis/PythonStyleGuide.wiki](https://code.google.com/archive/p/soc/wikis/PythonStyleGuide.wiki)  
29. Google Python Style Guide, accessed on October 28, 2025, [https://android.googlesource.com/platform/external/google-styleguide/+/refs/tags/android-s-beta-2/pyguide.md](https://android.googlesource.com/platform/external/google-styleguide/+/refs/tags/android-s-beta-2/pyguide.md)  
30. Google Python Style Guide, accessed on October 28, 2025, [https://google.github.io/styleguide/pyguide.html](https://google.github.io/styleguide/pyguide.html)  
31. A Comprehensive Guide to Python Type Hints \- Codefinity, accessed on October 28, 2025, [https://codefinity.com/blog/A-Comprehensive-Guide-to-Python-Type-Hints](https://codefinity.com/blog/A-Comprehensive-Guide-to-Python-Type-Hints)  
32. Type Hints. Enhancing Your Codes with Clarity and… | by Data604 \- Medium, accessed on October 28, 2025, [https://medium.com/@Mustafa77/type-hints-6992f4297221](https://medium.com/@Mustafa77/type-hints-6992f4297221)  
33. Coding Standards for AI Agents \- Medium, accessed on October 28, 2025, [https://medium.com/@christianforce/coding-standards-for-ai-agents-cb5c80696f72](https://medium.com/@christianforce/coding-standards-for-ai-agents-cb5c80696f72)  
34. Best Practices for Learning Automated Docstring Generation \- Zencoder, accessed on October 28, 2025, [https://zencoder.ai/blog/learn-automated-docstring-techniques](https://zencoder.ai/blog/learn-automated-docstring-techniques)  
35. How to Write Docstrings in Python – Real Python, accessed on October 28, 2025, [https://realpython.com/how-to-write-docstrings-in-python/](https://realpython.com/how-to-write-docstrings-in-python/)  
36. Python Docstrings for Machine Learning Models \- Ready Tensor, accessed on October 28, 2025, [https://app.readytensor.ai/publications/python-docstrings-for-machine-learning-models-DM3Ao23CIocT](https://app.readytensor.ai/publications/python-docstrings-for-machine-learning-models-DM3Ao23CIocT)  
37. 6 Best practices for Python exception handling \- Qodo, accessed on October 28, 2025, [https://www.qodo.ai/blog/6-best-practices-for-python-exception-handling/](https://www.qodo.ai/blog/6-best-practices-for-python-exception-handling/)  
38. Python Error Handling: Try, Except, Finally & With Guide | Inventive HQ Blog, accessed on October 28, 2025, [https://inventivehq.com/blog/error-handling-in-python-try-except-with-and-finally-explained](https://inventivehq.com/blog/error-handling-in-python-try-except-with-and-finally-explained)  
39. Effective Error Handling in Python Applications Strategies and Best Practices \- Pitangent, accessed on October 28, 2025, [https://pitangent.com/python-developer/python-applications-strategies-best-practices/](https://pitangent.com/python-developer/python-applications-strategies-best-practices/)  
40. Guide to structured logging in Python \- New Relic, accessed on October 28, 2025, [https://newrelic.com/blog/how-to-relic/python-structured-logging](https://newrelic.com/blog/how-to-relic/python-structured-logging)  
41. Implementing Structured Logging in Python: A Comprehensive Guide \- Graph AI, accessed on October 28, 2025, [https://www.graphapp.ai/blog/implementing-structured-logging-in-python-a-comprehensive-guide](https://www.graphapp.ai/blog/implementing-structured-logging-in-python-a-comprehensive-guide)  
42. Python's structlog: Modern Structured Logging for Clean, JSON-Ready Logs \- Naveen Pn, accessed on October 28, 2025, [https://blog.naveenpn.com/pythons-structlog-modern-structured-logging-for-clean-json-ready-logs](https://blog.naveenpn.com/pythons-structlog-modern-structured-logging-for-clean-json-ready-logs)  
43. Structuring Python Logs for Better Observability \- Leapcell, accessed on October 28, 2025, [https://leapcell.io/blog/structuring-python-logs-for-better-observability](https://leapcell.io/blog/structuring-python-logs-for-better-observability)  
44. Python Logging with Structlog: A Comprehensive Guide \- Last9, accessed on October 28, 2025, [https://last9.io/blog/python-logging-with-structlog/](https://last9.io/blog/python-logging-with-structlog/)  
45. Python Logging Best Practices \- Obvious and Not-So-Obvious | SigNoz, accessed on October 28, 2025, [https://signoz.io/guides/python-logging-best-practices/](https://signoz.io/guides/python-logging-best-practices/)  
46. Structuring Your Project \- The Hitchhiker's Guide to Python, accessed on October 28, 2025, [https://docs.python-guide.org/writing/structure/](https://docs.python-guide.org/writing/structure/)  
47. Best Practices for Managing Python Dependencies \- GeeksforGeeks, accessed on October 28, 2025, [https://www.geeksforgeeks.org/python/best-practices-for-managing-python-dependencies/](https://www.geeksforgeeks.org/python/best-practices-for-managing-python-dependencies/)  
48. Dependency Management | IBM Data Science Best Practices, accessed on October 28, 2025, [https://ibm.github.io/data-science-best-practices/dependency\_management.html](https://ibm.github.io/data-science-best-practices/dependency_management.html)  
49. Mastering Python Dependency Management: Best Practices for Efficient Development, accessed on October 28, 2025, [https://ones.com/blog/mastering-python-dependency-management-best-practices/](https://ones.com/blog/mastering-python-dependency-management-best-practices/)  
50. Python Environment Management Best Practices \- Inedo Blog, accessed on October 28, 2025, [https://blog.inedo.com/python/python-environment-management-best-practices](https://blog.inedo.com/python/python-environment-management-best-practices)  
51. Virtualenv 101: The Secret to Managing Python Projects Like a Pro \- Medium, accessed on October 28, 2025, [https://medium.com/@jagtaprathmesh19/virtualenv-101-the-secret-to-managing-python-projects-like-a-pro-ee24c9f5362f](https://medium.com/@jagtaprathmesh19/virtualenv-101-the-secret-to-managing-python-projects-like-a-pro-ee24c9f5362f)  
52. Understanding when and how to use Conda, Pipenv, Virtualenv, Pip, and Poetry is crucial for efficient Python development. | by Mahesh K | Medium, accessed on October 28, 2025, [https://medium.com/@maheshkarthu/understanding-when-and-how-to-use-conda-pipenv-virtualenv-pip-and-poetry-is-crucial-for-2a518a951945](https://medium.com/@maheshkarthu/understanding-when-and-how-to-use-conda-pipenv-virtualenv-pip-and-poetry-is-crucial-for-2a518a951945)  
53. Pipenv vs Poetry: Complete Comparison Guide | Better Stack Community, accessed on October 28, 2025, [https://betterstack.com/community/comparisons/pipenv-vs-poetry/](https://betterstack.com/community/comparisons/pipenv-vs-poetry/)  
54. Cracking the Code: How to Make Your Codebase AI-Friendly (and Developer-Friendly\!) | by Darshan KN | Medium, accessed on October 28, 2025, [https://medium.com/@kndkdarshan/cracking-the-code-how-to-make-your-codebase-ai-friendly-and-developer-friendly-9299eb51100a](https://medium.com/@kndkdarshan/cracking-the-code-how-to-make-your-codebase-ai-friendly-and-developer-friendly-9299eb51100a)  
55. AI Instruction Best Practices \- Builder.io, accessed on October 28, 2025, [https://www.builder.io/c/docs/ai-instruction-best-practices](https://www.builder.io/c/docs/ai-instruction-best-practices)  
56. Building agents with the Claude Agent SDK \\ Anthropic, accessed on October 28, 2025, [https://www.anthropic.com/engineering/building-agents-with-the-claude-agent-sdk](https://www.anthropic.com/engineering/building-agents-with-the-claude-agent-sdk)  
57. Customize Claude Code with plugins \- Anthropic, accessed on October 28, 2025, [https://www.anthropic.com/news/claude-code-plugins](https://www.anthropic.com/news/claude-code-plugins)
