# Crypto Due Diligence Agent

An autonomous AI-powered due diligence system that performs institutional-grade technical analysis on cryptocurrency projects in real-time. The agent orchestrates multiple specialized analysis modules to evaluate smart contract security, whitepaper legitimacy, and tokenomics sustainability, delivering comprehensive risk assessments in seconds rather than the days or weeks traditional audits require.

## The Problem

The cryptocurrency ecosystem moves at an unforgiving pace. New projects launch daily, each presenting potential opportunity alongside substantial risk. Traditional due diligence is slow, expensive, and doesn't scale. Retail investors lack access to the sophisticated analysis tools that institutions use, while even professional analysts struggle to keep pace with the volume of new projects entering the market.

Rug pulls, honeypot contracts, and vaporware whitepapers have collectively drained billions from the ecosystem. The asymmetry is stark: bad actors can deploy a malicious project in minutes, but thorough analysis takes days. By the time red flags surface through conventional means, the damage is done.

## The Solution

This system inverts the asymmetry. By combining large language model reasoning with deterministic smart contract analysis, the agent performs multi-dimensional due diligence that would traditionally require a team of specialists: a security auditor, a tokenomics analyst, and a technical writer reviewer, all working in parallel.

The architecture employs a hierarchical agent pattern. A central orchestrator delegates to domain-specific analysis agents, each operating with carefully tuned system prompts that encode years of accumulated knowledge about crypto-specific red flags, attack vectors, and manipulation patterns. The agents don't just parse documents; they reason about implications, cross-reference claims against technical feasibility, and synthesize findings into actionable risk scores.

## System Architecture
```mermaid
flowchart TB
    subgraph Client["Client Layer"]
        UI[Web Interface]
        API_CLIENT[API Consumer]
    end

    subgraph Gateway["API Gateway"]
        EXPRESS[Express.js Server]
        VALIDATION[Request Validation]
        RATE_LIMIT[Rate Limiting]
    end

    subgraph Orchestration["Orchestration Layer"]
        ORCH[Orchestrator Agent]
        TASK_DECOMP[Task Decomposition]
        PARALLEL[Parallel Execution Engine]
        SYNTH[Result Synthesizer]
    end

    subgraph Agents["Specialized Analysis Agents"]
        WP[Whitepaper Agent]
        CONTRACT[Contract Agent]
        TOKEN[Tokenomics Agent]
    end

    subgraph Intelligence["AI Intelligence Layer"]
        CLAUDE[Claude API]
        PROMPTS[(System Prompts)]
        JSON_PARSE[Structured Output Parser]
    end

    subgraph DataSources["External Data Sources"]
        ETHERSCAN[Etherscan API]
        BASESCAN[Basescan API]
        ARBISCAN[Arbiscan API]
        POLYGONSCAN[Polygonscan API]
    end

    subgraph Persistence["Persistence Layer"]
        SUPABASE[(Supabase PostgreSQL)]
        CACHE[(Analysis Cache)]
    end

    subgraph Output["Risk Assessment Output"]
        SCORE[Risk Score Engine]
        FLAGS[Red Flag Aggregator]
        VERDICT[Verdict Generator]
        RECS[Recommendation Engine]
    end

    UI --> EXPRESS
    API_CLIENT --> EXPRESS
    EXPRESS --> VALIDATION
    VALIDATION --> RATE_LIMIT
    RATE_LIMIT --> ORCH

    ORCH --> TASK_DECOMP
    TASK_DECOMP --> PARALLEL
    PARALLEL --> WP
    PARALLEL --> CONTRACT
    PARALLEL --> TOKEN

    WP --> CLAUDE
    CONTRACT --> CLAUDE
    TOKEN --> CLAUDE
    CLAUDE --> PROMPTS
    CLAUDE --> JSON_PARSE

    CONTRACT --> ETHERSCAN
    CONTRACT --> BASESCAN
    CONTRACT --> ARBISCAN
    CONTRACT --> POLYGONSCAN

    WP --> SYNTH
    CONTRACT --> SYNTH
    TOKEN --> SYNTH

    SYNTH --> SCORE
    SYNTH --> FLAGS
    SCORE --> VERDICT
    FLAGS --> VERDICT
    VERDICT --> RECS

    RECS --> SUPABASE
    RECS --> CACHE
    SUPABASE --> EXPRESS
```

## Agent Communication Flow
```mermaid
sequenceDiagram
    participant C as Client
    participant G as API Gateway
    participant O as Orchestrator
    participant W as Whitepaper Agent
    participant S as Contract Agent
    participant T as Tokenomics Agent
    participant AI as Claude API
    participant DB as Supabase

    C->>G: POST /api/analyze
    G->>G: Validate Request
    G->>O: Initialize Analysis

    par Parallel Agent Execution
        O->>W: Analyze Whitepaper
        W->>AI: Send whitepaper + system prompt
        AI-->>W: Structured analysis JSON

        O->>S: Analyze Contract
        S->>S: Fetch source from block explorer
        S->>AI: Send contract + system prompt
        AI-->>S: Security findings JSON

        O->>T: Analyze Tokenomics
        T->>AI: Send tokenomics data + system prompt
        AI-->>T: Distribution analysis JSON
    end

    W-->>O: Whitepaper red flags
    S-->>O: Contract red flags
    T-->>O: Tokenomics red flags

    O->>O: Aggregate flags
    O->>O: Calculate risk score
    O->>O: Generate verdict
    O->>O: Build recommendations

    O->>DB: Persist analysis
    O-->>G: Complete analysis
    G-->>C: Risk assessment response
```

## Risk Scoring Pipeline
```mermaid
flowchart LR
    subgraph Input["Red Flag Input"]
        CRITICAL[Critical Flags]
        HIGH[High Flags]
        MEDIUM[Medium Flags]
        LOW[Low Flags]
    end

    subgraph Weights["Severity Weights"]
        W_CRIT["+30 points"]
        W_HIGH["+15 points"]
        W_MED["+7 points"]
        W_LOW["+3 points"]
    end

    subgraph Calculation["Score Calculation"]
        AGG[Aggregate Weighted Sum]
        CAP[Cap at 100]
    end

    subgraph Classification["Risk Classification"]
        R_LOW["0-19: LOW"]
        R_MOD["20-44: MODERATE"]
        R_HIGH["45-69: HIGH"]
        R_CRIT["70-100: CRITICAL"]
    end

    subgraph Output["Final Output"]
        VERDICT[Risk Verdict]
        RECS[Recommendations]
    end

    CRITICAL --> W_CRIT --> AGG
    HIGH --> W_HIGH --> AGG
    MEDIUM --> W_MED --> AGG
    LOW --> W_LOW --> AGG

    AGG --> CAP
    CAP --> R_LOW
    CAP --> R_MOD
    CAP --> R_HIGH
    CAP --> R_CRIT

    R_LOW --> VERDICT
    R_MOD --> VERDICT
    R_HIGH --> VERDICT
    R_CRIT --> VERDICT
    VERDICT --> RECS
```

## Analysis Modules

### Whitepaper Agent
```mermaid
mindmap
  root((Whitepaper Analysis))
    Technical Claims
      Architecture feasibility
      Scalability assertions
      Innovation assessment
      Implementation specifics
    Economic Model
      Revenue sustainability
      Token utility logic
      Game theory alignment
      Incentive structures
    Red Flag Detection
      Guaranteed returns
      Plagiarized content
      Vague promises
      Impossible timelines
    Trust Signals
      Team credentials
      Advisor verification
      Partnership claims
      Audit references
```

The whitepaper agent performs deep semantic analysis on project documentation. Unlike simple keyword matching, it evaluates the logical coherence of technical claims, identifies impossible promises (guaranteed returns, physics-defying scalability claims), and measures buzzword density as a proxy for substance. The agent maintains an internal model of what legitimate blockchain architectures look like and flags deviations from established technical patterns.

**Key detection capabilities:**
- Plagiarism indicators and recycled content patterns
- Vague technical claims that lack implementation specifics
- Economic models that violate basic game theory
- Timeline feasibility given stated technical scope
- Team credential verification gaps

### Contract Agent
```mermaid
mindmap
  root((Contract Analysis))
    Ownership Risks
      Admin privileges
      Multisig requirements
      Renounced ownership
      Timelock presence
    Token Controls
      Mint functions
      Burn mechanisms
      Pause capability
      Blacklist functions
    Security Patterns
      Reentrancy guards
      Access controls
      External calls
      Delegate calls
    Upgrade Risks
      Proxy patterns
      Implementation slots
      Logic migration
      Storage collisions
```

The contract agent analyzes smart contract source code through the lens of security and centralization risk. It identifies ownership patterns that grant excessive control, mint functions without supply caps, pause mechanisms that could freeze user funds, and proxy patterns that allow silent logic upgrades. The analysis goes beyond simple function detection to evaluate how these capabilities interact and compound risk.

**Key detection capabilities:**
- Reentrancy vulnerability patterns
- Unlimited minting authority
- Owner-controlled transfer restrictions
- Hidden fee mechanisms
- Self-destruct capabilities
- Upgradeable proxy patterns without timelocks

### Tokenomics Agent
```mermaid
mindmap
  root((Tokenomics Analysis))
    Distribution
      Team allocation
      Investor share
      Public percentage
      Treasury reserves
    Vesting
      Cliff periods
      Linear unlock
      Milestone based
      Immediate liquidity
    Emission
      Inflation rate
      Supply cap
      Burn mechanics
      Reward schedules
    Risk Patterns
      Insider concentration
      Dump coordination
      Circular utility
      Ponzi mechanics
```

The tokenomics agent evaluates the economic sustainability of token distribution and emission schedules. It identifies allocation patterns that favor insiders, vesting schedules that create coordinated dump risk, and inflationary models that dilute retail holders. The agent understands that tokenomics is game theory: it evaluates incentive alignment between teams, investors, and public participants.

**Key detection capabilities:**
- Excessive team/insider allocations (over 20%)
- Missing or abbreviated vesting periods
- Cliff concentrations that enable coordinated selling
- Circular utility models (tokens for tokens)
- Unsustainable yield mechanics

## Risk Classification Matrix
```mermaid
quadrantChart
    title Risk Assessment Quadrant
    x-axis Low Technical Risk --> High Technical Risk
    y-axis Low Economic Risk --> High Economic Risk
    quadrant-1 Critical Risk
    quadrant-2 Security Focus
    quadrant-3 Generally Safe
    quadrant-4 Tokenomics Focus
    "Verified Contract + Fair Launch": [0.2, 0.2]
    "Proxy Pattern + Good Distribution": [0.7, 0.3]
    "Simple Contract + Team Heavy": [0.3, 0.7]
    "Upgradeable + No Vesting": [0.8, 0.85]
    "Audited + VC Backed": [0.25, 0.45]
    "Anonymous + Mint Function": [0.9, 0.6]
```

## Data Flow Architecture
```mermaid
flowchart TD
    subgraph Input["Input Sources"]
        WP_TEXT[Whitepaper Text]
        WP_URL[Whitepaper URL]
        CONTRACT_ADDR[Contract Address]
        CHAIN[Chain Identifier]
    end

    subgraph Extraction["Data Extraction"]
        PDF_PARSE[PDF Parser]
        HTML_FETCH[HTML Fetcher]
        SOURCE_FETCH[Contract Source Fetcher]
        TOKENOMICS_EXTRACT[Tokenomics Extractor]
    end

    subgraph Processing["AI Processing"]
        CLAUDE_WP[Claude: Whitepaper Analysis]
        CLAUDE_CONTRACT[Claude: Contract Analysis]
        CLAUDE_TOKEN[Claude: Tokenomics Analysis]
    end

    subgraph Structured["Structured Output"]
        WP_JSON[WhitepaperAnalysis JSON]
        CONTRACT_JSON[ContractAnalysis JSON]
        TOKEN_JSON[TokenomicsAnalysis JSON]
    end

    subgraph Synthesis["Risk Synthesis"]
        FLAG_AGG[Flag Aggregation]
        SCORE_CALC[Score Calculation]
        VERDICT_GEN[Verdict Generation]
        REC_BUILD[Recommendation Builder]
    end

    subgraph Storage["Persistence"]
        SUPABASE_WRITE[(Write to Supabase)]
        RESPONSE[API Response]
    end

    WP_TEXT --> CLAUDE_WP
    WP_URL --> PDF_PARSE --> CLAUDE_WP
    WP_URL --> HTML_FETCH --> CLAUDE_WP
    CONTRACT_ADDR --> SOURCE_FETCH --> CLAUDE_CONTRACT
    CHAIN --> SOURCE_FETCH
    WP_TEXT --> TOKENOMICS_EXTRACT --> CLAUDE_TOKEN

    CLAUDE_WP --> WP_JSON
    CLAUDE_CONTRACT --> CONTRACT_JSON
    CLAUDE_TOKEN --> TOKEN_JSON

    WP_JSON --> FLAG_AGG
    CONTRACT_JSON --> FLAG_AGG
    TOKEN_JSON --> FLAG_AGG

    FLAG_AGG --> SCORE_CALC
    SCORE_CALC --> VERDICT_GEN
    VERDICT_GEN --> REC_BUILD

    REC_BUILD --> SUPABASE_WRITE
    REC_BUILD --> RESPONSE
```

## State Machine: Analysis Lifecycle
```mermaid
stateDiagram-v2
    [*] --> Received: API Request

    Received --> Validating: Parse body
    Validating --> Rejected: Invalid input
    Validating --> Initialized: Valid input

    Rejected --> [*]: Return 400

    Initialized --> Extracting: Fetch external data
    Extracting --> Analyzing: Data ready

    Analyzing --> WhitepaperAnalysis: If whitepaper provided
    Analyzing --> ContractAnalysis: If contract provided
    Analyzing --> TokenomicsAnalysis: If tokenomics available

    WhitepaperAnalysis --> Synthesizing: Complete
    ContractAnalysis --> Synthesizing: Complete
    TokenomicsAnalysis --> Synthesizing: Complete

    Synthesizing --> Scoring: Aggregate flags
    Scoring --> Persisting: Calculate risk
    Persisting --> Completed: Save to DB

    Completed --> [*]: Return analysis

    Analyzing --> Failed: Claude API error
    Extracting --> Failed: External API error
    Persisting --> Failed: Database error

    Failed --> [*]: Return 500
```

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/health` | Service health check |
| POST | `/api/analyze` | Full multi-agent analysis |
| POST | `/api/analyze/whitepaper` | Whitepaper-only analysis |
| POST | `/api/analyze/contract` | Contract-only analysis |
| GET | `/api/analysis/:id` | Retrieve saved analysis |

See `API_DOCUMENTATION.md` for complete request/response specifications.

## Risk Scoring Methodology

The system produces a composite risk score from 0-100 using severity-weighted flag aggregation:

| Severity | Weight | Examples |
|----------|--------|----------|
| Critical | +30 | Guaranteed returns, unlimited mint, no vesting |
| High | +15 | Anonymous team, proxy upgrades, over 30% team allocation |
| Medium | +7 | High buzzword density, missing audit, short vesting |
| Low | +3 | Minor documentation gaps, standard owner functions |

**Final risk classification:**

| Score | Level | Interpretation |
|-------|-------|----------------|
| 0-19 | Low | Standard risk profile, proceed with normal caution |
| 20-44 | Moderate | Notable concerns requiring additional research |
| 45-69 | High | Significant red flags, elevated caution warranted |
| 70-100 | Critical | Severe risk indicators, extreme caution advised |

## Tech Stack
```mermaid
flowchart LR
    subgraph Runtime
        NODE[Node.js]
        TS[TypeScript]
    end

    subgraph Framework
        EXPRESS[Express.js]
        VERCEL[Vercel Serverless]
    end

    subgraph AI
        ANTHROPIC[Anthropic Claude API]
    end

    subgraph Database
        SUPABASE[Supabase PostgreSQL]
    end

    subgraph External
        ETHERSCAN[Etherscan]
        BASESCAN[Basescan]
        ARBISCAN[Arbiscan]
        POLYGONSCAN[Polygonscan]
    end

    NODE --> TS
    TS --> EXPRESS
    EXPRESS --> VERCEL
    EXPRESS --> ANTHROPIC
    EXPRESS --> SUPABASE
    EXPRESS --> ETHERSCAN
    EXPRESS --> BASESCAN
    EXPRESS --> ARBISCAN
    EXPRESS --> POLYGONSCAN
```

- **Runtime:** Node.js + TypeScript
- **Framework:** Express.js
- **AI Engine:** Claude (Anthropic API)
- **Database:** Supabase (PostgreSQL)
- **Deployment:** Vercel (Serverless)
- **Chain Data:** Etherscan, Basescan, Arbiscan, Polygonscan APIs

## Installation
```bash
# Clone the repository
git clone https://github.com/yourusername/crypto-due-diligence-agent.git
cd crypto-due-diligence-agent

# Install dependencies
npm install

# Configure environment
cp .env.example .env
# Add your keys to .env:
#   ANTHROPIC_API_KEY=your_key
#   SUPABASE_URL=your_url
#   SUPABASE_ANON_KEY=your_key

# Run locally
npm run dev

# Deploy to production
vercel --prod
```

## Environment Variables

| Variable | Description |
|----------|-------------|
| `ANTHROPIC_API_KEY` | Claude API key from Anthropic |
| `SUPABASE_URL` | Supabase project URL |
| `SUPABASE_ANON_KEY` | Supabase anonymous key |

## Project Structure
```
crypto-due-diligence-agent/
├── api/
│   └── index.ts              # Express server + route handlers
├── src/
│   ├── agents/
│   │   ├── orchestrator.agent.ts   # Central coordination
│   │   ├── whitepaper.agent.ts     # Document analysis
│   │   ├── contract.agent.ts       # Smart contract analysis
│   │   └── tokenomics.agent.ts     # Economic analysis
│   ├── services/
│   │   ├── claude.service.ts       # Anthropic API integration
│   │   └── supabase.service.ts     # Database operations
│   ├── types/
│   │   └── index.ts                # TypeScript interfaces
│   └── utils/
│       └── extractors.ts           # Data extraction utilities
├── vercel.json                     # Deployment configuration
├── API_DOCUMENTATION.md            # Frontend integration docs
└── README.md
```

## License

MIT