AINCS AI-Native Application Framework (XANAF) v1.0
=====

# 1. Overview  

## 1.1 Introduction  

AI is increasingly embedded in applications, yet most systems still treat AI as an afterthought—a bolt-on feature rather than a first-class participant. Current AI integration methods rely on patched-together APIs, manual data extraction, and UI-bound state, forcing AI to interact in ways fundamentally different from human users.  

This fragmented approach introduces severe inefficiencies:  
- AI and human users operate through separate execution pathways, leading to inconsistent behavior and duplicated logic.  
- AI must be manually adapted per application, requiring custom integrations, predefined commands, and human intervention.  
- AI cannot dynamically interpret system state—instead, it relies on hardcoded mappings, unstructured text parsing, or brittle screen-scraping techniques.  
- Execution feedback is inconsistent and unstructured, preventing AI from reasoning about outcomes deterministically.  
- AI is forced to interact with systems using human-designed interfaces rather than machine-readable state, limiting its ability to function autonomously.  

These inefficiencies create technical debt, fragmented execution models, and AI that remains dependent on human oversight.  

XANAF (AINCS AI-Native Application Framework) solves this by providing a unified standard where AI and human users interact seamlessly, identically, and predictably.  


## 1.2 Why XANAF?  

AI integration today is fragmented—applications rely on separate APIs, human-readable state, and predefined interactions that require AI to be manually adapted per system. These inefficiencies create technical debt, inconsistent execution models, and limited AI autonomy.  

XANAF eliminates the inefficiencies of retrofitting AI into traditional application architecture by enforcing:  
- A single execution model for AI and humans—no parallel AI-specific logic.  
- Structured, machine-readable state exposure—AI can interpret state naturally.  
- Dynamic input discoverability—AI can enumerate valid actions at runtime.  
- Explicit success/failure feedback—ensuring AI understands execution outcomes deterministically.  

With XANAF, applications become AI-native by design, allowing AI agents to function autonomously without pre-training, special APIs, or workarounds.  

### Comparison to Traditional AI Integration

#### **Execution Path**  
- **Traditional Model:** Custom AI APIs, manual adaptation  
- **AI-Native Model:** AI & humans use the same execution model  

#### **State Exposure**  
- **Traditional Model:** UI-bound, human-centric  
- **AI-Native Model:** Machine-readable, structured state  

#### **AI Input**  
- **Traditional Model:** Hardcoded commands, per-app tuning  
- **AI-Native Model:** AI dynamically discovers valid actions  

#### **Training**  
- **Traditional Model:** AI must be pre-configured per system  
- **AI-Native Model:** AI learns interactions dynamically  

### 1.3 Core Capabilities & Architectural Approach  

XANAF enforces an AI-Native execution model by standardizing how applications expose state, accept input, and handle execution feedback.  XANAF assumes that AI agents begin with no prior knowledge of application structure or semantics. All valid commands must be discoverable from system state, with no hidden pathways or special instructions assumed.

### 1.3.1 Event-Driven Execution Model  
- All actions (AI & human) trigger structured events.  
- State updates propagate incrementally in real time.  
- No polling or blocking execution.  

### 1.3.2 Unified Input & State Management  
- AI and humans submit commands through the same structured input pipeline.  
- State is exposed in a structured, machine-readable format—free from UI constraints.  

### 1.3.3 Dynamic Input Discovery  
- AI autonomously queries available actions—no predefined command lists.  
- Input schemas are structured and constraint-aware.  

### 1.3.4 Explicit Execution Feedback  
- All commands return structured success/failure responses—no ambiguity.  
- Execution results are machine-verifiable and deterministic.  

### 1.3.5 Separation of UI & Business Logic  
- State representations are AI-first, not UI-dependent.  
- Execution logic remains purely business-driven.  


# 2. Unified Input Pipeline (AI and Human Input Must Be Identical)  

XANAF mandates a single, structured input pipeline where AI and human users interact through the same execution model, input structures, and validation constraints.  

This requirement is essential to AINCS (AI-Native Computing Standard) because:  
- It eliminates execution fragmentation—ensuring AI does not require separate APIs, custom integrations, or side-channel workarounds.  
- It guarantees predictability—AI operates under the same constraints as human users, preventing unintended behaviors caused by parallel execution pathways.  
- It ensures AI can function autonomously—AI must be able to observe, reason, and act using the same mechanisms as human users, without manual adaptation.  
- It simplifies development and compliance—developers write one unified input model instead of managing AI-specific logic, reducing complexity and long-term technical debt.  

AI-specific input pathways, hidden execution channels, or UI-driven adaptations are strictly prohibited, as they break AI-human parity, introduce inconsistencies, and violate the foundational principles of AINCS.  

## 2.1 Mandatory Input Channels  

### 2.1.1 Standard Command Execution Channel  
All user interactions—whether initiated by AI or human users—must be processed through a standardized command execution model with the following characteristics:  
- Every input action is explicitly defined in a structured, machine-readable format.  
- Execution constraints (e.g., permissions, preconditions) must be machine-readable to prevent ambiguous AI behavior.  
- Command execution must be event-driven, not request-response-based.  

### 2.1.2 Additional Input Channels (Extensible, Application-Specific)  
While command execution is the primary required input channel, applications may support additional structured input methods as needed, provided that:  
- AI and human users interact identically within each additional input channel.  
- Input data is structured and machine-readable.  
- AI is not required to infer meaning from human-centric interfaces.  

Example extensible input models include:  
- Declarative Form-Based Submission → AI and humans submit structured forms to initiate system interactions.  
- Command-Driven Event Triggers → AI and humans trigger predefined, structured system events.  
- Streaming Input Channels → AI and humans interact through continuous structured data streams.  

These additional models are optional but must always conform to XANAF’s unified input requirements if implemented.  

## 2.2 Prohibited Input Models  

To ensure full AI-human parity, XANAF explicitly prohibits any input models that introduce segregated execution pathways or require AI-specific adaptations.  

### No AI-Specific Endpoints  
- AI must submit input through the same structured command execution model as humans.  
- No special APIs, hidden system calls, or privileged AI input mechanisms are allowed.  

### No UI-Bound Input Models for AI  
- AI must never be forced to parse human-designed interfaces (e.g., HTML-based interactions, screen scraping).  
- All AI inputs must be structured, machine-readable, and independent of UI layers.  

### No Hidden Execution Pathways  
- All valid user actions must be discoverable through structured input definitions.  
- AI should never require undocumented or hidden mechanisms to interact with the system.  

## 2.3 Standardized Command Structure  

XANAF enforces a structured command format to guarantee deterministic execution and prevent AI-human input discrepancies.  

### 2.3.1 Explicit Command Definitions  
Every command must be fully self-describing, ensuring AI can issue valid actions without relying on external documentation, predefined mappings, or implicit system rules. Each command must include:  
- Command Type → Specifies the action to be performed.  
- Parameters → Structured definitions of required and optional inputs.  

AI must construct commands using only the structured input schema provided by the system—it must not rely on undocumented behaviors, inferred execution rules, or hidden system constraints. 

### 2.3.2 Identical Execution Path for AI and Humans  
- AI and humans must execute the same structured commands with the same constraints.  
- The system must not differentiate AI-originated vs. human-originated commands at the execution level.  
- Commands must be processed through the event-driven execution model.  Blocking or request-response-based processing is prohibited.  

### 2.3.3 Compliance & Validation  
To be XANAF-compliant, an application must:
- Process all AI and human input through the standardized command execution model.
- Ensure commands are structured, explicit, and self-describing.
- Prohibit separate AI input pathways, UI-based interactions, or implicit execution mechanisms.



# **3. State Exposure**  

## 3.1 State Must Be Structured and Machine-Readable  

XANAF requires that all application state be structured in a way that AI can interpret directly, without relying on human-readable formats, UI elements, or unstructured text.  

- State must be explicitly structured and consistently formatted.  
- AI must be able to consume state without manual interpretation, pre-training, or additional formatting layers.  
- State must be sufficiently descriptive for AI to reason about system conditions and available actions without guesswork or ambiguity.  
- Developers may choose any format that meets these requirements.  

Any system exposing state in UI-bound, human-readable, or inconsistent formats is not compliant with XANAF.  

## 3.2 Required State Components  

While XANAF does not enforce a specific format, it does require that state include the following structured components to support AI-native operation:  

- **Entity ID** – A unique, persistent identifier for each object in the system.  
- **Additional Properties** – A structured representation of the entity’s current state.  
- **Valid Actions** – A discoverable list of actions that can be performed on or by this entity.  

State must include these components in a structured and consistent manner to ensure AI does not require human-like inference, pre-defined mappings, or UI workarounds to function. If any of these components are missing or ambiguous, AI cannot reliably function autonomously, and the system is not AI-native.  

## 3.3 No UI-Centric Data Allowed  

To ensure state is fully consumable by AI, XANAF prohibits:  

- HTML, CSS, or visual formatting in state responses.  
- Natural language descriptions of critical state data, in place of structured attributes.  
- UI-specific elements such as pagination controls, tooltips, or visual metadata.  

### Examples of Non-Compliant State Exposure  

- State containing embedded HTML elements, CSS classes, or visual formatting.  
- State requiring AI to extract meaning from unstructured text descriptions.  
- UI-bound representations that mix structured data with human-readable output.  

### Examples of Compliant State Exposure  

- State is structured for direct AI interpretation without human-readable formatting.  
- Entities expose their identifiers, additional properties, and valid actions in a consistent, structured manner.  
- State updates are incremental and event-driven, preventing stale execution. 

Any system that fails to meet these requirements cannot be considered AI-native under XANAF.  



# 4. Discoverability of Input Pathways  

## 4.1 The Two-Model Approach  

XANAF defines two distinct models for exposing state and available actions, ensuring AI agents can interpret and act upon system state without ambiguity. These models cover the full range of AI-Native applications:  

1. Schema-Defined Model → For structured, CRUD-like applications.  
2. Direct-Action Model → For highly dynamic, emergent AI interactions.  

By supporting both models, XANAF remains flexible while enforcing strict clarity in AI-readable state exposure.  Every AI-native system must adhere to one or both models, depending on its needs.

XANAF does not dictate AI behavior but assumes AI can process structured input without prior training. The system is responsible for ensuring that all valid interactions are surfaced in a machine-readable format, allowing AI to determine available actions dynamically based on exposed state.

## 4.2 Schema-Defined Model  

### Purpose  

The Schema-Defined Model is designed for applications where data follows a structured format and predictable execution patterns. This model is best suited for:  

- Enterprise & Business Applications (CRUD-based systems)  
- Forms & Transactional Workflows (e.g., structured data submission)  
- AI Assistants that rely on defined schemas for data interpretation  

The Schema-Defined Model requires that all entity definitions conform to a structured validation format. JSON Schema is the recommended standard, but equivalent structured validation frameworks may be used, provided they enforce explicit constraints, data types, and required fields in a machine-readable manner.

### Core Principles  

- **Explicitly Defined Structure** – Entities must be described using a structured schema that outlines constraints, data types, and required fields.  
- **Predefined Validation Rules** – Input validation follows strict formatting, ensuring data integrity before execution.  
- **Structured Action Exposure** – AI can only execute actions that have been explicitly defined within the schema.  

### Implementation  

- Each entity type is defined with a schema describing its structure, constraints, and valid inputs.  
- AI dynamically reads valid actions from the entity definition and associated state data.  
- Commands must follow a structured execution format, ensuring consistent and predictable behavior.  

### Command Format  

- Commands must be explicitly structured and must be ready to execute immediately once placeholders are populated with valid data.
- Placeholders are wrapped in curly braces {} to indicate dynamically inserted values.
- The command must be executable as-is after placeholder substitution, without requiring additional transformation or processing.
- All required parameters must be explicitly defined within the schema to prevent AI from executing incomplete or invalid commands.
- Commands must follow a consistent, machine-readable format to ensure AI agents can reliably construct and execute them.

### Actions

- Actions **must** have the following properties defined: `label` *(think: button text)* and `command` *(the command that will be executed)*.
- Optionally, the property `actionHints` may be used to provide advisory context describing dynamic conditions, dependencies, or validation rules related to the action.

Example:  
*An AI assistant submitting structured data into a system follows predefined schemas for entity types.*  

```json
{
  "entities": [
    {
      "type": "sprocket",
      "schema": {
        "$schema": "https://json-schema.org/draft/2020-12/schema",
        "type": "object",
        "properties": {
          "name": { "type": "string", "minLength": 3, "maxLength": 50 },
          "description": { "type": "string", "maxLength": 255 }
        },
        "required": ["name"]
      },
      "data": [
        {
          "id": "12345",
          "name": "Example Sprocket",
          "description": "A high-quality example sprocket, built for AI-native systems."
        }
      ],
      "actions": [
        {
          "label": "Submit",
          "command": "create sprocket {\"name\": \"{name}\", \"description\": \"{description}\"}",
          "actionHints": [
            "User must have edit permissions.",
            "Sprocket name cannot be empty."
          ]
        }
      ]
    }
  ]
}
```

## 4.3 Direct-Action Model  

### Purpose  

The Direct-Action Model is designed for real-time, dynamic environments where AI interactions emerge based on evolving conditions. This model is best suited for:  

- Real-Time Simulations & Games  
- AI-Driven Worlds & Adaptive Environments  
- Event-Based Interaction Systems  

This model enables AI to interpret and respond to state changes fluidly, without the overhead of predefined schemas.  

**Important:** If parameter constraints are required, the Schema-Defined Model must be used. The Direct-Action Model assumes AI will infer constraints dynamically based on system feedback.

### Core Principles  

- **State-Driven Action Discovery** – AI reads available actions directly from state, without referencing predefined constraints.  
- **Minimal Overhead** – No schema enforcement, allowing AI to act dynamically as the system evolves.  
- **Immediate Execution** – If a command appears in state data, it is assumed to be executable.  

### Implementation  

- AI agents read state updates to determine available actions dynamically.  
- Actions surface within state as simple execution commands rather than structured schemas.  
- The system determines what is possible at any given time based on the state itself.  

### Command Format  

- Commands must be fully formed, except for placeholders requiring substitution.
- Placeholders are wrapped in curly braces {} to indicate dynamically inserted values.
- Commands should follow the system’s native execution format (e.g., command-line-like syntax, structured keywords).
- If parameters are required, they should be formatted predictably within the command string to ensure AI can substitute values correctly.
- The command must be immediately executable once placeholders are populated—no additional transformation steps should be required.

Example:  
*In a game world, AI sees “take i|abc123” attached to an item, instantly knowing it can pick it up.*  

```json
{
  "entities": {
    "data": [
      {
        "title": "Enchanted Flask",
        "description": "A mystical flask shimmering with an eerie glow.",
        "commands": [
          "take i|abc123",
          "use i|abc123"
        ]
      }
    ]
  }
}
```

### Command Processing

XANAF-compliant applications must process AI-issued commands **in a structured, predictable, and reliable manner.**  While XANAF does not enforce a specific transport or execution model, **it strongly recommends** implementing command execution using the **[AINCS Smart Live Agent Protocol (XSLAP)](./XSLAP.md).**  

XSLAP provides a proven framework for **structured command execution, tracking, and feedback,** ensuring AI agents can:  
- **Receive immediate validation feedback** on submitted commands.  
- **Track execution outcomes deterministically** using structured responses.  
- **Avoid ambiguous failures** by receiving explicit error reasons.  
- **Adapt dynamically to system state** through reliable transaction correlation.  

#### **Minimum Command Processing Requirements**  
Regardless of implementation, all XANAF-compliant applications **must** adhere to the following core principles in command execution:  

- **Commands must be processed asynchronously.** Execution **must not block system resources**, and AI must not be left waiting indefinitely for results.  
- **Command receipts must be issued immediately.** AI must receive an acknowledgment confirming **whether a command was syntactically valid** before execution begins.  
- **All executed commands must have a unique execution identifier.** This ensures AI can correlate actions with subsequent success/failure feedback.  
- **Success and failure responses must be structured and machine-readable.** AI **must not** rely on parsing unstructured text to determine execution outcomes.  
- **If a command cannot be resolved within 15 seconds,** the system must issue a **temporary "pending" response** before the final resolution.
- State updates resulting from a user's command execution **must** be pushed by the application in real-time, as they occur.

By enforcing explicit success and failure feedback, XANAF guarantees that AI agents can operate autonomously without relying on human-designed workarounds or manual intervention. Systems that fail to comply introduce unnecessary ambiguity, making them **AI-retrofitted, not AI-native**.

### 4.4 Meta Actions (Global Action List)

While most commands in XANAF are contextual to entities (e.g., actions tied to a specific item or user), **applications may also expose global or meta-level commands** that are not bound to any one entity.

These actions typically affect the overall session, environment, or application state and may include behaviors like:

- Full state refresh (i.e. `GetState`)
- Application-level settings or configuration
- Global help or support access
- Navigation, view toggles, or layout control

#### Key Properties:
- **Must appear in the discoverable state object**  
  Meta actions must be delivered like all other commands—AI agents should discover them by inspecting state.

- **Can be used with either Schema-Defined or Direct-Action models**  
  These are **not** tied to a specific modeling approach. They exist independently of entities.

- **Placed in the root of the state object**  
  To signal their global nature, meta actions are typically listed under a top-level `actions` array.

This pattern ensures that **AI agents receive a complete, machine-readable menu of valid interactions**, including commands that operate beyond individual entities. By placing meta actions at the root level, applications maintain clarity, simplicity, and discoverability across all models.

```json
{
  "actions": [
    {
      "label": "Refresh the Application State. This is your 'turn it off and back on again' command.",
      "command": "GetState"
    },
    {
      "label": "Change Theme",
      "command": "set theme {\"mode\": \"{dark|light}\"}"
    }
  ],
  "entities": [
    {
      "type": "user",
      "data": [
        {
          "id": "u123",
          "name": "Alice",
          "actions": [
            {
              "label": "Update Profile",
              "command": "edit profile {\"name\": \"{name}\"}"
            }
          ]
        }
      ]
    }
  ]
}
```

### 4.5 Recommended Meta-Actions: GetState

While XANAF does not mandate any specific commands, it is considered best practice for applications to expose a discoverable command that performs a **full state refresh**. This action is commonly referred to as `GetState`.

#### Purpose
AI agents operating in long-lived sessions or dynamic environments may occasionally require a reset of their internal state to reestablish context. A full-state refresh allows them to:
- Recover from local desynchronization.
- Rehydrate memory after being restarted.
- Verify that their world model is consistent with server state.

#### Recommendations
- This action should be presented to the user like any other discoverable command.
- It should be explicitly labeled, clearly indicating that it provides a full state snapshot.
- The behavior of the command should be equivalent to the initial state push on connection.

#### Example Implementations
- `"command": "GetState"`  
- `"command": "refresh context"`


### 4.6 Stale-State Detection (Optional Best Practice)

To ensure AI agents do not act on outdated information, applications may optionally implement **stale-state validation** during command execution. This is especially useful in multi-user or real-time systems where state changes frequently.

#### Overview

When an AI issues a command, the system compares the agent’s last known state version received (tracked by the application) to the current system version. If a mismatch is detected, the command is rejected, and an updated state is pushed to the AI for reevaluation.

#### Why It Matters

- Prevents execution based on outdated assumptions
- Improves safety and correctness in shared or volatile systems
- Enables smarter retry logic in autonomous agents

#### Implementation Recommendations

- Validate per-entity version, not global state
- If stale, respond with a structured error and full state refresh
- Provide clear reasons for rejection (e.g., `"error": "state_version_outdated"`)



## 5. Conclusion  

XANAF establishes the foundational principles for **AI-Native application design**, ensuring that AI agents can interact with systems as **first-class participants**, without the inefficiencies of legacy integration approaches.  

By enforcing **structured state exposure, unified input pathways, and explicit execution feedback**, XANAF eliminates ambiguity, reduces technical debt, and creates an environment where AI can function autonomously without pre-training, custom integrations, or human-designed workarounds. 

