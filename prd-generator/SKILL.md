---
name: prd-generator
description: Turn a rough idea, description, or concept into a structured product requirements document (PRD). Accepts any input — a sentence, bullet points, a ramble, a competitor reference — and produces a PRD ready for the presearch skill. Use when the user has an idea but no formal PRD, wants to create a requirements doc, needs to formalize product requirements, or says things like "I have an idea for..." or "I want to build...".
---

# PRD Generator — Idea to Requirements

Turn a rough idea into a structured, complete product requirements document through guided conversation.

## Trigger

The user describes a product idea in any form — a sentence, a paragraph, bullet points, a competitor they want to improve on, a problem they've noticed — and doesn't have a formal PRD.

## Workflow

### Phase 1: Capture the Idea

1. **Accept whatever the user has.** Could be:
   - "I want to build a budgeting app"
   - A page of bullet points
   - "Something like Notion but for recipes"
   - "I keep forgetting to water my plants, there should be an app for that"
   - A link to a competitor's product
2. **Reflect it back.** Summarize the core concept in 2-3 sentences. Don't add anything yet — just prove you understood the idea.
3. **Confirm** with the user before moving on.

### Phase 2: Research the Landscape

Use WebSearch to understand the space:

1. **Find existing solutions** — what products already solve this problem? How do they approach it?
2. **Identify gaps** — what do existing solutions do poorly or not at all? Where's the opportunity?
3. **Note common features** — what do most products in this space include? This informs the feature suggestions in Phase 4.
4. **Check market context** — is this a growing space? Any recent trends or shifts?

Present a brief competitive landscape summary:
- 3-5 existing products with their approach and key weakness
- The opportunity gap this product could fill
- Common table-stakes features in the space

The user can react, adjust the positioning, or skip this phase if they already know the landscape.

### Phase 3: Shape the Vision

Ask structured questions to flesh out the product. Batch into groups of 3-4, not a wall of questions.

**Round 1: The problem**
- What specific problem does this solve?
- Who experiences this problem? (be specific — not "everyone")
- How do they currently deal with it?

**Round 2: The product**
- What does this product do that existing solutions don't?
- What's the one thing it must do exceptionally well?
- What platforms should it run on? (web, mobile, desktop, API, CLI)

**Round 3: Success and scope**
- How would you measure success? (users, revenue, engagement, time saved)
- What's the initial scope — MVP for launch, or full vision?
- Any hard constraints? (budget, timeline, team size, tech requirements, compliance)

Use the AskQuestion tool when available. Adapt based on how much the user already provided — skip questions they've already answered.

### Phase 4: Define the Users

1. **Identify user types** — primary users, secondary users, admins, etc.
2. **For each user type, define:**
   - Who they are (role, context)
   - What they're trying to accomplish
   - Key pain points
   - How they'd discover and start using the product
3. **Present as a user summary table** and confirm.

### Phase 5: Map the Features

1. **Propose a feature set** based on everything gathered so far. Group by domain (e.g., Auth, Core, Data, UI, Integrations, Admin). For each feature:
   - Name and brief description
   - Priority (Must-have / Should-have / Could-have / Won't-have)
   - Which user type it serves

2. **Proactively suggest features the user didn't mention** based on:
   - Common features from the competitive research (Phase 2)
   - Implied needs from the user stories (Phase 4)
   - Standard requirements for the product type (e.g., every SaaS needs auth, settings, billing)
   - Mark these as "Suggested" so the user knows they were agent-recommended

3. **Present the full feature list** as a table and ask the user to:
   - Approve, reprioritize, or remove features
   - Expand on any feature that needs more detail
   - Add features that were missed

4. **For any feature the user wants to discuss**, dive deeper:
   - Present 3-4 implementation approaches with pros/cons
   - State a recommendation
   - Let the user decide
   - Record the decision

5. **Iterate** until the user is satisfied with the feature set.

### Phase 6: Non-Functional Requirements

Walk through non-functional requirements relevant to the product type:

| Category | Questions to ask |
|----------|-----------------|
| **Performance** | Expected response times? Concurrent users? Data volume? |
| **Security** | Sensitive data? Auth requirements? Encryption needs? Compliance (GDPR, HIPAA, SOC2)? |
| **Scalability** | Growth expectations? Peak traffic patterns? |
| **Reliability** | Uptime requirements? Disaster recovery needs? |
| **Accessibility** | WCAG compliance level? Internationalization? |
| **Platform** | Browser support? Mobile responsiveness? Offline capability? |

Skip categories that don't apply. Don't force enterprise concerns on a weekend side project.

### Phase 7: Produce the PRD

Generate the PRD using the template in [prd-template.md](prd-template.md).

1. **Fill every section** with specifics from the conversation — no placeholders, no "TBD" unless the user explicitly deferred something.
2. **Use structured tables** for requirements (designed to be parsed by the presearch skill).
3. **Include the competitive landscape** summary from Phase 2.
4. **Save as `PRD.md`** in the project root (or ask the user where they'd like it).
5. **Present a summary** of what was created:
   - Total feature count by priority
   - User types defined
   - Key decisions made
   - Suggested next step: "Run presearch on this PRD to create a project plan"

## Interaction Guidelines

- **Start light, go deeper as needed.** Don't front-load 20 questions. Capture the idea, reflect it, then dig in incrementally.
- **Suggest, don't just ask.** The user said they want proactive feature suggestions. When you see a gap, say "apps like this usually include X — want that?" Don't wait for them to think of everything.
- **Propose, then react.** Present a complete feature set for reaction rather than building it feature by feature. It's faster and gives the user the full picture to respond to.
- **Use the research.** The competitive landscape should actively inform your suggestions — "Competitor X does this, but based on what you've said, you might want to approach it differently because..."
- **Match the user's energy.** If they gave you a detailed brief, skip the basics. If they gave you one sentence, spend more time in the vision and user phases.
- **Structured output matters.** The PRD feeds into presearch. Use the exact table format from the template so presearch can parse it directly.

## Additional Resources

- For the PRD output template, see [prd-template.md](prd-template.md)
