---
name: ux-advisor
description: Solve UX/UI problems by first gathering context (platform, theme, existing experience), then researching how major software providers (Google Material Design, Apple HIG, Microsoft Fluent, Shopify Polaris, Atlassian, etc.) handle similar patterns, presenting multiple visual solutions as an interactive HTML page for the user to choose from, and finally creating a full implementation plan. Use this skill whenever the user describes a UX challenge, asks how to design a UI component, needs UI pattern inspiration, wants to improve usability or accessibility of a feature, asks about best practices for a specific interaction pattern, or mentions struggling with a design decision. Even if they don't say "UX" explicitly — if they're asking about dropdowns, modals, navigation, forms, tables, onboarding flows, search, filters, empty states, error handling UI, or any user-facing interaction pattern, this skill applies.
argument-hint: [describe your UX problem]
allowed-tools: WebSearch, WebFetch, Read, Grep, Glob, Write, Bash
---

The user has a UX/UI problem they need solved. You will guide them through a structured process: gather context, research solutions, present options visually, and then build an implementation plan for their chosen solution.

## The UX problem

$ARGUMENTS

## Phase 1: Discovery interview

Before doing any research, you need full context. Ask the user clarifying questions to understand the problem deeply. Don't skip this — the quality of your solutions depends entirely on how well you understand the constraints.

Ask about:

- **Platform & device targets** — Web, mobile (iOS/Android), desktop app? Which screen sizes matter most? Is it responsive or a fixed layout?
- **Tech stack** — What framework, component library, and styling approach are they using? (Check the codebase too — look at package.json, imports, etc.)
- **Design theme & brand** — Dark mode, light mode, or both? Any existing design tokens, brand colors, or style guidelines? Is there a Figma file or design system they follow?
- **Current experience** — What does the current implementation look like? Is there a URL, screenshot, or file they can point to? What specifically isn't working about it?
- **Users** — Who are the end users? Technical or non-technical? How frequently do they use this feature? Any known accessibility requirements?
- **Scale & data** — How much data/content does this need to handle? What are the edge cases (empty state, thousands of items, very long text)?
- **Constraints** — Any hard requirements? (e.g., must work offline, must support RTL, must load in under 2 seconds, must match an existing pattern in the app)

You don't need to ask every question — use judgment based on the problem. If some answers are obvious from context or the codebase, state your assumptions instead of asking. The goal is to have enough context to propose solutions that actually fit, without exhausting the user with questions.

Also check the codebase yourself in parallel:
- Read `package.json` or equivalent to identify the tech stack
- Look at existing components to understand patterns and conventions
- Check for existing design tokens, theme files, or style configurations
- Look at the current implementation of the feature if it exists

Wait for the user to respond before proceeding to Phase 2.

## Phase 2: Research

Now that you have context, search the web for how established design systems and real products handle this pattern. The user's specific constraints (platform, scale, user type) should guide which sources are most relevant.

**Design systems** — these document their reasoning, not just their components:
- Google Material Design (m3.material.io)
- Apple Human Interface Guidelines (developer.apple.com/design)
- Microsoft Fluent UI (fluent2.microsoft.design)
- Atlassian Design System (atlassian.design)
- Shopify Polaris (polaris.shopify.com)
- IBM Carbon (carbondesignsystem.com)
- Adobe Spectrum (spectrum.adobe.com)

**Real products** — see how the pattern works in production:
- Gmail, Google Docs, YouTube (Google's ecosystem)
- Slack, Notion, Linear, Figma (modern SaaS)
- VS Code, GitHub (developer tools)
- Airbnb, Stripe, Shopify (e-commerce/fintech)

**Research articles** — understand the cognitive science behind the pattern:
- Nielsen Norman Group (nngroup.com)
- Baymard Institute (baymard.com)
- Smashing Magazine, A List Apart

Search for at least 3 different sources. Use parallel web searches to speed this up. Fetch the actual pages to get details, not just search snippets. The most useful thing to find is *why* a provider made specific design choices, not just *what* they built.

## Phase 3: Present 3+ solutions

Based on your research, design at least 3 distinct solution approaches. Each solution should represent a genuinely different UX strategy, not just cosmetic variations. For example:
- One might prioritize simplicity and progressive disclosure
- Another might optimize for power users and information density
- A third might focus on mobile-first or accessibility-first design

For each solution, document:
- **Name** — A short descriptive name (e.g., "Progressive Disclosure", "Power User Dashboard", "Mobile-First Cards")
- **Inspired by** — Which providers/products inspired this approach
- **How it works** — The interaction flow described clearly
- **Tradeoffs** — What this approach is great at and where it compromises
- **Accessibility** — How it handles keyboard nav, screen readers, WCAG 2.1 AA compliance
- **Best for** — What kind of user/context this solution fits best

### Generate the visual HTML preview

Create an interactive HTML file that lets the user see and compare all solutions visually. Write this file to the project directory (e.g., `ux-solutions-preview.html`) and open it in the browser.

The HTML file should:
- Be a single self-contained file (inline CSS and JS, no external dependencies)
- Show each solution as a visual mockup/wireframe using HTML and CSS — not just text descriptions
- Make the mockups interactive where possible (clickable tabs, expandable sections, hover states) so the user can feel the interaction pattern
- Include a clear heading and description for each solution
- Use a clean, professional layout with good typography
- Include a comparison summary section at the top highlighting the key tradeoffs
- Adapt the mockup visuals to match the user's stated theme (dark/light mode, brand colors if provided)
- At the bottom, have a clear call-to-action: "Which solution do you prefer? Tell Claude your choice (e.g., 'Solution 1' or 'I like the progressive disclosure approach but with the filter panel from Solution 3')"

```bash
open ux-solutions-preview.html
```

Tell the user you've opened the solutions preview in their browser and ask them to review and pick their preferred approach. They can also mix and match elements from different solutions.

Wait for the user to choose before proceeding to Phase 4.

## Phase 4: Implementation plan

Once the user picks a solution (or a combination), create a detailed implementation plan. This plan should be specific to their codebase and tech stack — not generic advice.

### The implementation plan should include:

**1. Overview**
- Summary of the chosen approach
- Expected outcome and how it solves the original problem

**2. Component architecture**
- What components need to be created or modified
- Component hierarchy and data flow
- Props/state/events for each component
- Which existing components can be reused

**3. Step-by-step implementation**
Break the work into ordered, discrete steps. Each step should be:
- Small enough to complete and test independently
- Clear about what files to create/modify
- Specific about what code to write (include code snippets for non-obvious parts)

**4. Accessibility checklist**
- ARIA attributes needed
- Keyboard navigation plan (tab order, arrow keys, escape to close, etc.)
- Screen reader announcements for dynamic content
- Focus management (where does focus go after actions?)
- Color contrast and motion considerations

**5. Edge cases to handle**
- Empty states
- Error states
- Loading states
- Content overflow / very long text
- Extreme data volumes
- Slow network / offline behavior (if relevant)

**6. Testing recommendations**
- Key interactions to test manually
- Accessibility testing approach (axe, VoiceOver/NVDA, keyboard-only)
- Responsive breakpoints to verify

After presenting the plan, ask if the user wants you to start implementing it. If yes, proceed step by step, following the plan.

## References

Always include links to the design systems, articles, and examples you drew from throughout the process.
