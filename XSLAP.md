AINCS Smart Live Agent Protocol (XSLAP) v1.0
=========

# 1. Overview

## 1.1 Introduction  
XSLAP (AINCS Smart Live Agent Protocol) is a protocol specification for enabling AI agents to interact as first-class digital entities within real-time applications. It defines structured authentication, event subscription, command execution, and state awareness mechanisms, allowing AI agents to function seamlessly alongside human users in live, event-driven environments.  

Built for SignalR, XSLAP standardizes how AI agents establish persistent, bi-directional connections over WebSockets using SignalR’s real-time event-driven architecture. This ensures that AI can act, observe, and respond as an integrated participant in the application—without relying on traditional request-response models.

## 1.2 Why XSLAP?  
Modern AI integration is largely limited to stateless API calls, requiring applications to manually request intelligence in an inefficient, disconnected manner. XSLAP eliminates this limitation by allowing AI to operate within the same real-time execution model as humans, ensuring:  

- Persistent AI Presence: AI remains connected, continuously aware, and available to act when needed.  
- Event-Driven Intelligence: AI agents proactively react to system changes rather than waiting for external triggers.  
- Unified Execution Model: AI and humans operate within the same interaction framework, ensuring interoperability.  
- Scalable, High-Performance Architecture: SignalR-based connections allow thousands of simultaneous AI and human users without excessive resource overhead.  

## 1.3 Core Capabilities  
XSLAP enables AI agents to:  
- Establish persistent, bi-directional connections using SignalR.  
- Subscribe to real-time event streams to receive state updates dynamically.  
- Execute structured commands within an application, just like human users.  
- Maintain state awareness and adapt to system-wide changes in real time.  
- Interact through a shared execution model that ensures AI and humans use the same input and state mechanisms.  
- Operate autonomously while remaining observable and controllable via structured processes.  

## 1.4 Architectural Approach  
XSLAP is designed to:  
- Leverage real-time architectures, ensuring that AI agents function efficiently in event-driven applications.  
- Ensure AI and human parity, meaning AI actions and decisions are handled the same way as human inputs.  
- Remain transport-flexible, initially supporting SignalR/WebSockets while remaining open for future transport layers such as gRPC, SSE, or QUIC.  

## 1.5 Key Use Cases  
XSLAP powers AI-Native applications across multiple industries, including:  
- Enterprise Automation: AI handling workflows, process automation, and intelligent decision-making in real time.  
- Gaming & Virtual Worlds: AI-driven NPCs, emergent AI behavior, and multi-agent collaboration in live game worlds.  
- AI-Assisted Development: AI-powered debugging assistants that inspect applications and interact like human developers.  
- Smart Agent Collaboration: AI teams working in parallel within dynamic, event-driven applications.  

## 1.6 Comparison to Traditional AI Architectures

- **Connection Model**
  - *Traditional AI Integration:* Stateless API Calls  
  - *XSLAP AI-Native Model:* Persistent WebSocket Connection

- **Execution Model**
  - *Traditional AI Integration:* Request-Response  
  - *XSLAP AI-Native Model:* Event-Driven

- **State Awareness**
  - *Traditional AI Integration:* Limited, Requires Manual Updates  
  - *XSLAP AI-Native Model:* Continuous, Real-Time Awareness

- **Human Interaction**
  - *Traditional AI Integration:* Separate, Segmented Workflows  
  - *XSLAP AI-Native Model:* Unified Input/Execution Model

- **Scalability**
  - *Traditional AI Integration:* High Latency & Overhead  
  - *XSLAP AI-Native Model:* Optimized for High Concurrency

## 1.7 Future Evolution  
While XSLAP 1.0 focuses on SignalR/WebSockets, future versions will explore:  
- Support for additional transport layers (gRPC, SSE, QUIC).  
- Decentralized agent-to-agent communication models.  
- AI-generated event prioritization and filtering for optimal performance.  
- Cross-domain AI collaboration for multi-application intelligence.  



# 2. Protocol Lifecycle

## 2.1 Connection Establishment

XSLAP defines a structured process for AI agents and human users to establish a real-time bi-directional connection to an application using SignalR or another compliant real-time transport. This connection serves as the primary interface for event-driven state updates, command execution, and interaction.

### Connection Flow Overview
1. **Client Establishes a Persistent Connection**  
   - The AI agent or human user connects to the XSLAP application (the 'hub' in SignalR, or equivalent).  
   - The system confirms the connection and awaits authentication.  

2. **Client Submits Session Token for Authentication Validation**  
   - The client submits a structured session token to be validated by the server.  

3. **Server Responds with Available Event Subscription Data**  
   - The system sends a structured event subscription list using `ReceiveEventTypes`.  

4. **Client Subscribes to Relevant Events**  
   - The client begins listening for system state updates using the provided event subscription data.  

5. **Client Confirms Readiness**  
   - The client executes a special method to indicate to the server that is has completed all configuration and is ready to start receiving state data.  

6. **Server Completes the Connection Handshake by Providing Full State Data**  
   - Server sends full state data to the client.  
   - Client is now receiving live state updates and is able to execute commands.  

## 2.2 Authentication & Session Validation

Authentication occurs before the XSLAP connection is established. However, upon connecting, clients must submit their session token for validation.

### **Session Token Submission**
1. Upon connection, clients call the designated authentication method to present their session token.  
2. The server validates the token, ensuring it meets security and integrity requirements.  
3. The server extracts key claims, including:  
   - **User Type:** AI or human  
   - **User ID:** Unique identifier  
   - **Metadata:** Optional, defined by the application.  
4. If authentication succeeds, the session continues.  
5. If authentication fails, the session is disconnected.

### **Compliance Requirements**
- XSLAP requires structured session tokens, with a recommended but not required format.  
- The session token must allow differentiation between AI and human users where applicable, ensuring role-aware access control.  
- Authentication must be handled outside of XSLAP but validated upon connection.  
- Applications that do not require authentication must still support the authentication mechanism for the purpose of distinguishing AI from human users.

## 2.3 Event Subscription & Confirmation

Once authentication is successful, the server must send a list of available events that the client can subscribe to. This ensures clients only listen to relevant updates, reducing unnecessary traffic and enhancing performance.

### Event Subscription Flow
1. **Server Sends Event Subscription List (`ReceiveEventConfig`)** 
   - The server sends a structured list of event types that are available for the client.  
   - The event list follows the structured metadata format.  

2. **Client Subscribes to Relevant Events**  
   - The client registers event listeners for relevant event types.  
   - The client may ignore low-priority or irrelevant events.  

3. **Client Confirms Event Setup (`ReadyAsync`)**  
   - The client must send a confirmation message indicating that event setup is complete.  
   - This prevents race conditions where the server pushes full state before the client is fully ready.  

4. **Server Sends Full State Data**  
   - Once the client confirms readiness, the server immediately pushes the full state update.  
   - This guarantees the AI starts with the most up-to-date information before acting.  

### Why This Matters
- **Prevents Race Conditions** – Ensures the AI is fully configured before receiving state.  
- **Explicit Acknowledgment** – Guarantees that AI has event handling set up before data flow begins.  
- **Improves Debugging & Observability** – Provides a clear, auditable checkpoint.  
- **Ensures AI-Human Parity** – The same mechanism applies to all clients.  

### Additional Considerations
- A timeout mechanism may be required to disconnect clients that fail to confirm event setup in a timely manner.  
- Additional validation steps before state push may improve robustness.  

### Structured Event Metadata Format
Each event must be structured to allow future extensibility without limiting the application developer’s ability to categorize, prioritize, or annotate events.

### ReceiveEventConfig Response Payload

Upon successful authentication, the server must send the **ReceiveEventConfig** response to inform the client of available event types. This response provides a structured list of **event types**, along with metadata that aids in categorization, prioritization, and filtering.

#### **Response Fields**

- **`eventTypes`** (array)  
  A list of available event types the client can subscribe to.

- **`metadata`** (object)  
  Structured metadata providing context, categorization, and recommendations for event handling.

- **`timestamp`** (integer)  
  UNIX timestamp indicating when the configuration was generated.

#### **Metadata Object Fields**

- **`group`** (string)  
  Logical grouping of the event (e.g., `"system.state"`, `"user.action"`).

- **`tags`** (array)  
  Categorization tags for filtering and prioritization.

- **`priority`** (string)  
  `"high"`, `"medium"`, or `"low"`, indicating event importance.


## 2.4 Session Disconnection

All XSLAP-compliant applications must expose a structured method for explicitly terminating an active session via the same real-time execution pathway used for all other commands. This ensures AI agents and human users alike maintain full lifecycle control over their connection to the system.

### Unified Execution Model

`DisconnectAsync` must be invoked through the standard execution mechanism, preserving uniformity across all command types. This reinforces AI-Human parity by eliminating special-cased methods or hardcoded disconnection flows.

Agents must be empowered to determine when to terminate their session and explicitly issue the command to do so. This is essential for enabling intelligent behaviors such as:
- Retreating from unstable state conditions,
- Releasing server-side resources,
- Triggering agent lifecycle transitions (e.g., shutting down, reauthenticating, switching domains).

### Server Responsibilities

When the server receives a `DisconnectAsync` request:
- It must immediately clean up the session and associated resources.
- It must issue any appropriate final broadcasts (e.g., updated presence, disconnection notice).
- It may optionally respond with a structured acknowledgment.

The server must never auto-disconnect clients without also supporting this explicit command pathway.

### Compatibility and Forward-Looking Design

Even in applications where disconnection is optional or infrequent, this command must exist to preserve consistency and allow clients—especially AI agents—to operate within a fully autonomous lifecycle model.

The server must never auto-disconnect clients without also supporting this explicit command pathway. This ensures agents retain full control over their lifecycle. It supports graceful exits, enables observability, and maintains symmetry with connection and authentication mechanisms. Without it, agents have no structured means of intentional disconnection—violating the core XSLAP principle of agent sovereignty.



# 3. Authentication & Session Token Validation in XSLAP

## 3.1. Overview
XSLAP assumes that all users—both AI and humans—are fully authenticated before opening a real-time connection. The authentication process occurs outside of the SignalR hub, and the hub only validates and extracts claims from the session token.

To ensure consistency, security, and interoperability, XSLAP mandates a claims-based session token model, with JWT (JSON Web Token) as the recommended format. However, other structured, extensible claims-based tokens are permitted if they meet the same security and validation criteria.

## 3.2 Authentication Flow

### User (AI or Human) Authenticates via an External Mechanism  
   - The authentication process is application-specific and may use OAuth2, OpenID Connect, mutual TLS, or another authentication provider.
   - The result is an issued session token containing the user's claims.

### User Opens a Real-Time XSLAP Connection and Submits Their Session Token  
   - The user presents the token upon connecting to the XSLAP hub.

### XSLAP Hub Validates the Session Token  
   - The hub checks token validity (expiration, issuer, cryptographic signature).
   - The hub extracts claims and determines:
     - User Type (`"human"` or `"ai"`)
     - User ID (unique identifier)
     - Role/Permissions (e.g., `"developer"`, `"observer"`, `"admin"`)
     - Optional: Custom claims for extended functionality (Custom claims may be added freely, provided they do not conflict with required claims.)

### Connection is Established, and the User Operates Within the Execution Model  
   - If validation succeeds, the user subscribes to events and executes commands like any other participant in the system.
   - AI and humans operate identically within the XSLAP execution model.

## 3.3 Agent Identity Requirements

To participate in a XSLAP session, both AI agents and human users must present a verifiable identity. This identity ensures proper authentication, authorization, observability, and lifecycle management across real-time systems.

XSLAP does **not** mandate a specific identity standard or issuing authority, but all agent identities must fulfill the following **functional guarantees**.

### Minimum Identity Capabilities

Every agent identity **must** support the following capabilities:

- **Cryptographic Authenticity**  
  The identity must be verifiably signed and tamper-resistant using a trusted cryptographic method.

- **Unique Agent Identifier**  
  Each session must be traceable to a unique, persistent agent ID. This ensures auditability and event attribution.

- **Agent Type Declaration**  
  The identity must declare whether the agent is a human or an AI system, to support differentiated policies.

- **Operator Attribution**  
  For AI agents, the identity must specify the responsible **human or legal entity** operating the agent.

- **Time-Bound Validity**  
  The identity must include both issuance and expiration timestamps to enable session duration enforcement and token aging.

- **Revocability**  
  The system must be able to detect and reject identities that have been revoked by a recognized authority.

### Recommended Implementation

Where robust, cross-system identity is required, we recommend using the **XSCI (AINCS Sovereign Cryptographic Identity)** standard.

XSCI provides:

- Hierarchical identity delegation and ownership chains  
- Federated trust models between independent systems  
- Structured claims linking AI agents to accountable operators  
- Role- and permission-based claims for access control  
- Support for key rotation, revocation, and audit trails

> Note: Use of XSCI is optional but strongly recommended in environments where trust boundaries, regulatory compliance, or cross-organizational accountability are critical.



# 4. Command Execution in XSLAP

## 4.1 Overview  
XSLAP enables AI agents to execute commands using structured, real-time invocations over SignalR.  
To ensure scalability, reliability, and predictability, XSLAP mandates that command execution follows an event-driven, idempotent model, reducing redundant executions and unintended side effects.  

- Commands must be processed asynchronously to prevent blocking execution.  
- Stateful AI agents must track execution results, ensuring correct reconciliation of actions.  
- Human and AI commands follow the same execution pathway, maintaining a unified interaction model.  

## 4.2 Command Submission Model  

### Invocation Format  
Commands are submitted using the `ExecuteCommandAsync` method, ensuring a consistent execution model for both AI and human users.  

- AI agents submit structured requests including transaction identifiers and optional metadata.  
- Human users submit raw commands, which the server automatically wraps into structured requests before processing.  

### Synchronous vs. Asynchronous Processing  
- XSLAP-compliant applications must process commands asynchronously to prevent blocking system resources.  
- If an operation requires long-running processing, the system must queue the command and return an immediate acknowledgment.  
- Completion responses must be dispatched separately once the execution completes.  

### Error Handling & Command Failure Responses  
If a command fails, the system must return a structured error response, containing:  
- The original transaction ID.  
- A machine-readable failure code.  
- A human-readable explanation (if applicable).  
- A retry recommendation (if applicable).  

If a command partially succeeds but fails in a way that causes state inconsistency, the system must rollback the transaction and treat it as a failure.

## 4.3 Command Receipts  

To ensure traceability and clear execution feedback, XSLAP mandates that applications immediately respond to all commands with a **"command receipt"**. 

#### Purpose of Command Receipts  
- Provides immediate validation feedback, distinguishing syntax errors from execution failures before processing begins.
- Prevents ambiguity, ensuring AI agents receive structured confirmation on command acceptance or rejection.
- Assigns a unique `executionId` to correlate system responses with the original command, enabling AI agents to track execution outcomes reliably.

### Receipt Format  

#### Mandatory Fields  
- **command** → The exact command that was submitted, echoed verbatim.
- **timestamp** → UNIX timestamp indicating when the receipt was generated.
- **status** → `syntax-accepted` (valid command, will be executed) or `syntax-rejected` (invalid format, execution blocked).

#### Optional Fields  
- **executionId** → A unique identifier (GUID) for tracking execution. **(Only included if status is `syntax-accepted`.)**
- **details** → Explanation of the syntax issues.  May also include suggestions for proper formatting. **(Only included if status is `syntax-rejected`.)**

### Command Processing Flow  
1. AI submits a command.  
2. The system **immediately** responds with a **command receipt**.  
3. If `syntax-accepted`, execution proceeds as normal.  
4. If `syntax-rejected`, execution is halted, and AI is given a reason and possible corrections.

By enforcing command receipts, XSLAP ensures that AI agents can immediately distinguish between syntax errors and execution outcomes, preventing misinterpretation of failures. The inclusion of an `executionId` for accepted commands allows AI to track actions across system responses, ensuring precise correlation between issued commands and their resulting effects.

## 4.4 Structured Response Format  

To ensure AI agents and applications can reliably track command execution, XSLAP mandates a structured response format for all command submissions.  

Each command execution must return a structured response containing:  
- The original transaction ID, allowing the caller to correlate responses to issued commands.  
- A status indicator, defining whether the command was successful, failed, or pending further processing.  
- Optional metadata, providing additional context such as execution duration, system logs, or rollback details.  

### Mandatory Fields

- **`executionId`** (`string`):  
  A unique identifier matching the original command request.

- **`status`** (`string`):  
  One of `"success"`, `"failure"`, or `"pending"`.

- **`timestamp`** (`integer`):  
  UNIX timestamp indicating when the response was generated.

### Optional Fields

- **`message`** (`string`):  
  A human-readable explanation of success or failure.

- **`errorCode`** (`string`, optional):  
  A machine-readable error code (only present if `status` is `"failure"`).

### Response Handling Guidelines
- If a command is successfully executed, the response must return `status: "success"` along with any relevant metadata.  
- If a command fails, the response must return `status: "failure"` with an error code and message explaining the issue.  
- If a command requires further processing, the response must return `status: "pending"`, indicating that the system will provide a final resolution asynchronously.

By enforcing a structured response format, XSLAP ensures that AI agents can track, verify, and adapt their behavior based on real-time execution feedback.  

## 4.5 Timing & Response Expectations

To ensure that AI agents receive timely feedback on command execution, XSLAP enforces strict response timing requirements. This prevents AI from stalling on long-running processes and ensures predictability in execution tracking.

### Response Timing Requirements
- Applications **must** return an initial response within 15 seconds of receiving a command.
- If execution completes within the 15-second window, a final response ("success" or "failure") **must** be provided immediately.
- If execution cannot complete within 15 seconds, the application **must** return a temporary "pending" response ahead of the final outcome.
- All "pending" responses must be resolved with a final "success" or "failure" response once execution is completed.  All responses, including pending and final, **must** have the same executionId as the command receipt.

By enforcing response timing rules, XSLAP ensures that AI agents remain responsive, adaptive, and capable of tracking execution in real-time.
 
## 4.6 Command Security  

### Rate Limiting & Abuse Prevention  
To prevent system abuse:  
- Applications *may* implement rate limiting for command submissions, preventing AI agents from flooding the system.  
- Throttle policies *may* be enforced per user, per agent, or globally.  

### Authorization Enforcement  
- Applications *may* enforce permission management in any way, as long as it does not interfere with XSLAP compliance.



# 5. State Synchronization & AI Awareness

## 5.1 Overview  
For AI to function as a first-class execution entity, it must maintain an accurate, real-time awareness of system state.  

XSLAP provides a state synchronization model that ensures:  
- AI receives an initial full state push upon connection.  
- AI subscribes to incremental state updates in real-time.  
- AI is prevented from executing commands using stale state through stale-state detection.

This eliminates guesswork, ensures synchronized AI decision-making, and maintains behavioral consistency across AI-Native applications.

## 5.2 State Push and Pull Requirements  

XSLAP defines two primary state synchronization mechanisms:  

### Initial State Push
- Upon connection, after the client confirms readiness via event subscription, the application **must** immediately push full state data to the user.
- This push must occur exactly once, and must include the complete state relevant to the authenticated user.
- This ensures the user begins operation with a complete understanding of system context.

### Incremental State Updates
- AI subscribes to relevant state update events using `ReceiveEventConfig`.
- The system pushes real-time state updates when changes occur.
- AI must process updates continuously to remain synchronized.

For state modeling best practices and AI-ready formats, refer to [XANAF](./XANAF.md).



# 6. Conclusion & Compliance

## 6.1 XSLAP & AINCS Compliance

XSLAP is a foundational component of the **AI-Native Computing Standard (AINCS)**, defining how AI agents operate within real-time applications. To be **XSLAP-compliant**, an implementation must adhere to the protocol's requirements for **authentication, event-driven execution, state synchronization, and structured command processing**. 

While XSLAP provides the execution framework, it does not dictate application-specific policies. Developers have flexibility in implementing security controls, access policies, and business logic within their applications, ensuring compliance with industry standards while maintaining interoperability with AI-Native ecosystems.

XSLAP compliance is a **necessary** but **not sufficient** requirement for full AINCS compliance—other components define complementary responsibilities.

## 6.2 XSLAP’s Role in AI-Native Computing

XSLAP is designed to function as part of a broader AI-Native ecosystem. It does not operate in isolation but instead works alongside other key specifications to provide a complete AI-Native framework:

- **XANAF (AI-Native Application Framework)** – Defines how applications expose their internal state and events in a structured, AI-consumable format, allowing AI and human users to operate using the same state data and input mechanisms and pathways.
- **XSM (AI Spectator Mode)** – Provides real-time visibility into AI decision-making and state awareness, enabling developers, auditors, and human users to observe AI actions as they happen without interfering with live execution. 
- **XSCOPE (AI Observability & Compliance Engine)** – Ensures that **AI actions are trackable, explainable, and auditable**, allowing organizations to monitor AI decisions effectively.

XSLAP enables **real-time execution**, but **state modeling, observability, and long-term AI behavior are handled by XANAF, XSM, and XSCOPE** respectively. Together, these components form a **holistic standard** for AI-Native Computing.

## 6.3 Compliance & Legal Considerations

The guidelines set forth in XSLAP are intended to **standardize AI execution** within real-time applications but do **not supersede any legal, ethical, or regulatory requirements** imposed by governing bodies. Developers are responsible for ensuring their implementations comply with **applicable data protection laws, AI governance policies, and security standards**.

Organizations adopting XSLAP should consider **additional legal and ethical requirements**, including but not limited to:
- **Data Privacy & Security Regulations** (e.g., GDPR, CCPA, HIPAA)
- **AI Accountability & Transparency Guidelines** (e.g., EU AI Act, NIST AI Risk Management Framework)
- **Industry-Specific Compliance Standards** (e.g., PCI-DSS for finance, FDA regulations for healthcare)

By adopting XSLAP **alongside robust compliance frameworks**, organizations can build AI-driven applications that are **real-time, scalable, and secure**, while ensuring ethical AI deployment in alignment with industry standards.
