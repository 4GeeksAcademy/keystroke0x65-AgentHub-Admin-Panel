# AgentHub Admin Panel — Product Specification

## Project overview

AgentHub is a SaaS platform where companies rent AI agents configured for business tasks. Agents can be equipped with reusable skills—capabilities such as web browsing, document reading, and calendar management. Client contracts specify the rented agent, included skills, dates, and itemized prices. The AgentHub admin is a platform operator who oversees business health, user accounts, agents, the skill catalog, contracts, and execution errors.

## Prototype scope and implementation constraints

- Build the prototype as **one `index.html` page** containing all six admin sections.
- Use semantic HTML, Tailwind CSS utility classes loaded via the CDN, and vanilla JavaScript only. Do not use frameworks, a backend, external CSS files, or inline `style` attributes.
- Use hardcoded sample data and client-side interactions; no real authentication, persistence service, or server requests are required.
- Configure Tailwind for class-based dark mode (`darkMode: 'class'`) before loading the Tailwind CDN. Use Tailwind `dark:` utilities for the light/dark appearance; JavaScript toggles the `dark` class on the document root and may use browser storage for the selected theme.
- The page must be usable on desktop and tablet widths. Keep tables readable with responsive overflow or an equivalent responsive layout.
- Sidebar navigation switches the visible section in the single page; it is not real page navigation.
- Keep sample agent names and ownership consistent wherever those agents appear, especially in Agent Management, Agent Contracts, and Error Log.

## Design direction: minimalist admin interface

- **Overall feel:** Create a focused, uncluttered admin experience that puts essential information first. Avoid decorative elements, unnecessary copy, and visual effects that do not improve hierarchy or usability.
- **Layout and whitespace:** Use a consistent grid, generous spacing between sections, and clear alignment. Keep content comfortably readable without crowding, while ensuring the six sections remain easy to scan.
- **Navigation:** Keep navigation simple and persistent. Use concise labels and a clear active state so admins can move between sections with minimal effort.
- **Typography:** Use one readable sans-serif typeface (or a restrained pairing at most). Establish a simple, consistent type scale for page titles, section headings, body text, and muted metadata.
- **Surfaces and shapes:** Favor flat, clean surfaces, restrained borders, and simple shapes. Use shadows sparingly; avoid gradients, glass effects, and excessive rounding.
- **Color:** Use a monochrome foundation of white, charcoal, and neutral grays in light mode, with corresponding dark neutrals in dark mode. Reserve restrained semantic color accents only where they carry meaning—such as agent health, status, or error severity—and pair these with text labels so meaning is never color-only.
- **Lightweight implementation:** Keep visual assets and interface effects lightweight. Use simple inline icons or text/icon-library-free symbols where appropriate; do not add unnecessary dependencies.

## Global shell and navigation

- A persistent sidebar contains the AgentHub name/mark and six navigation links: Dashboard, User Management, Agent Management, Skills, Agent Contracts, and Error Log. Indicate the current section with a clearly visible active state and `aria-current` where appropriate.
- A top bar displays the current section title and a dark/light mode control. Main content changes when a sidebar link is selected while the sidebar and top bar remain available.
- Use semantic landmarks, including `header`, `nav`, `main`, and a labeled `section` for each view. Only the selected view needs to be displayed, but all six views must be present in the document.
- Provide a consistent page title, spacing, typography, surfaces, and focus-visible states. Ensure interactive controls are keyboard reachable and have accessible names.

## View specifications

### 1. Dashboard

1. **Business metric cards:** Show exactly four prominent cards for total revenue this month, discount/coupon losses, active agents, and failing agents. Each card contains a recognizable icon, a descriptive label, and a hardcoded formatted value; use a responsive two-column grid at tablet/desktop widths and a single column on narrow screens.
2. **Metric visual hierarchy:** Use restrained neutral card surfaces and borders, with subtle tonal distinction rather than a rainbow of accents. Keep readable contrast in both themes; failing-agent information may use a restrained semantic accent but must remain visually noticeable without relying on color alone.
3. **Weekly activity chart placeholder:** Place a full-width chart placeholder below the cards, with a dashed border, a centered “Weekly activity” label, and supporting context that clearly identifies it as a static placeholder rather than a functioning chart.

### 2. User Management

1. **User table:** Display a semantic table with at least five hardcoded user records and columns for name, email, plan, status, and actions. Each row includes a status badge; use representative plan/status values and keep the table usable at tablet width through horizontal overflow if needed.
2. **Per-user action menu:** Every user row has a `⋮` button that opens “View detail” and “Delete”. Choosing “Delete” opens a confirmation dialog naming the selected user and offering “Cancel” and “Delete user”. Canceling or closing the dialog leaves the user unchanged. Confirming removes that user’s row from the visible table and shows a success message. Only one row menu may be open at a time; menus close when an option is selected or when the user clicks outside.
3. **User detail modal:** “View detail” opens an accessible overlay with the selected user’s full hardcoded record (including the table fields and any additional useful profile details). The modal closes using a visible close button or by clicking the backdrop, and clicking within the dialog does not close it.

### 3. Agent Management

1. **Agent listing:** Show at least four hardcoded agents with agent name, owner, active/inactive/failing status badge, a skill-list control, and a row-level `⋮` action button. The sample agent names and owners must match entries referenced in contracts and errors.
2. **Collapsible skills:** Each agent’s associated skills are hidden by default. Activating its expand control reveals the complete associated skill list; activating it again collapses the list. Animate the height/opacity change smoothly and expose the state through `aria-expanded` and a relationship to the skill-list element.
3. **Configure and delete actions:** Each agent dropdown contains “Configure” and “Delete”. “Configure” opens a modal showing that agent’s system prompt in an editable `<textarea>` and a close button. The textarea is an editable prototype field; no backend save is required. “Delete” must provide a clear confirmation or feedback interaction rather than silently doing nothing.

### 4. Skills

1. **Skills catalog:** Display at least four hardcoded skill entries/cards, each with a skill name, short description, and count of how many agents have it enabled. Keep counts consistent with the associated skills in Agent Management.
2. **In-panel explanation:** Include a concise explanatory callout defining a skill as a reusable capability that can be attached to an agent and included in a client contract.
3. **Per-skill actions:** Every skill has a `⋮` dropdown with “View detail” and “Delete”. “View detail” presents the selected skill’s name, description, and enabled-agent count in a modal or equivalent detail overlay. Menus close on outside click; deletion provides confirmation or visible feedback.

### 5. Agent Contracts

1. **Contract table:** Display at least four hardcoded active or past contracts in a semantic table. Each row shows client, rented agent, contracted skills, start date, end date, total amount paid, and a `⋮` action button.
2. **Contract detail breakdown:** Each row’s “View detail” action opens a modal for the selected contract. Include client and agent details, contract status and dates, an itemized list of contracted skills with an individual price for each, and a total that agrees with the itemized prices.
3. **Consistent and scannable presentation:** Use readable date/currency formatting, a status indicator where useful, and responsive table overflow at tablet widths. Agent references must use the same names as Agent Management and Error Log; contracted skills must correspond to the Skills catalog.
4. **Per-contract action menu:** Every contract row has a `⋮` menu, and every menu offers “View detail”. Active contracts also offer “Cancel contract”; choosing it opens a confirmation dialog, and confirming changes the visible contract status to “Cancelled” and shows feedback. Past contracts instead offer “Copy contract ID”; each row displays its contract ID, and choosing the action copies that ID when clipboard access is available and shows feedback. Menus close after an action or an outside click.

### 6. Error Log

1. **Error entries:** Show at least six hardcoded agent execution errors. Each entry includes timestamp, agent name, error type/severity, and a concise description. At least one agent name must also appear in Agent Management and Agent Contracts.
2. **Severity badges:** Categorize error types with distinct color-coded badges (for example, critical, warning, and informational), with text labels so meaning remains clear without color. Resolved items should receive a visible resolved state.
3. **Per-error actions:** Every error entry has a `⋮` dropdown with “View detail” and “Mark as resolved”. “View detail” opens a modal with the full trace and relevant metadata. “Mark as resolved” updates that entry’s visible state and provides confirmation/feedback; the menu closes after either action.

## Shared component inventory

- **Persistent sidebar:** Brand, six section links, current-section indicator, and accessible navigation state.
- **Top bar:** Current view title and shared theme toggle.
- **Metric card:** Icon, label, formatted value, and optional metric-specific accent.
- **Data table/list row:** Consistent alignment for record fields and a reserved action-menu column.
- **Action dropdown:** `⋮` trigger and context-specific menu actions; supports outside-click dismissal, Escape dismissal, and one open menu at a time.
- **Modal/dialog:** Backdrop, labeled dialog content, close button, and backdrop-click dismissal; content varies for user, agent configuration, skill, contract, and error details.
- **Status/severity badge:** Compact text-labeled state indicator with theme-safe color treatment.
- **Collapsible skill list:** Agent skill summary/control and animated expanded content with accessible expanded state.
- **Dark mode toggle:** Top-bar control that switches the document-wide theme and preserves the selection while navigating among sections (and across reloads when browser storage is available).
- **Confirmation/feedback message:** Reusable confirmation or result feedback for destructive actions and error resolution.

## Acceptance criteria

1. The prototype is implemented in a single `index.html` using semantic HTML, Tailwind utilities via CDN, and vanilla JavaScript; it has no framework, backend, external CSS file, or inline `style` attributes.
2. The persistent sidebar provides working navigation to all six sections, and the active link updates to match the visible section.
3. The Dashboard shows four labeled, icon-bearing hardcoded metric cards and a full-width weekly activity placeholder beneath them.
4. User Management contains at least five records with name, email, plan, status badge, and a `⋮` menu per row; its menu provides “View detail” and “Delete”. “Delete” requires confirmation; canceling or closing the dialog makes no change, while confirming removes only the selected user from the visible table and shows feedback.
5. Agent Management contains at least four agents with owner, valid status, and skills collapsed by default; activating each skill control expands and collapses its list with a visible smooth transition and accurate accessible state.
6. Each agent’s dropdown provides “Configure” and “Delete”; “Configure” opens a modal with that agent’s system prompt in an editable textarea.
7. Skills contains at least four catalog entries with name, description, and enabled-agent count, plus an explanation of skills in AgentHub; each entry has “View detail” and “Delete” actions.
8. Agent Contracts contains at least four contracts with client, agent, skills, start/end dates, amount paid, and a displayed contract ID; each “View detail” action displays itemized skill prices and a matching total. Active contract menus also provide “Cancel contract”, which requires confirmation and updates the visible status when confirmed; past contract menus provide “Copy contract ID” and show feedback after the copy attempt.
9. Error Log contains at least six entries with timestamps, agent names, descriptions, and text-labeled, color-coded error badges; “View detail” shows the full trace and “Mark as resolved” visibly updates the selected error.
10. Every row/item action dropdown opens from its `⋮` button, exposes the specified actions, closes when clicking outside, closes when an action is selected, and does not leave multiple menus open simultaneously.
11. Detail/configuration modals work in at least four sections (User Management, Agent Management, Skills, Agent Contracts, and Error Log are expected); each modal closes from its close button and backdrop click, while clicks inside the dialog do not dismiss it.
12. Tailwind is configured for class-based dark mode (`darkMode: 'class'`) before its CDN is loaded. The dark/light toggle adds or removes the document root’s `dark` class, changes the whole interface using Tailwind `dark:` utilities, and keeps the chosen mode while navigating among sections; persist the selection across reloads where browser storage is available.
13. Hardcoded agent names and skill names/counts are consistent across Agent Management, Skills, Agent Contracts, and Error Log.
14. Layout remains usable on desktop and tablet viewports; tables do not make the whole page unusable when their content exceeds the available width.
15. Semantic landmarks and controls are used appropriately, including `header`, `nav`, `main`, `section`, and `table`; interactive controls are keyboard reachable and have accessible names and visible focus states.
