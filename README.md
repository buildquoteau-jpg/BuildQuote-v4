	# BuildQuote-v4
BuildQuote V3 – AI‑Native Platform (Master Working Document)
Part 1. BuildQuote Overview – Read Me
BuildQuote is an AI‑native materials coordination platform for builders and trades. It is not an estimating tool, not a takeoff tool, and not a pricing engine. Its sole purpose is to help builders structure, coordinate, and send material quote requests (RFQs) to suppliers, while keeping judgement, compliance, and final decisions firmly with the builder and engineer.   BuildQuote sits between takeoff/estimating tools and suppliers. It assumes builders are competent professionals and uses AI only to assist with coordination, clarity, and speed — never authority.
Part 2. BuildQuote Tone & Copy System
Tone principles:  - Professional, calm, competent  - Assumes builder expertise  - Never authoritative  - Never prescriptive  - Always optional and reviewable   Language rules:  - Do not use: estimating, takeoff, costing, margins, winning bids  - Use: quote request, materials required, systems, components, supplier coordination   Suggestion / hint text rules:  - Always greyed, optional, non‑blocking  - Stage‑relevant only  - Never recommend brands unless proprietary  - May include South‑West WA‑style examples (e.g. Yallingup Rd, Southwest Constructions)  
Part 3. Screen‑by‑Screen Platform Flow
S0 – Sign In → Builder Dashboard   S1 – Builder Dashboard  - Welcome back (builder name)  - New Project / Add to Existing Project  - Project tiles (scrollable)  - Sandbox project always visible  - Archived projects accessible   S2 – Project Setup  - Project name  - Site address  - Build stage selection (including Builder Custom Stage)   S3 – Scope Input  - 1–2 sentence scope of works  - Example suggestion text per stage   S4 – Suggested Component Groups  - AI suggests typical material groups  - Tick / untick list  - Option to add missing component group   S5 – Component Specification Screen  - Scrollable single screen  - Select specs, dimensions, pack sizes, quantities  - Custom line item always available   S6 – Review & Supplier Details  - Delivery / pickup  - Dates (optional)  - Message to supplier  - Select supplier or add new  - Save draft   S7 – RFQ Preview & Send  - HTML + PDF preview  - Send RFQ  - Supplier acknowledgement tracking  




Part 4. File Structure & Responsibilities
/app/dashboard – builder dashboard UI  /app/projects – project lifecycle  /app/quoteRequest/buildUp – core materials coordination screens  /app/quoteRequest/review – review & supplier details  /app/quoteRequest/preview – RFQ preview & send  /lib/ai – AI prompts, safety rules, orchestration  /lib/documents – PDFs, guides, references  /lib/data – Convex schemas  /lib/import (future) – materials list import   Each file must declare:  - What it owns  - What it does NOT decide  
Part 5. How to Build BuildQuote – Day by Day Plan
Day 1–2: Lock UX flow and copy  Day 3–4: Finalise schemas and boundaries  Day 5–7: Build core UI screens  Day 8–9: Supplier RFQ flow + email logic  Day 10: Sandbox, polish, disclaimers  
Part 6. Future‑Proofing BuildQuote (AI‑Native 2026+)
- Keep AI assistive, not authoritative  - Separate coordination from judgement  - Maintain global vs builder‑specific memory separation  - Avoid seat‑based thinking  - Design for invisible quality (trust, speed, clarity)   Reserved future concepts:  - Import materials lists  - Builder memory library (non‑pricing)  - Advisory standards references








BUILDQUOTE V3 — LOCKED SPEC

Screens S1 → S3 (Expanded, Code-Ready)

⸻

S1 — Builder Dashboard

Purpose

The Builder Dashboard is the single landing page after sign-in.
It gives builders immediate situational awareness and a clear next action without cognitive load.

This screen contains no AI decisions — only orchestration.

⸻

Primary User Intents
	•	Start a new project
	•	Continue work on an existing project
	•	View RFQ status at a glance
	•	Practice safely (Sandbox)
	•	Archive finished projects

⸻

UI Structure

Header
	•	“Welcome back, {Builder First Name}”
	•	Optional secondary line:
                     “You have {X} active quote requests.”

Primary Actions (Top of Screen)
	•	Primary CTA: New project
	•	Secondary CTA: Add to existing project

These buttons must be visually dominant and immediately tappable on mobile.


Project Tiles (Scrollable Grid or Horizontal Scroll)
Each project tile displays:
	•	Project name
	•	Site suburb (not full address)
	•	Build stage(s) active
	•	RFQ summary:
	•	Sent
	•	Will quote
	•	Can’t quote
	•	Pending response

Tap behaviour:
	•	Opens Project Overview
	•	Does not auto-start a new RFQ


Sandbox Project (Pinned)
	•	Always visible
	•	Clearly labelled:
“Sandbox (practice — suppliers will not be contacted)”
	•	Contains:
	•	guided hint copy
	•	example scopes
	•	dummy suppliers

Archived Projects (Collapsed Section)
	•	Hidden by default
	•	Expandable
	•	Read-only

⸻

Data Read / Write

Reads:
	•	projects[]
	•	quoteRequests[]
	•	supplierResponses[]

Writes:
❌ None

AI Rules
	•	❌ No AI suggestions
	•	❌ No interpretation
	•	❌ No automation

Copy Rules
	•	Confident
	•	Neutral
	•	No pressure
	•	No “getting started” fluff

⸻

S2 — Project Setup

Purpose

Create or select the context container that all quote requests live inside.

A project may have multiple quote requests over time.

Inputs

1. Project Name
	•	Free text
	•	Example suggestion (greyed):
                        “Example: Smith Residence”

2. Site Address
	•	Address lookup or manual entry
	•	Stored once per project
	•	Editable later

3. Build Stage Selection
Single-select list (radio or tile style):
	•	Slab / Footings
	•	Wall Framing
	•	Roof Framing
	•	Roofing
	•	External Cladding
	•	Internal Lining
            •	Insulation
            •	Brickies corner
	•	Decking / Pergola / Outdoor Structures
	•	Builder Custom Stage

Builder Custom Stage:
	•	Opens text input
	•	Example:
                      “Example: Pool house framing”

Important Rule*

Build stage is for organisation and suggestion context only.
It does not affect pricing, compliance, or supplier eligibility.

Data Written

Creates or updates:
project {
  id
  name
  siteAddress
  stages[]
}

AI Rules
	•	❌ AI does not validate stage
	•	❌ AI does not restrict stages
	•	✅ AI may later reference stage for suggestion copy

⸻

S3 — Scope of Works Input

Purpose

Allow the builder to describe the intent of the quote request in their own words, in a way that AI can interpret without overruling judgement.

This is the primary AI input for system suggestions.

UI Elements

Instruction (Short, Respectful)
“Provide a brief scope of works for this quote request.”


Scope Input
	•	Text box (2–3 lines visible)
	•	Voice-to-text enabled (mobile)
	•	Soft character guidance (not enforced):
                         “1–2 sentences is usually enough.”

Stage-Specific Example (Greyed Suggestion)
Dynamically generated based on stage selected in S2.

Example — Decking / Pergola / Outdoor Structures:

“Example: Supply of H4 structural posts, post stirrups, concrete and fixings for an external pergola structure.”

Builder Control Rules
	•	Builder wording is never corrected
	•	Builder wording is preserved verbatim
	•	Builder can edit later at any time


AI Behaviour (Strictly Limited)

AI may:
	•	identify likely component groups
	•	identify likely systems (generic vs proprietary)
	•	prepare a draft suggestion only

AI must never:
	•	infer quantities
	•	infer compliance
	•	infer suitability
	•	recommend brands (unless proprietary is explicitly named)

Data Written
quoteRequest {
  id
  projectId
  stage
  scopeText
}

Transition

On submit → S4 Suggested Component Groups


Global Disclaimer Logic (Applied from S3 onward)

This paragraph must appear once per quote flow, collapsible:

“BuildQuote assists with structuring material quote requests only.
It does not make engineering, compliance, quantity, or suitability decisions.
All selections must be reviewed and confirmed by the builder and relevant professionals.”


S4 & S5 — Component Logic, Driver Items, and Builder Control


S4 — Suggested Component Groups

Purpose

Translate the builder’s scope of works into a clear, editable list of material component groups that will later be specified in detail.

This screen answers one question only:

“What broad groups of materials are required for this quote request?”

It does not deal with specs, sizes, quantities, brands, or pricing.


Input Sources
	•	Build stage (from S2)
	•	Scope of works text (from S3)


What the AI Does (Strictly)

AI performs classification, not decision-making.

AI may:
	•	map scope text → typical component groups
	•	group materials logically (systems thinking)
	•	distinguish generic systems vs proprietary systems

AI must never:
	•	add quantities
	•	remove groups automatically
	•	imply completeness
	•	recommend brands unless proprietary system is explicitly named


UI Structure

Intro Copy (Short, Professional)

“Based on your scope, the following material groups are typically included.
Please untick any that do not apply, or add a group if required.”

Component Group List

Each group appears as a row with a tick toggle.

Example (Decking / Pergola / Outdoor Structures):
	•	☑ Structural posts
	•	☑ Post stirrups / brackets
	•	☑ Concrete / footing products
	•	☑ Fixings & fasteners
	•	☑ Site accessories (string line, packers, etc.)


Controls
	•	Toggle on/off per group
	•	Add component group (free text)
	•	Example placeholder:
                     “e.g. Custom steel brackets”

Rules
	•	Default state: all suggested groups ticked
	•	Unticking removes the group entirely from S5
	•	No required / optional labels
	•	Builder can re-enter S4 later and change selections


Data Written
quoteRequest.componentGroups = [
  {
    id,
    name,
    source: "ai_suggested" | "builder_added",
    included: true
  }
]


Transition

Continue → S5 Material Selection & Specification


S5 — Quote Builder (Specifications & Quantities)

⚠️ This is the core screen of BuildQuote.
Everything else supports this.

Purpose

Allow the builder to explicitly define what they want quoted, in supplier-ready terms, without estimating, pricing, or assumptions.

Screen Layout (Critical UX Decision)
	•	Single scrollable screen
	•	No step-by-step wizard
	•	No forced sequencing
	•	Builder must be able to scroll up/down and revise freely

This mirrors how builders actually think.

High-Level Structure

For each Component Group (from S4):
[ Component Group Header ]
  ├─ Driver Item (if applicable)
  ├─ One or more Line Items
  └─ Add custom line item
⸻

Driver Item Concept (LOCK THIS)

A Driver Item is the item whose specification affects the relevance of other items in the same group.

Examples
	•	Structural posts → drives stirrup compatibility
	•	Timber size → drives fixings/brackets suitability
	•	Steel section → drives connectors

Rules
	•	Only one driver per component group
	•	Driver must be selected first
	•	Driver does NOT auto-populate other items
	•	Driver only filters reference suggestions


Driver Item UI

Example: Structural Posts

Fields:
	1.	Material type
	•	Timber
	•	Steel
	2.	Treatment / grade (if timber)
	•	H3
	•	H4
	3.	Section size
	•	90×90
	•	100×100
	•	135×135
	•	Custom
	4.	Length
	5.	Purchase unit
	•	Each
	•	Pack (if applicable)
	6.	Quantity

Builder can select Custom at any point and type freely.

Line Item Model (Applied Everywhere)

Every non-driver item uses the same structure:

Line Item Fields
	1.	Item category
	•	e.g. Post stirrup, Anchor bolt, Rapid set concrete
	2.	Specification / variant
	•	e.g. Galvanised, Heavy duty
	3.	Dimensions (if applicable)
	•	e.g. 12mm × 150mm
	4.	Purchase unit
	•	Each | Pack | Box | Bag
	5.	Pack size (conditional)
	•	e.g. Box of 25
	6.	Quantity


Pack vs Each Logic
	•	Default unit may be suggested by category
	•	Builder can always override
	•	Switching unit does not recalculate quantities
	•	No assumptions about wastage or overage


Reference Documents (Advisory Only)

If available:
	•	“View installation guide”
	•	“View technical data”

Rules:
	•	Read-only
	•	Downloadable
	•	Never positioned as instructions
	•	Never required

⸻

Custom Line Items

Available in every component group.

Purpose:
	•	Capture items outside the library
	•	Allow supplier-specific quirks
	•	Avoid blocking the builder

AI Behaviour on S5

AI may:
	•	suggest item categories
	•	suggest typical variants
	•	filter reference docs

AI must never:
	•	add items automatically
	•	select specs
	•	change quantities
	•	remove builder entries

Data Written

quoteRequest.lineItems[] = {
  id,
  componentGroupId,
  isDriver,
  description,
  spec,
  dimensions,
  unit,
  packSize,
  quantity,
  source: "ai_suggested" | "builder_defined" | "imported"
}

Builder Guarantees

At all times, the builder must be able to:
	•	scroll back
	•	change quantities
	•	change specs
	•	add/remove items
	•	add custom items

No lock-in until send.

S5.5 — Add Additional Scope or Missed Item (Optional)

Before proceeding to supplier review, the builder is given an optional opportunity to extend or amend the quote request.

This screen exists to reduce anxiety, prevent omissions, and reflect real-world quoting behaviour where scope often expands incrementally.

Purpose
	•	Allow builders to add more work without restarting the quote
	•	Capture missed materials or late scope changes
	•	Maintain a single RFQ while supporting multi-system requests

Available Actions

The builder may choose one or both of the following:

1. Add Another Scope / System
Used when the quote requires an additional system or stage (e.g. adding cladding to a framing quote).
	•	Builder selects a build stage (e.g. Wall Framing, External Cladding, Decking & Pergolas, Builder Custom Stage)
	•	Builder enters a 1–2 sentence scope of works
	•	BuildQuote returns to S3–S5 flow for that additional system
	•	Newly added system is appended to the existing quote request

2. Add a Single Missed Line Item
Used when only one material or accessory was overlooked.
	•	Free-text description field
	•	Quantity and unit selector
	•	Optional notes field
	•	Item is clearly marked as “Builder-Added Custom Item”

Design Notes
	•	No pricing is introduced
	•	No engineering decisions are made
	•	All additions are editable prior to sending
	•	Builder can skip this screen entirely if no changes are needed

Outcome

Once confirmed, the full combined quote proceeds to S6 — Review & Supplier Details.

Transition

Continue → S6 Review & Supplier Details


S6 & S7 — Review, Supplier Selection, RFQ Preview & Lifecycle

S6 — Review & Supplier Details

Purpose

Finalize logistics, supplier selection, and context before generating and sending the RFQ.
This screen confirms intent, not content: no materials editing here.


Primary User Intents
	•	Confirm delivery or pickup details
	•	Add timing context
	•	Add a message to the supplier
	•	Select the supplier to send to
	•	Save a draft or proceed to preview

UI Structure

Section A — Fulfilment Method
	•	Toggle: Delivery | Pickup

If Delivery:
	•	Site address (prefilled from Project; editable)
	•	Optional delivery window (free text)

If Pickup:
	•	Pickup suburb/location (optional)

Copy note (small):
“Details are provided for supplier context only.”


Section B — Project Timing (Optional)
	•	Expected project start date (date picker)
	•	Required-by date (date picker)

Dates are optional and never validated.


Section C — Message to Supplier
	•	Label: Message to supplier (optional)
	•	Input:
	•	Multiline text
	•	Voice-to-text (mobile)
	•	Placeholder example:
                 “Please advise lead times and availability.”

Rules
	•	No AI rewriting by default
	•	(Future: optional “polish message” toggle)


Section D — Supplier Selection
	•	Dropdown of saved suppliers (name + email)
	•	Add new supplier (inline modal):
	•	Supplier name
	•	Email address
	•	Optional phone
	•	Supplier selection is single-select (one RFQ at a time)

Copy note:
“Each quote request is sent to one supplier at a time.”

Section E — Actions
	•	Save draft
	•	Continue to preview


Boundaries
	•	❌ No sending from S6
	•	❌ No bulk/multi-supplier send
	•	❌ No materials editing
	•	❌ No AI decisions


Data Written

quoteRequest {
  fulfilmentType: "delivery" | "pickup",
  deliveryDetails?: { address, window },
  pickupLocation?: string,
  projectStartDate?: Date,
  requiredByDate?: Date,
  supplierMessage?: string,
  selectedSupplierId: string,
  status: "draft"
}


Transition

Continue → S7 RFQ Preview & Send

S7 — RFQ Preview & Send

Purpose

Show exactly what the supplier will receive, then allow an intentional send.
This is the final checkpoint.

UI Structure

Section A — RFQ Preview
	•	Default: HTML preview (fast, readable)
	•	Buttons:
	•	Download PDF
	•	Download CSV

Preview must include:
	•	Project name & stage
	•	Site suburb
	•	Fulfilment method
	•	Dates (if provided)
	•	Materials list (grouped as defined in S5)
	•	Supplier message

Rule: Preview must match the sent email 1:1.


Section B — Summary Panel (Side / Top)
	•	Supplier name
	•	Stage
	•	Delivery / Pickup
	•	Required-by date (if any)


Section C — Final Actions
	•	Send quote request (primary)
	•	Back to edit (returns to S6)
	•	Save draft


On Send — System Actions (LOCK THIS)
	1.	Create SupplierRFQ record:

Data written

supplierRFQ {
  id,
  quoteRequestId,
  supplierId,
  sentAt,
  status: "sent"
}


End of screen explanation 


🔒 BUILDQUOTE V3 — DATA MODEL (CONVEX SCHEMAS)

Design principles (locked)
	•	No pricing fields anywhere
	•	Builder intent is preserved verbatim
	•	AI suggestions are always distinguishable from builder input
	•	RFQs are immutable once sent
	•	One RFQ → one supplier (always)

1. builders

Represents the account owner.
builders {
  _id
  firstName
  lastName
  companyName

  email
  phone?
  address?

  abn?
  acn?

  createdAt
}
Notes
	•	One builder → many projects
	•	Auth handled elsewhere (Clerk etc.)


2. projects

Long-lived container for work.
projects {
  _id
  builderId
  name
  siteAddress
  archived: boolean
  createdAt
}
quoteRequests {
  _id
  projectId
  builderId

  stage               // e.g. "Decking / Pergola"
  customStageLabel?   // if Builder Custom Stage

  scopeText           // verbatim builder input

  status              // "draft" | "sent"

  fulfilmentType      // "delivery" | "pickup"
  deliveryDetails? {
    address
    window
  }
  pickupLocation?

  projectStartDate?
  requiredByDate?

  supplierMessage?

  createdAt
  updatedAt
}

Rules
	•	Editable until sent
	•	After send → read-only except status


4. componentGroups

Logical groupings selected in S4.
componentGroups {
  _id
  quoteRequestId
  name                // e.g. "Structural posts"
  source              // "ai_suggested" | "builder_added"
  included: boolean
  orderIndex
}

Rules
	•	Unticked groups remain stored (included=false)
	•	Order preserved for UI rendering
5. lineItems

The core of BuildQuote.

lineItems {
  _id
  quoteRequestId
  componentGroupId

  isDriver: boolean

  description         // free text / category
  spec?               // grade, treatment, type
  dimensions?         // string, not parsed (e.g. "135x135 x 4.8m")

  unit                // "each" | "pack" | "box" | "bag"
  packSize?           // e.g. "Box of 25"
  quantity: number

  source              // "builder_defined" | "ai_suggested" | "imported"
  originalText?       // for imports

  createdAt
}

Critical rules
	•	No totals
	•	No derived fields
	•	Quantity is always tied to unit
	•	Dimensions are descriptive, not numeric (avoids false authority)


6. suppliers

Builder-managed contacts.
suppliers {
  _id
  builderId

  name
  email
  phone?

  builderAccountRef?     // supplier-specific account name or number
  tradingTerms?          // "COD" | "Proforma" | "EOM" | "30 days" | free text
  notes?                 // internal builder notes

  createdAt
}
quoteRequests {
  ...
  scopes: [
    { id, stageContext?, scopeText, createdAt }
  ]
}


7. supplierRFQs

Tracks each send.

supplierRFQs {
  _id
  quoteRequestId
  supplierId

  sentAt
  status            // "sent" | "will_quote" | "declined"

  respondedAt?
  declineReason?
}
Rules
	•	Immutable payload after send
	•	Multiple RFQs allowed per quoteRequest (sent sequentially, not bulk)

8. documents (reference only)

Installation guides, tech data, etc.
documents {
  _id
  title
  type              // "installation" | "tech_data" | "span_table"
  relatedCategory?  // e.g. "post_stirrups"
  fileUrl
}

Rules
	•	Read-only
	•	Never mandatory
	•	Never implies compliance


9. sandboxFlags (optional helper)

Keeps sandbox behaviour isolated.
sandboxFlags {
  projectId
  isSandbox: boolean
}


Relationship Map (mental model)
Builder
 └── Projects
      └── QuoteRequests
           ├── ComponentGroups
           │     └── LineItems
           └── SupplierRFQs
Documents sit outside this tree and are referenced, never embedded.



Why this model works
	•	Matches your UX exactly
	•	Keeps AI safely bounded
	•	Allows future imports without refactor
	•	Prevents pricing creep
	•	Scales from solo builders to teams


End of section

🧱 BUILDQUOTE V3 — FILE STRUCTURE + RESPONSIBILITIES

(AI-native, Convex-backed, builder-first)

This assumes:
	•	Frontend: React (Vite / Next / similar)
	•	Backend: Convex
	•	AI calls: Cloudflare Workers (later)
	•	Auth: Clerk (or similar)


TOP-LEVEL REPO STRUCTURE
buildquote-v3/
├─ app/                     ← UI + routing
├─ components/              ← reusable UI blocks
├─ convex/                  ← data + business logic (SOURCE OF TRUTH)
├─ lib/                     ← helpers (AI, parsing, formatting)
├─ prompts/                 ← AI system prompts (locked copy)
├─ types/                   ← shared TS types
├─ docs/                    ← internal docs (non-runtime)
└─ public/
⸻

1️⃣ convex/ — THE BRAIN (LOCK THIS FIRST)

If something changes data → it lives here
If AI suggests something → it writes through here
convex/
├─ schema.ts
├─ builders.ts
├─ projects.ts
├─ quoteRequests.ts
├─ componentGroups.ts
├─ lineItems.ts
├─ suppliers.ts
├─ supplierRFQs.ts
├─ documents.ts
└─ utils.ts
⸻

convex/schema.ts

Defines all tables (already mostly locked).

Reads: none
Writes: everything

Purpose:
	•	Single source of truth
	•	Prevents accidental pricing creep
	•	Makes refactors visible


convex/builders.ts
createBuilder()
getBuilder()
updateBuilder()
Used by:
	•	Sign-in flow
	•	Dashboard header
	•	RFQ footer metadata

Never touched by AI.

convex/projects.ts

createProject()
listProjectsByBuilder()
archiveProject()

Used by:
	•	Dashboard tiles
	•	Sandbox project
	•	Project picker

⸻

convex/quoteRequests.ts

createDraftQuote()
updateQuoteMeta()
lockQuoteForSend()
quoteRequests {
  ...
  scopes: [
    { id, stageContext?, scopeText, createdAt }
  ]
}

Key rule
Once status === "sent" → write protection

Used by:
	•	S2–S6 screens
	•	Supplier send flow
	•	Review screen


convex/componentGroups.ts

createSuggestedGroups()
toggleGroupIncluded()
reorderGroups()

Used by:
	•	S4 “Materials overview” screen
	•	AI suggestion injection
	•	Builder untick/tick UX

AI can:
	•	create groups
	•	never auto-include silently

convex/lineItems.ts

addLineItem()
updateLineItem()
removeLineItem()

Used by:
	•	Core build-up screen
	•	Driver item logic
	•	Custom line items
	•	Future import feature

AI rules:
	•	Can suggest line items
	•	Must mark source: "ai_suggested"

convex/suppliers.ts

createSupplier()
updateSupplier()
listSuppliers()

Used by:
	•	Supplier selector (S6)
	•	Account ref + terms display
	•	Builder notes

Never AI-created.

convex/supplierRFQs.ts

sendRFQ()
recordSupplierResponse()

Used by:
	•	Send screen
	•	Email webhook callbacks
	•	Dashboard RFQ timeline

Immutable payload after send.

convex/documents.ts

listDocuments()

Used by:
	•	Dashboard “Guides” tab
	•	Product reference links

Never required. Never authoritative.


2️⃣ app/ — SCREENS (USER FLOW LOCKED)
app/
├─ sign-in/
├─ dashboard/
├─ project/
│   ├─ [projectId]/
│   │   ├─ s1-start/
│   │   ├─ s2-stage/
│   │   ├─ s3-scope/
│   │   ├─ s4-materials/
│   │   ├─ s5-build-up/
│   │   ├─ s6-review/
│   │   └─ s7-preview/
⸻

S1 — Dashboard (dashboard/)

Reads
	•	builders
	•	projects
	•	supplierRFQs (summary)

Actions
	•	New project
	•	Open project
	•	Sandbox project

⸻

S2 — Stage Selection

Writes
	•	quoteRequests.stage
	•	quoteRequests.customStageLabel

No AI yet

⸻

S3 — Scope Input

Writes
	•	quoteRequests.scopeText

AI role
	•	Read only
	•	Prepare internal understanding (no output yet)

⸻

S4 — Materials Overview

Reads
	•	AI suggestions → componentGroups
	•	Existing componentGroups

Builder action
	•	Tick / untick groups
	•	Add missing group

Critical

This screen creates trust.

⸻

S5 — Build-Up Screen (CORE)

Rename internally as:

Request Build-Up

Reads
	•	componentGroups
	•	lineItems

Builder controls
	•	Spec
	•	Dimensions
	•	Unit
	•	Pack size
	•	Quantity
	•	Custom items

AI role
	•	Inline suggestions
	•	Never auto-edit

⸻

S6 — Review + Supplier

Reads
	•	quoteRequests
	•	suppliers
	•	lineItems

Writes
	•	fulfilment
	•	delivery details
	•	supplier message

⸻

S7 — RFQ Preview

Read-only
	•	PDF
	•	CSV
	•	HTML email

Final confirmation before send.


3️⃣ components/ — REUSABLE UI

🔐 LOCKED CHANGE: S5 ➜ S6 TRANSITION

New micro-step introduced:

S5.5 — “Add more before review” (Optional Gate)

This sits between Screen 5 (Request Build-Up) and Screen 6 (Review & Supplier).

✅ WHAT IS LOCKED IN

1️⃣ Multiple scopes per quote (future-proofed, clean)
	•	A single RFQ can now contain multiple scopes / systems
	•	Example:
	•	Scope 1: Outdoor structures – monkey bar supports
	•	Scope 2: External cladding
	•	Each scope:
	•	Has its own short scope text
	•	Generates its own suggested component groups
	•	Builds into the same RFQ

Canonical structure (locked):
quoteRequests.scopes[] = [
  { id, stageContext, scopeText, createdAt }
]

2️⃣ Two optional actions before review (builder-safe)

At the moment the builder clicks “Continue” from S5, they see:

“Anything else to include?”

They may choose to:

A) Add another scope / system
	•	1–2 sentence scope input
	•	Re-runs:
	•	system inference
	•	component group suggestions
	•	build-up selection
	•	Returns builder to S5 with a new scope section added

B) Add a single missed item
	•	Quick-add line item modal
	•	Supports:
	•	description
	•	spec / dimensions
	•	unit (each / pack / box / bag)
	•	pack size
	•	quantity
	•	optional component group

C) Continue to review
	•	Proceeds to S6 with no changes
3️⃣ No schema bloat
	•	No duplicate quote
	•	No branching RFQs
	•	No supplier selection yet
	•	Line items remain flat and editable

4️⃣ AI boundaries (explicitly locked)
	•	AI may:
	•	suggest component groups for new scopes
	•	help interpret scope text
	•	AI may not:
	•	auto-add scopes
	•	auto-add line items
	•	auto-select products
	•	auto-send RFQs

Builder judgement always wins.

5️⃣ UI / file impact (minimal)
	•	No new full screen
	•	Implemented as:
	•	inline interstitial OR modal component
	•	New reusable component:
components/AddMoreGate.tsx

3️⃣ components/ — REUSABLE UI
components/
├─ StageSelector.tsx
├─ ScopeInput.tsx
├─ ComponentGroupList.tsx
├─ LineItemEditor.tsx
├─ SupplierPicker.tsx
├─ DisclaimerBlock.tsx
└─ AIHint.tsx
Each component:
	•	Receives data
	•	Emits events
	•	Never owns state truth

4️⃣ prompts/ — AI GOVERNANCE (CRITICAL)
prompts/
├─ system.md
├─ suggestion-rules.md
├─ disclaimer.md
└─ stage-context/
   ├─ decking.md
   ├─ framing.md
   └─ custom.md
These are:
	•	Versioned
	•	Human-readable
	•	Auditable

No prompts embedded in code.

5️⃣ lib/ — HELPERS
lib/
├─ aiClient.ts
├─ textNormalizer.ts
├─ takeoffImport.ts   ← future
└─ unitHelpers.ts
No Convex writes directly here — helpers only.

⸻

6️⃣ types/ — SHARED CONTRACTS
types/
├─ quote.ts
├─ lineItem.ts
├─ supplier.ts
└─ stage.ts

1. Manufacturer Portal

(Core Data Source for BuildQuote V3 — Reference + System Generation)

The Manufacturer Portal is a core part of the BuildQuote V3
It serves two distinct but connected functions:
	1.	Supplying trusted manufacturer documentation and reference material to builders
	2.	Supplying structured source data used by the BuildQuote AI to generate systems, component groups, and suggested materials

The Manufacturer Portal is not part of the builder RFQ flow, but it is a foundational backend system that powers AI-native behaviour throughout the platform.


1.1 Manufacturer-Supplied Source Material

Manufacturers provide BuildQuote with authoritative documentation, including but not limited to:
	•	Product catalogues
	•	Product specification sheets
	•	Installation guides
	•	Span tables
	•	System manuals
	•	Technical data sheets

These are typically provided as PDFs or official documents produced by the manufacturer.

Key rules:
	•	BuildQuote does not scrape public websites for this data
	•	Manufacturers supply and approve their own documentation
	•	Documents are versioned and date-stamped
	•	Older documents may be retained for reference but marked as superseded

1.2 Dual Use of Manufacturer Data

Manufacturer documentation is used in two ways within BuildQuote:

A) Builder Reference Content (Human-Facing)

Manufacturer documents are surfaced to builders via the Guides & Downloads library.

Builders can:
	•	View installation guides
	•	Download specification sheets
	•	Access span tables on site
	•	Reuse documents across multiple projects

Important boundaries:
	•	Documents are always reference-only
	•	Documents do not imply compliance or suitability
	•	Builders remain responsible for engineering checks and approvals
	•	Viewing documents is never required to complete a quote

⸻

B) AI System Generation (Machine-Facing)

Manufacturer documentation is also processed by the BuildQuote AI to extract structured knowledge.

This process includes:
	•	Identifying defined manufacturer systems (e.g. cladding systems, framing systems)
	•	Identifying components that belong to each system
	•	Understanding relationships between components (e.g. post size ↔ stirrup type ↔ fixing requirements)
	•	Extracting constraints, dependencies, and compatibility rules where explicitly stated by the manufacturer

This process is commonly referred to internally as chunking, where large documents are broken into structured, searchable units that are stored in Convex.

Outputs stored in Convex include:
	•	System definitions
	•	Component group templates
	•	Component compatibility mappings
	•	Reference links back to source documents

1.3 AI Boundaries and Safety

The AI uses manufacturer-derived systems to:
	•	Suggest relevant systems based on builder scope
	•	Suggest component groups for a given system
	•	Suggest typical materials that belong together

The AI does not:
	•	Select final products on behalf of the builder
	•	Enforce manufacturer-specific brands unless the system is explicitly proprietary
	•	Make engineering, compliance, or quantity decisions
	•	Override builder selections

All AI outputs remain advisory and editable.

1.4 Proprietary vs Generic Systems

The Manufacturer Portal supports both:

Proprietary Systems
	•	Systems where all components must be sourced from a single manufacturer
	•	Example: proprietary cladding or decking systems

In these cases:
	•	Brand specificity is explicit
	•	System boundaries are clear
	•	Builder is informed that the system is proprietary

Generic Systems
	•	Systems composed of interchangeable components available from multiple manufacturers
	•	Example: structural posts, fixings, fasteners, concrete

In these cases:
	•	AI suggestions remain brand-agnostic
	•	Builders choose manufacturers and suppliers later in the flow
	•	Manufacturer data is used to inform compatibility, not dictate sourcing

1.5 Architectural Role of the Manufacturer Portal

From an architectural perspective, the Manufacturer Portal:
	•	Feeds structured system data into Convex
	•	Feeds reference documents into the Builder Guides library
	•	Acts as a controlled, auditable source of truth
	•	Enables AI-native behaviour without scraping or hallucination
	•	Allows BuildQuote to improve system intelligence over time without changing the builder UX

The Manufacturer Portal is therefore a core backend capability, even though it is not directly visible during the builder’s RFQ workflow.

1.6 Summary

The Manufacturer Portal in BuildQuote V3:
	•	Is part of the MVP
	•	Supplies both human-facing reference content and machine-readable system data
	•	Powers AI-native system and component suggestions
	•	Preserves builder judgement and responsibility
	•	Eliminates reliance on scraped or unreliable sources

This design ensures BuildQuote remains accurate, trusted, scalable, and genuinely AI-native.

2. Supplier Management (Builder Dashboard Tab)

The Builder Dashboard includes a Suppliers tab where builders manage supplier relationships independently of any quote request.

Builders can:
	•	Add and edit suppliers
	•	Store supplier-specific identifiers (account name or number)
	•	Record supplier trading terms (e.g. COD, Proforma, EOM, 30 days)
	•	Add internal notes for their own reference

Key boundaries:
	•	Supplier data is builder-specific
	•	AI does not create, edit, or prioritise suppliers
	•	Supplier data does not influence material suggestions

This reflects how builders actually work and removes repetitive data entry during quoting.

3. Saved Builder Items (Reusable Custom Items)

When a builder adds a custom line item, they may choose to save it for reuse.

Saved items:
	•	Are stored in a builder-specific library
	•	Can be inserted into any future quote request
	•	Remain editable every time they are used

Important distinctions:
	•	Saved items are not treated as systems
	•	Saved items do not become defaults
	•	Saved items do not affect global AI behaviour

This feature exists solely to reduce friction and repetition for experienced builders.

4. Guides & Downloads Library (Builder Dashboard Tab)

The Builder Dashboard includes a Guides & Downloads tab that is independent of projects and quote requests.

Builders can:
	•	Access installation guides
	•	Download technical and specification PDFs
	•	Save commonly referenced documents
	•	Reuse documents across multiple jobs

Design intent:
	•	Builders often need reference material on site
	•	Documents may apply across many projects
	•	Access should not require navigating into a specific quote or project

Rules:
	•	Documents are optional
	•	Documents are reference-only
	•	Documents do not imply compliance or approval

5. Reusing Sent Quote Requests (Duplicate to Draft)

Once a quote request has been sent to a supplier, it becomes immutable.

Builders may:
	•	Duplicate a previously sent quote request
	•	Create a new draft based on it
	•	Make changes before sending to another supplier

Editable fields in a duplicated draft include:
	•	Project name and address
	•	Dates (required-by, project start)
	•	Quantities
	•	Supplier selection
	•	Supplier-specific notes

Key rule:
	•	Original sent RFQs remain unchanged
	•	Duplicated drafts are treated as new quote requests

This allows builders to efficiently request pricing from multiple suppliers without rebuilding the request.

Architectural Note

All systems in this section:
	•	Are part of the BuildQuote V3 
	•	Are implemented as adjacent systems, not inline steps
	•	Do not interrupt the primary RFQ flow
	•	Require no later refactoring to scale or extend

They are documented here to ensure clear boundaries, clean implementation, and long-term maintainability.

Codex — apply the “Option 1: Builder-Grade Industrial” colour system across the app using CSS variables + a small UI kit. Keep it mobile-first, calm, and professional (site office vibe). No bright/childish colours. Use brass sparingly and NEVER for warnings.

PALETTE (LOCK THESE IN)
- Primary (steel):        #2F3E4F
- Secondary (slate):      #445C70
- Background (concrete):  #F4F6F8
- Surface (white):        #FFFFFF
- Border (light):         #D7DEE6
- Text (primary):         #1F2A33
- Text (muted):           #5D6B78
- Accent (brass):         #B08D57   (use sparingly: selected state, tiny highlights, small badges)
- Success (muted green):  #3E7C59
- Warning (muted amber):  #8A6A3D   (IMPORTANT: must be visually distinct from brass)
- Danger (brick):         #A94442

================================================================================
1) CREATE TOKENS
================================================================================
Create /src/styles/tokens.css with variables (use these exact names):

:root {
  --bq-bg: #F4F6F8;
  --bq-surface: #FFFFFF;
  --bq-primary: #2F3E4F;
  --bq-secondary: #445C70;
  --bq-accent: #B08D57;
  --bq-success: #3E7C59;
  --bq-warning: #8A6A3D;
  --bq-danger: #A94442;

  --bq-text: #1F2A33;
  --bq-text-muted: #5D6B78;
  --bq-border: #D7DEE6;

  --bq-radius-sm: 10px;
  --bq-radius-md: 14px;
  --bq-radius-lg: 18px;

  --bq-shadow-sm: 0 1px 2px rgba(0,0,0,.06);
  --bq-shadow-md: 0 8px 24px rgba(0,0,0,.10);

  --bq-space-1: 6px;
  --bq-space-2: 10px;
  --bq-space-3: 14px;
  --bq-space-4: 18px;
  --bq-space-5: 24px;

  --bq-font: ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, Helvetica, Arial;
}

Wire tokens.css into the main entry so all screens get it (main.tsx or index.css).

================================================================================
2) BASE STYLES (MOBILE FIRST)
================================================================================
Create /src/styles/base.css:
- body background: var(--bq-bg)
- default text color: var(--bq-text)
- headings weight: 650–750
- reduce visual noise: no heavy gradients, no neon accents
- card spacing optimized for one-handed mobile use
- max width on web: content container 720px centered

================================================================================
3) COMPONENT STYLES (BUTTONS / INPUTS / CARDS)
================================================================================
Create /src/styles/components.css with shared classes:

Buttons:
- min-height: 48px
- radius: var(--bq-radius-md)
- Primary: background var(--bq-primary), text white, subtle shadow
- Secondary: white background, border var(--bq-border), text var(--bq-primary)
- Accent button (rare): background var(--bq-accent), text white (use ONLY when needed)
- Disabled: opacity 0.5 and no shadow
- Press state: transform scale(0.98), background darken ~6–8%
- Focus ring: 2px ring using var(--bq-accent) at low opacity

Inputs:
- 48px height
- white background
- border var(--bq-border)
- focus border var(--bq-primary)
- helper text uses var(--bq-text-muted)

Cards:
- background var(--bq-surface)
- border var(--bq-border)
- radius var(--bq-radius-lg)
- shadow var(--bq-shadow-sm)

Badges:
- neutral badge: subtle tint of secondary
- success badge: subtle tint of success
- warning badge: subtle tint of warning (do not use brass for warning)
- do NOT use bright yellows/oranges

================================================================================
4) UI KIT (REUSE EVERYWHERE)
================================================================================
Create /src/components/ui:
- Button.tsx
- IconButton.tsx
- Card.tsx
- TextField.tsx
- Toggle.tsx
- StickyFooter.tsx (for Next/Continue on mobile)

Make the core screens use these components:
DashboardScreen, ProjectSetupScreen, ScopeInputScreen, ComponentGroupsScreen, ReviewScreen, PreviewScreen.

================================================================================
5) FEATURE UI: UPLOAD BUILDER LOGO + PROJECT PHOTO
================================================================================
Builder logo:
- In Builder Profile screen: add “Upload business logo” with preview.
- Show logo on dashboard header if present.
- Store the file as a URL on the builder record (or a profile record). Implement upload via existing R2 flow if present; otherwise stub UI + store placeholder URL for now.

Project photo:
- In Project Setup screen: add “Upload project photo (optional)” (plans/site photo/stage photo)
- Show photo thumbnail on project cards.

Constraints:
- optional uploads
- png/jpg/webp
- max 2MB
- clean placeholder if missing (no loud icons)

================================================================================
6) CLERK THEMING (LOOKS LIKE BUILDQUOTE)
================================================================================
Theme Clerk sign-in/up using Clerk appearance + custom CSS so it inherits:
- tokens (colors, radii, font)
- buttons and inputs matching our UI kit
Do NOT leave default Clerk UI.

================================================================================
7) DELIVERABLES
================================================================================
Deliver a PR with:
- tokens.css, base.css, components.css
- UI kit components
- Clerk theming applied
- builder logo upload UI + project photo upload UI (even if upload backend is stubbed)
- minimal refactors so screens use the UI kit and variables (no hard-coded hex You are Claude Code working inside this repo: buildquote-v3. Implement the following 4 features with minimal refactor. Keep existing builder RFQ flow working. Add only what’s required, and prefer small, composable components in /src/components and new Convex fields/tables rather than large rewrites.

===============================================================================
FEATURE 1 — “Face recognition sign-in” (practical, web-safe implementation)
===============================================================================
Context: This is a web app (Vite/React + Clerk). “Face ID” is not directly available on the web like native iOS apps. Implement the correct equivalent: passkeys / WebAuthn via Clerk, surfaced in UI as “Use Face ID / Passkey” when supported.

Tasks:
1) Enable Clerk passkeys/WebAuthn (follow Clerk docs). If the repo already uses Clerk, add a “Use passkey” sign-in option.
2) On the Sign In screen:
   - Add a toggle/CTA reading: “Enable Face ID / Passkey (recommended)”
   - If device/browser supports it, show “Sign in with Face ID / Passkey”
   - If not supported, hide CTA or show disabled helper text: “Not supported on this device/browser”
3) Store user preference (enabled/disabled) per builder, so the UI remembers:
   - Add optional field on builders table:
     - authPreference: { passkeyEnabled?: boolean }
4) UX requirements:
   - Never block sign-in if passkey isn’t available.
   - Use plain language. No hype.

Deliverables:
- Updated sign-in UI (component + screen)
- Clerk passkey integration wired
- Builder preference stored in Convex and read on dashboard

===============================================================================
FEATURE 2 — Builder logo upload (stored in Cloudflare R2)
===============================================================================
Goal: Builders can upload a company logo that appears on:
- Builder Dashboard header
- RFQ email/PDF exports later (just store now; wiring later ok)

Data model changes (Convex):
- Add to builders table:
  - logoR2Key?: string
  - logoUrl?: string   (if you generate a public URL) OR store just key and use signed URL later
  - marketingOptIn?: boolean  (used in Feature 4)

Backend:
1) Create Convex mutation to request an upload URL for R2 (or use your existing R2 upload approach if already implemented):
   - builder requests upload slot -> returns pre-signed URL + r2Key
2) After upload, call another mutation to save logoR2Key (and logoUrl if applicable) onto builder record.
3) Validate:
   - image only (png/jpg/webp)
   - max size (e.g. 2MB)
   - square crop optional (nice-to-have)

Frontend:
- Add screen in onboarding OR builder settings:
  - “Upload business logo”
  - file picker + preview + save
- Show logo (if exists) on Dashboard screen.

===============================================================================
FEATURE 3 — Add photo/image to project card
===============================================================================
Goal: Each project can have an optional thumbnail image (site photo, plan snippet, etc.) shown on the project cards on dashboard.

Data model changes:
- Add to projects table:
  - imageR2Key?: string
  - imageUrl?: string

Backend:
- Same upload flow pattern as Feature 2:
  - getUploadUrl(projectId) -> upload -> saveProjectImage(projectId, imageR2Key, imageUrl?)

Frontend:
- On project create/edit screen (ProjectSetupScreen):
  - add “Project photo (optional)” uploader
- On dashboard project cards:
  - show thumbnail if present
  - fallback to initials/placeholder if not

===============================================================================
FEATURE 4 — Newsletter opt-in + sending capability
===============================================================================
Goal: Builders can opt-in to receive a newsletter. This is NOT transactional email; it’s marketing. Must be explicit opt-in and easy to change later.

Data model changes:
- Add to builders table:
  - marketingOptIn: v.boolean() (default false)
  - marketingOptInAt?: v.number()

Frontend:
- During sign up / first-run onboarding:
  - Checkbox: “Send me occasional BuildQuote updates and relevant industry news.”
  - Link: “Unsubscribe anytime.”
- In Settings:
  - toggle to change preference.

Backend:
1) Add mutation updateMarketingOptIn({ builderId, optIn })
   - If optIn=true, set marketingOptInAt=now
2) Create an admin-only function to export opted-in builder emails:
   - listMarketingSubscribers() -> [{email, name, companyName}]
   - Gate access by ADMIN_EMAILS env var.
3) Do NOT build a full newsletter engine in-app yet. Just the opt-in + export list.
4) Keep compliance basics:
   - opt-in only
   - store timestamp
   - allow opt-out

===============================================================================
IMPLEMENTATION NOTES
===============================================================================
- Keep code style consistent with existing repo.
- Add any new UI in:
  - /src/app (screens)
  - /src/components (reusable components)
- Add any Convex functions in:
  - /convex (new files ok)
- If you add R2 helpers, put them in /src/lib and /convex/lib as appropriate.
- Do not remove existing fields. Only extend.

===============================================================================
ACCEPTANCE CHECKLIST
===============================================================================
- Builder can enable passkey sign-in and sign in with it on supported devices.
- Builder can upload logo; dashboard shows it.
- Project can upload image; dashboard card shows it.
- Builder can opt-in/out of newsletter; admin can export subscribers list; default is opt-out.






















