# /init — Tempo WFM Dashboard Project

## Project name

Tempo WFM

## Project purpose

This project is a prototype Workforce Management platform for a multi-department operational environment. The current baseline is a polished standalone HTML dashboard demo that should be treated as the visual and product reference for the next stage of development.

The goal is to evolve the static HTML prototype into a maintainable, production-style web application with realistic architecture, reusable components, simulated or API-driven data, role-based access control, and clear separation between frontend, backend, and data layers.

## Baseline file

Use the provided HTML dashboard as the starting visual reference:

`tempo-wfm-full-multipage-v2.html`

This file contains the current demo product and should be preserved as the design source of truth unless explicitly instructed otherwise.

## Current product concept

Tempo WFM is a Workforce Management dashboard used by approximately 200 users across four departments:

1. Intraday
2. Scheduling
3. Capacity Planning
4. MI & Reporting

The application supports different user scopes:

- Team member: access to their own department only
- Manager: access to their own department
- Senior manager: access to two departments
- Head of WFM: global access across all departments
- Admin: user, team, data, settings, and audit management access

The product should feel like a credible commercial WFM platform, similar in maturity to tools such as NICE IEX, Verint, Calabrio, Genesys WEM, or injixo, while retaining the custom Tempo visual style.

## Current pages in the demo

The existing dashboard includes:

- Overview
- Real-Time Operations
- Intraday
- Scheduling
- Forecasting
- Capacity Planning
- People & Performance
- Reporting
- Alerts
- User Management
- Teams & Access
- Data Management
- System Settings
- Audit Logs

Each page should remain part of the product unless a future instruction removes it.

## Design direction

Maintain the current look and feel from the supplied HTML demo:

- Dark left navigation
- White content cards
- Light grey application background
- Teal primary accent
- Green, amber, red status colours
- Clean enterprise dashboard style
- KPI cards at the top of key pages
- Contained charts that do not overflow their card areas
- Tables with clear headers and consistent spacing
- Operationally realistic WFM language
- Responsive behaviour where practical

Avoid turning the product into a generic template. It should continue to look like the current Tempo dashboard.

## Important recent fixes to preserve

The latest version includes specific fixes that must be preserved:

1. The Overview page Real-time interval ribbon has been resized so it fits within its card.
2. The Intraday page volume chart is a line graph.
3. Intraday intervals are now 15-minute intervals, not 30-minute intervals.
4. Charts should not bleed into other cards or sections.
5. Chart legends and axis labels should remain contained within each card.
6. The dashboard includes a realistic executive narrative block on the Overview page.
7. Forecasting confidence intervals should be represented clearly.
8. Capacity Planning should show Required FTE, Scheduled FTE, and Mitigated Supply together.
9. Alerts should be clickable and should open a detail side panel.
10. Tables should retain readable headers and clean spacing.

## Suggested technical direction

When converting this prototype into a real application, prefer the following architecture:

### Frontend

Use:

- Next.js
- React
- TypeScript
- Tailwind CSS or CSS modules
- Recharts, ECharts, or another suitable charting library
- Component-based layout

Suggested structure:

```txt
app/
  layout.tsx
  page.tsx
  realtime/page.tsx
  intraday/page.tsx
  scheduling/page.tsx
  forecasting/page.tsx
  capacity/page.tsx
  people/page.tsx
  reporting/page.tsx
  alerts/page.tsx
  admin/users/page.tsx
  admin/teams/page.tsx
  admin/data/page.tsx
  admin/settings/page.tsx
  admin/audit/page.tsx

components/
  layout/
    Sidebar.tsx
    Topbar.tsx
    PageHeader.tsx
  cards/
    KpiCard.tsx
    ChartCard.tsx
    NarrativeCard.tsx
  charts/
    LineChart.tsx
    BarChart.tsx
    Heatmap.tsx
    GaugeChart.tsx
    IntervalRibbon.tsx
  tables/
    DataTable.tsx
  alerts/
    AlertDrawer.tsx
  filters/
    DateRangeFilter.tsx
    DepartmentFilter.tsx

lib/
  mockData.ts
  rbac.ts
  formatters.ts
  chartConfig.ts
```

### Backend

If adding a backend, use:

- FastAPI
- Python
- Pydantic models
- REST endpoints
- Optional future Snowflake integration
- Optional Postgres integration for transactional CRUD

Suggested backend responsibilities:

- Serve metrics
- Serve chart data
- Serve role-filtered department data
- Handle alert acknowledgement
- Handle schedule and capacity scenario actions
- Validate role and department access server-side

### Authentication and access model

Implement role-based access control using a user-to-department access model.

Suggested model:

```txt
users
  id
  name
  email
  role
  is_global_admin

departments
  id
  name

user_department_access
  user_id
  department_id
```

Rules:

- Team members see their own department only.
- Managers see their own department only.
- Senior managers see their assigned departments.
- Head of WFM sees all departments.
- Admin users can manage access and settings.
- Never rely only on frontend hiding. Backend endpoints must also enforce access.

## Data approach

For now, use realistic mock data.

Mock data should include:

- Departments
- Queues
- Employees
- Shifts
- Forecast intervals
- Actual volumes
- Service levels
- Occupancy
- AHT
- Shrinkage
- Schedule adherence
- Capacity requirements
- Hiring pipeline
- Attrition
- Reports
- Alerts
- Audit events
- User access records

Keep the mock data structured so it can later be replaced by API responses.

## WFM domain expectations

The product should reflect real workforce management workflows.

Examples:

### Intraday

- 15-minute interval data
- Forecast vs actual volume
- Reforecast line
- Interval variance
- SLA risk
- Occupancy
- Shrinkage
- Queue-level performance
- Runbook actions

### Scheduling

- Schedule adherence
- Open shifts
- Swap requests
- Coverage gaps
- Coverage heatmaps
- Break optimisation
- Shift marketplace

### Forecasting

- ML forecast
- Baseline forecast
- Actual volume
- Confidence intervals
- MAPE
- Bias
- Model drift
- Campaign and holiday drivers
- Forecast publish history

### Capacity Planning

- Required FTE
- Scheduled FTE
- Mitigated supply
- FTE gaps
- Hiring pipeline
- Attrition risk
- Scenario modelling

### Reporting

- Report catalogue
- Report execution history
- Data quality
- Data refresh status
- Pipeline monitoring
- Dashboard usage

### Alerts

- Alert inbox
- Severity levels
- Alert detail drawer
- Acknowledgement workflow
- Create action workflow
- Escalation status

### Admin

- Users
- Roles
- Department access
- Teams
- Data sources
- Feature flags
- Audit logs
- System settings

## UX expectations

Prioritise:

- Clarity
- Dense but readable dashboard layouts
- No chart overflow
- No clipped text
- Consistent card heights where useful
- Meaningful mock data
- Clickable interactions where helpful
- Side drawers for details
- Filters at page level
- Status colours used consistently
- Accessible colour contrast
- Responsive behaviour for laptop-sized screens first

## Do not do

Do not:

- Remove existing pages without instruction.
- Replace the design with a generic dashboard theme.
- Use placeholder lorem ipsum.
- Leave large blank sections.
- Let charts overflow cards.
- Use 30-minute intervals on the Intraday volume chart.
- Treat frontend-only RBAC as secure in a real implementation.
- Hardcode everything in one file once converting to Next.js.
- Introduce unnecessary dependencies without explaining why.

## Immediate next tasks

Recommended next development steps:

1. Split the current HTML into a Next.js component structure.
2. Create shared layout components:
   - Sidebar
   - Topbar
   - PageHeader
   - KpiCard
   - ChartCard
   - DataTable
3. Move mock data into `lib/mockData.ts`.
4. Rebuild each page as a React route.
5. Replace inline SVG chart helpers with a proper chart library.
6. Preserve the current visual design.
7. Implement role and department switching using mock RBAC.
8. Add alert drawer interactions.
9. Add realistic filter controls.
10. Add README documentation for running the app.

## Definition of done for the first conversion pass

The first conversion pass is complete when:

- The app runs locally.
- The visual layout matches the supplied HTML demo closely.
- All existing pages are represented.
- All charts render correctly.
- No chart text bleeds into other sections.
- Overview interval ribbon fits inside its card.
- Intraday volume chart is a 15-minute interval line chart.
- Mock data is separated from UI components.
- Navigation between pages works.
- Alerts open a detail drawer.
- Role/department access is simulated.
- Code is organised in a maintainable structure.

## Notes for Codex

Treat the supplied HTML dashboard as the design prototype and product reference.

Where the HTML uses inline scripts and generated SVG, convert those into maintainable React components. The goal is not simply to copy the HTML line-for-line, but to preserve the product design and behaviour while creating a clean, extensible codebase.

When uncertain, prioritise:
1. Preserving the current demo appearance
2. Creating reusable components
3. Keeping mock data realistic and structured
4. Avoiding visual regressions
5. Making the app easy to extend into a real Next.js + FastAPI product later
