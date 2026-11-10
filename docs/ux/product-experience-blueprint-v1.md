# AION Workspace — Product Experience & UX Blueprint

**Version:** 1.0 · **Date:** November 2026 · **Status:** Proposed design direction (not implemented)

> This blueprint sets out the screen structure and interaction rules. It does not change the existing codebase or publish a prototype. Its roadmap phases are proposed design milestones, not Linear tickets.

## Product vision

AION Workspace should feel like a dedicated operating environment for running a business with AI. It should not feel like another chatbot, a generic CRM dashboard, or a set of disconnected SaaS tools.

The experience should answer five questions right away:

1. What is happening in my business?
2. What needs my attention?
3. What is AION working on?
4. What decisions am I responsible for?
5. What measurable results have we achieved?

---

## 1. Design direction: Precision Ops

The existing AION Workspace design system is already a strong foundation. It defines a restrained, dark, desktop-style operational interface with translucent chrome, Space Grotesk for text, IBM Plex Mono for technical IDs, and minimal motion. **Evolve it; do not replace it.**

**Visual foundation tokens:** Canvas · Surface · Action · Success · Review

**Principle:** *Quiet by default. Precise when action matters.*

- High contrast for important decisions.
- Dense operational information without visual clutter.
- A clear difference between real business data, simulations, and systems that are unavailable.

Keep the Web Desktop approach: a system bar, a maximized application surface, a floating dock, a launcher, and optional application windows. **Do not** replace it with a conventional permanent SaaS sidebar.

The codebase already has Today, Assistant, Missions, Approvals, Capabilities, Settings, and operator tools. The opportunity is to connect these into one coherent product, then gradually add Analytics and independent business modules.

## 2. Navigation system

There are three levels of navigation:

| Level | Contents |
|---|---|
| **System bar** | AION logo · Active application · Space · Authorized organization · Environment · Connection · Notifications · Account |
| **Application workspace** | Today, Copilot, Missions, Approvals, Analytics, or an embedded product module |
| **Floating dock** | Copilot · Missions · Apps · Files · Approvals · Settings |

### Universal shortcuts

- **⌘/Ctrl + Space** opens the **application launcher**. It searches and opens registered apps, including planned modules, which are shown as unavailable.
- **⌘/Ctrl + K** opens the **command palette** for finding missions, capabilities, artifacts, and authorized actions. Results must respect the current user's permissions and tenant.

The existing Personal / Work / Ideas **Spaces** stay as organizational views. They must not silently switch the authenticated tenant or grant access to another client's data.

## 3. Interactive screen concept

A UI concept with placeholders covers the six main experiences: **Today, Apps, Copilot, Missions, Approvals, Analytics**. The concept is labelled `CONCEPT / NO LIVE DATA`.

Example: the Copilot view, "Intelligence Desk":

- Heading: *Work with AION Copilot — What would you like to accomplish?*
- Composer: *Describe a business problem or ask for an analysis…*
- Stage rail: **Ask → Prepare → Review → Execute → Observe**
- Footer rule: *Copilot prepares proposals. Only Runtime authorizes execution.*

It is a visual preview of the product direction. It uses placeholders, not real AION business data, and is not an implemented or published application.

## 4. Screen-by-screen specification

### A. Today: the operational homepage

An executive briefing, not a static dashboard.

- **Primary goal:** Understand the business and find the next high-value action in under 30 seconds.
- **Primary CTA:** Ask AION / Prepare a Mission.

**Content hierarchy**

1. **Context header:** selected organization, environment, authenticated identity, and data freshness.
2. **Priority briefing:** blockers, required decisions, and relevant changes since the last session.
3. **Operational summary:** active missions, pending approvals, verified outcomes, and system health.
4. **Copilot composer:** a natural-language starting point for new work.
5. **Continuity section:** recent missions, evidence, and the last completed review.
6. **Recommended actions:** only suggestions grounded in available evidence, or ones explicitly labelled as hypotheses.

The hierarchy changes by role. A founder may see cash flow and cross-company priorities. A delivery operator may see client blockers and workflow exceptions. A client approver sees only their authorized work.

> **Rule:** Never fill the homepage with fabricated KPIs or demo missions when Runtime is disconnected.

### B. Apps: the module catalog

The **launcher** is built for fast navigation. The **catalog** helps users discover, understand, and manage applications. Each app has its own detail view:

| App detail field | UX purpose |
|---|---|
| Name, icon, and description | Explain the customer-facing outcome |
| Product owner and version | Establish accountability |
| Availability | Distinguish planned, available, connected, and offline |
| Tenant entitlement | Show whether this customer may use the module |
| Capabilities | Show registered, versioned Runtime service keys |
| Dependencies and health | Explain operational limits |
| Data handling | Identify who owns module data and which providers are connected |
| Evidence | Link to validated integration and acceptance records |

**Interaction rules**

- Planned modules can show descriptions and roadmaps but **cannot** have an enabled Launch workflow action.
- An app appearing in the launcher does **not** mean the customer's product is installed or authorized.
- Keep the existing `/capabilities` technical Service Catalog available to operators. Use `/apps` for the broader product-facing catalog once that route is built.

### C. Copilot: the Intelligence Desk

Copilot is the main natural-language interface to AION. It must **never** become a second authorization layer.

**Five-stage interaction**

1. **Ask:** The user asks a question or describes a business goal. AION retrieves permitted context and can answer without triggering execution.
2. **Prepare:** Copilot builds a structured proposal: workflow, service key, inputs, affected systems, expected outcomes, and constraints.
3. **Review:** The user sees the exact proposed command, risk classification, resource scope, costs if known, and any required approvals.
4. **Execute:** An eligible command goes through the Workspace BFF to Runtime. If human approval is needed, execution pauses at the canonical approval gate.
5. **Observe:** The UI opens or links the resulting mission and shows real execution status, errors, side effects, and verified outcomes.

**Layout:** On desktop, a two-pane Intelligence Desk. Conversation and context on the left; proposal, artifacts, evidence, or mission detail on the right.

**Useful interactions:** prepare an action from a chat message, copy a proposal, compare revisions, open the original source, and jump straight to a mission or approval.

> **Rule:** Copilot must say clearly whether it *answered*, *suggested*, *prepared*, *submitted*, *executed*, or *verified* something. These states mean different things.

### D. Missions: the execution center for all modules

Missions bring together work from Revenue OS, Creative OS, TechOps, and future applications.

**Mission detail regions**

- **Summary:** title, actual status, active tenant, originating module, initiating actor, and current blocker.
- **Timeline:** submission, policy decision, approval request, execution, verification, and outcome.
- **Artifacts:** logs, outputs, references, and revisions, shown only to authorized users.
- **Evidence inspector:** execution ID, service version, side-effect receipts, cost units, error history, and verification record.

**Mission groups (existing):** All · Needs Attention · In Flight · Completed · Failed/Cancelled.

**Status model (one possible path, not a fixed sequence):**

```
Queued → Running → Approval required → Verifying → Completed
```

Blocked, failed, cancelled, and reconciliation-required states branch off this path.

**Rules**

- A software process that finished successfully does not prove a verified business outcome. Show **Completed** only where the required outcome evidence exists.
- Do not invent percent-complete estimates for workflows that only expose discrete stages.

### E. Approvals: the decision inbox

This should be the most trustworthy screen in the product. Each approval has one main object: **the exact action a named human is being asked to authorize.**

**The decision view shows:** a before/after preview where it applies, the requesting actor, the affected system, the tenant, the risk, the expiry, the immutable revision/hash, and supporting evidence.

**Rules**

- Users approve or reject only within their own permissions.
- High-risk actions add a deliberate confirmation step.
- A rejection requires a reason.
- Expired or stale approvals cannot be executed.
- **Creative OS:** approving one edit revision does not authorize a modified revision.
- **CRM operations:** the approved command is checked again against current authorization and data state before it runs.
- An AION operator can never appear as the customer's approving human without authenticating that customer principal.

### F. Analytics: the intelligence and outcome layer

Focuses on performance and on confidence in the underlying evidence.

| Dashboard | Core metrics |
|---|---|
| Executive | Collected revenue, pipeline, gross margin where grounded, customer outcomes |
| Revenue OS | Inquiries, meaningful response times, qualified-to-booked rate, confirmed appointments |
| Operations | Mission completion, exceptions, task time, recovery, operator workload |
| AI Economics | Provider spend, cost per verified outcome, workflow cost, usage trends |
| TechOps | Control coverage, risk exposure, findings, remediation, verification |
| Creative OS | Content output, turnaround, approval rate, publication status, attributable demand |

**Every metric states:** its definition, source, last refresh time, tenant scope, date range, and whether it is *verified*, *estimated*, or *incomplete*.

Keep these differences visible. Pipeline value is not collected cash. A model-generated claim of time saved is not the same as measured staff effort reduced.

**First release:** read-only dashboards built from existing Runtime records and approved product sources. Broad analytics is a later product expansion, not a reason to delay the first execution certification.

## 5. Module interaction patterns

Users should feel they are working in one environment, even when applications are deployed separately.

**Cross-module workflow example**

1. **Creative OS:** prepare an approved campaign artifact.
2. **Runtime approval boundary:** verify the authorization, the revision, and the intended side effect.
3. **Revenue OS:** link authorized inquiries to campaign attribution.
4. **Analytics:** show verified outcomes and linked evidence.

**Rules**

- A remote module opens in a Workspace window with a short-lived, server-verified identity handoff. The module keeps control of its own UI and data. Runtime keeps authorization and approval authority.
- A module may ask Workspace to open a mission or approval by ID. Browser window messages must **never** carry credentials or act as approval authority.
- Context moves between modules through approved reference IDs and contracts, never by sharing complete customer datasets freely.

## 6. User roles and access

The current Workspace is built around authorized operators. A broader client experience first needs real customer identity and membership provisioning.

| Role | Primary experience | Authority |
|---|---|---|
| Founder / Executive | Today, cross-business reporting, portfolio decisions | Only grants explicitly assigned to that principal |
| AION Operator | Missions, exceptions, capabilities, technical evidence | Authorized operational actions |
| Delivery Lead | Client delivery, workflow health, service-level evidence | Assigned customer/workflow scope |
| Client Approver | Approvals, relevant mission artifacts and outcomes | Approve only as the authenticated client actor |
| Viewer / Analyst | Analytics, permitted reports, activity history | Read-only |

One person can hold several roles, but Workspace must work out effective permissions **server-side**. A role label, UI Space, or window selection is never an authorization grant.

## 7. First-use onboarding

New customers should not start with server configuration or raw execution IDs.

**Proposed external customer journey**

1. **Access:** receive an invitation and sign in securely.
2. **Organization:** choose an authorized customer workspace.
3. **Connections:** review the required systems and consent to scoped integrations through secure provider flows.
4. **Readiness:** see what is connected, missing, or awaiting verification.
5. **First action:** prepare a low-risk, reviewable business workflow.
6. **First value:** see a real outcome and learn what it means.

Operator onboarding can show a more technical connection check and environment verification.

> **Gate:** Do not enable customer onboarding until the tenant-membership and authentication model has been validated.

## 8. Responsive behavior and accessibility

| Screen size | Interaction model |
|---|---|
| 1440px+ desktop | Maximized work area, optional split panes and windows |
| 1024px laptop | Condensed system chrome, tabbed or collapsible evidence panes |
| 768px tablet | Full-screen applications with accessible app switching |
| 375px mobile | One application at a time, bottom dock/navigation, no draggable windows |

- **Motion:** restrained. About 180ms hover transitions, subtle window entrances, and full support for reduced-motion preferences.
- **Accessibility:** keyboard navigation, explicit focus states, readable contrast, 40–44px touch targets, and text labels for risk and status.
- **Never** show approval urgency through color alone.

## 9. Product data-truth states

This is a design requirement for the whole product, not a minor loading-state detail.

| State | What the user sees |
|---|---|
| Live and available | Real records from the source, with freshness context |
| Live but empty | A real empty state with no sample activity |
| Loading | A progress indicator that does not imply success |
| Unavailable | The dependency error, the affected functions, and a recovery action |
| Offline demo | A clearly labelled simulation, kept separate from production |
| Planned module | A description and readiness criteria, with execution disabled |
| Blocked action | The reason, plus the authorization or dependency that is missing |
| Verification pending | Execution status shown separately from a confirmed outcome |

The same rules apply to metrics, app badges, notifications, Copilot messages, and exported reports.

## 10. Design-system component library

Standardize these in the existing Workspace frontend. Do not create a separate design-system repository yet.

| Component family | Components |
|---|---|
| Shell | `SystemBar`, `AppDock`, `Launcher`, `SpaceSwitcher`, `AppWindow` |
| Identity | `TenantSelector`, `PrincipalLabel`, `EnvironmentState` |
| Action | `CommandComposer`, `CapabilityPicker`, `ActionPreview`, `RiskIndicator` |
| Missions | `MissionCard`, `MissionTimeline`, `ExecutionInspector`, `ArtifactViewer` |
| Approvals | `ApprovalCard`, `ChangeDiff`, `ApprovalConfirmation`, `DecisionReceipt` |
| Apps | `AppTile`, `AppDetail`, `AvailabilityState`, `CapabilityList` |
| Analytics | `MetricCard`, `EvidenceStatus`, `SourceHealth`, `TimeRangeSelector` |
| Reliability | `EmptyState`, `UnavailableState`, `RecoveryAction`, `DemoModeBanner` |

The existing `components/os/*`, `components/workspace/*`, `components/ui/*`, and `app/globals.css` stay the natural home for these primitives.

## 11. Engineering roadmap

Build the UX in slices rather than launching the whole environment at once.

| Phase | Focus | Acceptance |
|---|---|---|
| UX-01 | Shell and navigation refinement | Keyboard routes, responsive dock, authorized context visible |
| UX-02 | Today and Copilot continuity | Ask → Prepare → Review keeps the exact selected capability |
| UX-03 | Missions and approvals polish | Explicit blockers, revision safety, outcome evidence, negative-path tests |
| UX-04 | Deep app registry | Planned vs connected status, app ownership, metadata, entitlements |
| UX-05 | First module interaction | Fake adapter and read-only module flow, SSO tests, no authority bypass |
| UX-06 | Analytics foundation | Source-backed metrics, correct empty/unavailable states, verified attribution |
| UX-07 | External customer experience | Authorized membership, client principals, tenant isolation, client approvals |
| UX-08 | Multi-module polish | Context handoff, artifact references, mobile/a11y, consistent interactions |

These are proposed design milestones, not new Linear tickets.

> **Existing gates stay in place:** GE-001 is not certified, the Twenty cutover is incomplete, and Sentinel/Forge authorization still awaits independent reviews. No visual redesign should suggest these systems are ready for wider production deployment.

## 12. Design decisions to lock first

1. **Keep the current Web Desktop shell.** It fits AION's long-term direction better than a conventional sidebar dashboard.
2. **Copilot is the input layer, Missions is the execution record, and Approvals is the human authority layer.** Users should never have to guess which screen actually performs an action.
3. **Apps is the distribution layer and Analytics is the proof layer.** This lets Revenue OS, Creative OS, TechOps, and future products plug in without redesigning the platform each time.
4. **Operator-first initial experience.** An external customer launch needs the identity, tenant, entitlement, and security work already identified, not just a better onboarding screen.
5. **Creative OS is the first proposed module-contract conformance target, and Revenue Capture OS stays the first commercial proof priority.** These are separate goals and should not compete for the same production authorization.

## Reference architecture

This design follows the existing AION Command Center implementation, its Web Desktop Shell Specification, the Product Module Contract, and the proposed ADR-015.

## Next design deliverable

A high-fidelity, screen-linked prototype of the six core views, covering the full **Copilot → Mission → Approval → Verified Outcome** journey.
