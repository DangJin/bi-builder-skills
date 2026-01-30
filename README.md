# BI Builder Skill

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Skill-blueviolet)](https://claude.ai/code)
[![Next.js](https://img.shields.io/badge/Next.js-16-black?logo=next.js)](https://nextjs.org/)
[![Prisma](https://img.shields.io/badge/Prisma-ORM-2D3748?logo=prisma)](https://www.prisma.io/)
[![Recharts](https://img.shields.io/badge/Recharts-Charts-22b5bf)](https://recharts.org/)
[![shadcn/ui](https://img.shields.io/badge/shadcn%2Fui-Components-000000)](https://ui.shadcn.com/)

A Claude Code skill for rapidly building BI dashboards and data visualization applications from existing databases.

## Features

- **6-Phase Workflow**: Database Connection → Schema Exploration → Requirements Dialog → Metrics Design → Chart Planning → Page Implementation
- **4 Layout Patterns**: Executive, Operational, Analytical, Comparison dashboards with complete templates
- **Industry-Specific Templates**: Pre-configured metrics for 7 industries (E-commerce, SaaS, Finance, Content, Education, Healthcare, Logistics)
- **Smart Skip Conditions**: Automatically skip phases based on project state
- **Progressive Document Loading**: Load reference docs on-demand to save context

## How It Works

```mermaid
flowchart TB
    subgraph Phase1["Phase 1: Database Connection"]
        A1[Check Prisma Installation] --> A2[Initialize Prisma]
        A2 --> A3[Create .env with Placeholders]
        A3 --> A4[User Fills Credentials]
        A4 --> A5[Pull Database Schema]
    end

    subgraph Phase2["Phase 2: Schema Exploration"]
        B1[Analyze Tables & Fields] --> B2[Identify Relationships]
        B2 --> B3[Detect Metric Potential]
        B3 --> B4[Generate Data Overview Report]
    end

    subgraph Phase3["Phase 3: Requirements Dialog"]
        C1[Identify Industry] --> C2[Suggest Core Metrics]
        C2 --> C3[Confirm Time Granularity]
        C3 --> C4[Define Filters & Features]
    end

    subgraph Phase4["Phase 4: Metrics Design"]
        D1[Define KPI Calculations] --> D2[Design Time Series Queries]
        D2 --> D3[Create Aggregation Functions]
    end

    subgraph Phase5["Phase 5: Chart & Layout Planning"]
        E1[Select Visualization Types] --> E2[Choose Layout Pattern]
        E2 --> E3[Plan Component Structure]
    end

    subgraph Phase6["Phase 6: Page Implementation"]
        F1[Create API Routes] --> F2[Build Chart Components]
        F2 --> F3[Build DataTable Components]
        F3 --> F4[Assemble Dashboard Page]
    end

    Phase1 --> Phase2
    Phase2 --> Phase3
    Phase3 --> Phase4
    Phase4 --> Phase5
    Phase5 --> Phase6

    %% Skip conditions
    Skip1{{"prisma/schema.prisma exists?"}}
    Skip2{{"Clear requirements?"}}
    Skip3{{"Single chart only?"}}

    Skip1 -->|Yes| Phase2
    Skip2 -->|Yes| Phase4
    Skip3 -->|Yes| Phase6
```

## Tech Stack

| Layer | Technology |
|-------|------------|
| Frontend Framework | Next.js 16 (App Router) |
| UI Components | shadcn/ui + Tailwind CSS |
| Charts | Recharts |
| ORM | Prisma |
| Database | MySQL / PostgreSQL / Supabase / SQLite |

## Installation

```bash
# Install using npx skills (recommended)
npx skills add DangJin/bi-builder-skills

# Or manually clone and symlink
git clone https://github.com/DangJin/bi-builder-skills.git
ln -s $(pwd)/bi-builder-skills/skills/bi-builder ~/.claude/skills/bi-builder
```

## Usage

Trigger the skill in Claude Code by mentioning:
- "Create a data dashboard"
- "Build BI dashboard"
- "Generate sales analytics page"
- "Create data visualization charts"

## Project Structure

```
bi-builder-skills/
├── skills/
│   └── bi-builder/
│       ├── SKILL.md                 # Main skill file
│       └── references/
│           ├── data-layer.md        # Prisma queries & API design
│           ├── recharts-guide.md    # Chart type examples
│           ├── table-patterns.md    # DataTable patterns
│           ├── dashboard-patterns.md # Layout & component patterns
│           └── export-patterns.md   # CSV & image export
└── README.md
```

## Workflow Flexibility

| Scenario | Skip Phases | Starting Point |
|----------|-------------|----------------|
| Project has `prisma/schema.prisma` | Phase 1 | Go directly to schema analysis |
| User has clear requirements | Phase 3 | Go directly to metrics design |
| Only need a single chart | Phases 1-5 | Read recharts-guide.md |

## Industry Templates

| Industry | Core Metrics |
|----------|--------------|
| E-commerce/Retail | GMV, AOV, Repeat purchase rate, Return rate |
| SaaS Software | MRR/ARR, Churn Rate, LTV, CAC, DAU/MAU |
| Financial Services | AUM, Bad debt rate, Approval rate |
| Content/Media | PV/UV, Session duration, Ad revenue |
| Education | Course completion rate, Renewal rate |
| Healthcare | Visit volume, Bed turnover, Satisfaction |
| Logistics | Order fulfillment rate, Delivery time |

## Layout Patterns

| Layout | Best For | Key Features |
|--------|----------|--------------|
| **Executive Dashboard** | C-level, managers | KPI cards + main trend chart + distribution |
| **Operational Dashboard** | Operations team | Real-time status bar + live table + alerts |
| **Analytical Dashboard** | Analysts, data team | Sidebar filters + drill-down + detailed table |
| **Comparison Dashboard** | Strategy, planning | Period selector + dual charts + change analysis |

Each layout includes complete code templates with responsive design.

## Reference Documents

| Document | Content |
|----------|---------|
| `data-layer.md` | Prisma schema analysis, aggregation queries, API routes |
| `recharts-guide.md` | Line, Bar, Pie, Area, Composed, Scatter, Radar, Funnel charts |
| `table-patterns.md` | DataTable with sorting, filtering, pagination, row selection |
| `dashboard-patterns.md` | 4 layout patterns, KPI cards, filter bar, responsive grid |
| `export-patterns.md` | Client/server CSV export, html2canvas image export |

## License

[MIT](./LICENSE)
