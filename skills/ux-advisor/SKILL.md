---
name: ux-advisor
description: Solve UX/UI problems by first gathering context (platform, theme, existing experience), then researching how major software providers (Google Material Design, Apple HIG, Microsoft Fluent, Shopify Polaris, Atlassian, etc.) handle similar patterns, presenting multiple visual solutions as an interactive HTML page for the user to choose from, and finally creating a full implementation plan. Use this skill whenever the user describes a UX challenge, asks how to design a UI component, needs UI pattern inspiration, wants to improve usability or accessibility of a feature, asks about best practices for a specific interaction pattern, or mentions struggling with a design decision. Even if they don't say "UX" explicitly — if they're asking about dropdowns, modals, navigation, forms, tables, onboarding flows, search, filters, empty states, error handling UI, or any user-facing interaction pattern, this skill applies.
argument-hint: [describe your UX problem]
allowed-tools: WebSearch, WebFetch, Read, Grep, Glob, Write, Bash, Agent
---

The user has a UX/UI problem they need solved. You are the orchestrator — you manage the conversation with the user and delegate heavy lifting to specialized subagents. This keeps things fast and thorough.

## The UX problem

$ARGUMENTS

---

## Phase 1: Discovery

This phase runs in the main conversation — you need to interact with the user directly.

### 1a: Scan the codebase (subagent)

While you prepare your questions, spawn an Explore subagent in the background to scan the codebase for context:

```
Subagent prompt: "Analyze this codebase for UX-relevant context. Find and report:
1. Frontend framework (check package.json, imports, build config)
2. Component library in use (MUI, Radix, shadcn, Ant Design, Chakra, etc.)
3. Styling approach (CSS modules, Tailwind, styled-components, SCSS, etc.)
4. Existing design tokens, theme files, or style configurations
5. Current implementation of [the feature in question] if it exists
6. Any existing UI patterns or component conventions used in the project
Report all findings in a structured summary."
```

### 1b: Interview the user

While the codebase scan runs in the background, ask the user clarifying questions. Tailor your questions to the problem — don't ask every question, just the ones that matter:

- **Platform & device targets** — Web, mobile (iOS/Android), desktop? Responsive or fixed?
- **Design theme & brand** — Dark/light mode? Brand colors? Existing design system or Figma file?
- **Current experience** — URL, screenshot, or file showing what exists now? What's not working?
- **Users** — Technical or non-technical? Frequency of use? Accessibility requirements?
- **Scale & data** — How much data? Edge cases (empty, thousands of items, long text)?
- **Constraints** — Offline support, RTL, performance budget, must match existing patterns?

When the codebase scan completes, incorporate its findings. Confirm assumptions with the user (e.g., "I can see you're using React + Tailwind with shadcn/ui — is that correct?").

**Wait for the user to respond before proceeding to Phase 2.**

---

## Phase 2: Research (parallel subagents)

Once you have full context, launch 3 research subagents in parallel. Each one focuses on a different category of sources. Send all 3 Agent tool calls in a single message so they run concurrently.

### Subagent A: Design Systems Research

```
Subagent prompt: "You are a UX researcher. Research how major design systems handle this UX pattern:

[Describe the UX problem and user context here]

Search these design systems and fetch the actual documentation pages (not just search snippets):
- Google Material Design (m3.material.io)
- Apple Human Interface Guidelines (developer.apple.com/design)
- Microsoft Fluent UI (fluent2.microsoft.design)
- Atlassian Design System (atlassian.design)

For each system that addresses this pattern, report:
1. Their recommended approach and component(s)
2. The REASONING behind their design decisions (this is the most valuable part)
3. Interaction details (keyboard support, animations, responsive behavior)
4. Accessibility guidance they provide
5. Direct links to the relevant documentation pages

Focus on quality over quantity — deep analysis of 2-3 systems is better than shallow coverage of all."
```

### Subagent B: Product & Industry Research

```
Subagent prompt: "You are a UX researcher. Research how real-world products solve this UX pattern:

[Describe the UX problem and user context here]

Search for how these products handle it:
- Google products (Gmail, Docs, YouTube)
- Modern SaaS (Slack, Notion, Linear, Figma)
- Developer tools (VS Code, GitHub)
- E-commerce/Fintech (Airbnb, Stripe, Shopify)

Also search for:
- Shopify Polaris (polaris.shopify.com) guidance on this pattern
- IBM Carbon (carbondesignsystem.com) guidance on this pattern
- Adobe Spectrum (spectrum.adobe.com) guidance on this pattern

For each relevant product or system, report:
1. How they implement this pattern
2. What makes their approach effective (or not)
3. Clever interaction details worth borrowing
4. Screenshots or descriptions of the UI if available
5. Links to any public documentation or articles about their approach"
```

### Subagent C: UX Research & Accessibility

```
Subagent prompt: "You are a UX researcher specializing in usability research and accessibility. Research the academic and practitioner knowledge about this UX pattern:

[Describe the UX problem and user context here]

Search these sources:
- Nielsen Norman Group (nngroup.com)
- Baymard Institute (baymard.com)
- Smashing Magazine
- A List Apart
- W3C WAI (w3.org/WAI) for accessibility patterns
- ARIA Authoring Practices Guide (w3.org/WAI/ARIA/apg)

Report:
1. Research-backed best practices for this pattern
2. Common usability mistakes to avoid (and why they fail)
3. Accessibility requirements — WCAG 2.1 AA criteria that apply
4. Required ARIA roles, states, and properties
5. Keyboard interaction model (what keys do what)
6. Screen reader behavior expectations
7. Links to all sources"
```

### Synthesize research

Once all 3 subagents complete, synthesize their findings into a unified research summary. Look for:
- **Convergence** — where multiple sources agree (strong signal)
- **Divergence** — where sources disagree (reveals context-dependent tradeoffs)
- **Gaps** — what none of them address well (opportunities for your solution)

---

## Phase 3: Solution Design (review gate)

This phase has two steps with a user review in between.

### 3a: Generate solution proposals (subagent)

Spawn a subagent to design 3+ distinct solutions based on the research:

```
Subagent prompt: "You are a senior UX designer. Based on the following research findings, design 3 or more distinct solution approaches for this UX problem.

UX Problem: [describe it]
User Context: [platform, tech stack, users, constraints]
Research Findings: [paste the synthesized research from Phase 2]

Each solution must represent a genuinely different UX STRATEGY — not cosmetic variations. For example:
- One might prioritize simplicity and progressive disclosure
- Another might optimize for power users and information density
- A third might be mobile-first or accessibility-first

For EACH solution, provide:
1. **Name** — short descriptive name (e.g., 'Progressive Disclosure', 'Power User Grid')
2. **Inspired by** — which providers/products inspired this approach and what you borrowed
3. **Interaction flow** — step-by-step what the user experiences
4. **Visual description** — describe the layout, key UI elements, and spatial relationships clearly enough that someone could sketch it
5. **Tradeoffs** — what this approach excels at and where it compromises
6. **Accessibility** — keyboard navigation model, ARIA roles, screen reader behavior, focus management
7. **Edge cases** — how it handles empty state, error state, loading, overflow, extreme data volumes
8. **Best for** — what type of user/context this fits best

Make each solution concrete and detailed enough to be turned into an interactive HTML mockup."
```

### 3b: Present solutions for review (user checkpoint)

Present the solution proposals to the user as a text summary — a concise comparison table followed by a brief description of each solution. Include:
- A comparison table: rows = solutions, columns = key tradeoffs (simplicity, power, mobile, accessibility, etc.)
- A 2-3 sentence summary of each solution's core idea

Ask the user: **"Do these directions look right? Should I adjust any of them before I build the visual previews, or add a different approach?"**

This review gate saves time — it's cheaper to adjust the strategy now than to regenerate the full HTML visualization. The user might say "drop Solution 2, and make Solution 1 more focused on mobile" or "these are great, go ahead."

**Wait for the user to confirm before proceeding to Phase 4.**

---

## Phase 4: Visual Preview (subagent)

Once the user approves the solution directions, spawn a subagent to build the interactive HTML preview:

```
Subagent prompt: "You are a frontend developer specializing in interactive prototypes. Create a single self-contained HTML file that visualizes these UX solutions for comparison.

Solutions to visualize:
[paste the approved solution proposals with full detail]

User's theme preferences: [dark/light mode, brand colors if provided]

Requirements for the HTML file:
- Single self-contained file — all CSS and JS inline, no external dependencies
- Show each solution as an INTERACTIVE visual mockup using HTML and CSS — not just text descriptions
- Make mockups interactive where possible: clickable tabs, expandable sections, hover states, transitions — the user should be able to FEEL the interaction pattern
- Include a comparison summary section at the top with key tradeoffs
- Use clean, professional layout with good typography
- Adapt the mockup visuals to match the user's stated theme (dark/light mode, brand colors)
- At the bottom, include a clear call-to-action: 'Which solution do you prefer? Tell Claude your choice (e.g., Solution 1 or I like the progressive disclosure approach but with the filter panel from Solution 3)'
- Make the page responsive so it works well when the browser window is resized
- Use a tabbed or card-based layout so each solution gets full focus when viewing it

Write the complete HTML to: ux-advisor-preview.html"
```

After the subagent completes, open the file in the browser:

```bash
open ux-advisor-preview.html
```

Tell the user you've opened the preview and ask them to pick their preferred approach. They can also mix and match elements from different solutions.

**Wait for the user to choose before proceeding to Phase 5.**

---

## Phase 5: Implementation Plan (subagent)

Once the user picks a solution (or a combination), spawn a subagent to create the implementation plan. This subagent should also have access to the codebase so it can make specific, actionable recommendations:

```
Subagent prompt: "You are a senior frontend engineer creating an implementation plan. The user has chosen a UX solution and you need to create a detailed, step-by-step plan for building it in their codebase.

Chosen solution: [describe the chosen solution, including any mix-and-match from other solutions]
Tech stack: [framework, component library, styling approach]
Codebase conventions: [what the codebase scan found — patterns, naming, file structure]

Explore the codebase to understand existing patterns, then create a plan with these sections:

**1. Overview**
- Summary of what will be built
- How it solves the original problem

**2. Component architecture**
- Components to create or modify (with file paths)
- Component hierarchy and data flow diagram (ASCII)
- Props/state/events for each component
- Existing components that can be reused

**3. Step-by-step implementation**
Break into ordered, discrete steps. Each step should:
- Be small enough to complete and test independently
- Name the exact files to create or modify
- Include code snippets for non-obvious parts
- Note dependencies on previous steps

**4. Accessibility checklist**
- ARIA attributes needed (specific roles, states, properties)
- Keyboard navigation plan (tab order, arrow keys, escape, enter)
- Screen reader announcements for dynamic content changes
- Focus management (where focus goes after open/close/delete/etc.)
- Color contrast requirements and motion preferences (prefers-reduced-motion)

**5. Edge cases**
- Empty states (what to show, how to guide the user)
- Error states (validation, network errors, permission errors)
- Loading states (skeletons, spinners, optimistic updates)
- Content overflow (truncation strategy, very long text, many items)
- Extreme data volumes (virtualization, pagination, lazy loading)
- Slow network / offline (if relevant)

**6. Testing recommendations**
- Key interactions to verify manually
- Accessibility testing (axe-core, VoiceOver/NVDA walkthrough, keyboard-only test)
- Responsive breakpoints to check
- Browser compatibility notes"
```

Present the plan to the user and ask if they want you to start implementing it. If yes, proceed step by step, following the plan.

---

## References

Throughout the process, collect all links to design systems, research articles, and product examples referenced by the research subagents. Include a consolidated references section in your final output.
