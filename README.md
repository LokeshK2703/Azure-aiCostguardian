Updated todo list

Great. Here is a polished, hackathon-ready README draft you can use.

# Azure AI Cost Guardian

Intelligent model routing for cost-efficient Azure AI usage.

## Problem Statement

Many AI apps send every request to a premium model, which increases cost unnecessarily.  
Azure AI Cost Guardian adds a transparent decision layer that analyzes request complexity and routes each request to the most cost-effective model tier that can still satisfy quality needs.

## Solution Overview

Azure AI Cost Guardian does four things for each request:

1. Analyzes prompt complexity and task type
2. Recommends a capability tier (Cost-Efficient, Standard, Premium)
3. Executes on the selected tier (automatic or override)
4. Compares routed cost versus premium-baseline cost and logs privacy-safe telemetry

## Key Features

- Intelligent tier routing based on deterministic complexity scoring
- Three-tier model strategy:
  - routine-model
  - balanced-model
  - premium-model
- Estimated and actual cost comparison
- Admin dashboard with allocation, workload, and cost trends
- Clear decision explanation for each routing outcome
- Privacy-first telemetry (no raw prompt or response stored)
- Demo mode support for offline or restricted environments

## Architecture (High Level)

User Request  
→ Prompt Analysis  
→ Token Estimation  
→ Tier Recommendation  
→ Model Execution (Demo or Azure)  
→ Cost Calculation (Estimated + Actual)  
→ Telemetry Logging  
→ Dashboard Insights

## Tech Stack

- Python
- Streamlit
- OpenAI SDK (Azure OpenAI)
- Pydantic and pydantic-settings
- tiktoken
- Plotly
- pytest

## Project Structure

- app.py: Main customer experience page
- pages/1_Savings_Dashboard.py: Admin dashboard
- services/: Core orchestration, routing, execution, telemetry
- models/: Request/response contracts and enums
- config/: Routing and pricing policies
- tests/: Unit and UI tests
- data/: Local telemetry file storage

## Execution Modes

### Demo Mode

- No Azure credential dependency
- Deterministic synthetic responses
- Full routing and telemetry flow for demonstrations

### Azure Mode

- Live model execution using Azure endpoint and deployment mapping
- Uses actual token usage from Azure response for actual cost metrics
- Requires valid auth and deployment configuration

## Cost Logic

The app computes cost using token counts and per-million-token rates.

Estimated cost:
- Uses estimated input and output tokens before execution

Actual usage cost:
- Uses actual prompt and completion tokens returned by Azure after execution

Savings:
- Premium baseline cost minus routed tier cost

Note:
- Billing accuracy depends on keeping pricing values aligned with current Azure rates.

## Setup

### 1) Create virtual environment and install dependencies

    python -m venv .venv
    Activate.ps1
    pip install -r requirements.txt

### 2) Configure environment

Create a local .env from .env.example and set values for your mode.

Minimum for demo mode:

    APP_MODE=demo

Minimum for Azure mode:

    APP_MODE=azure
    AZURE_OPENAI_ENDPOINT=<your-endpoint>
    AZURE_OPENAI_API_VERSION=<your-api-version>
    AZURE_OPENAI_API_KEY=<your-key-if-key-auth-enabled>
    DEPLOYMENT_ECONOMY=routine-model
    DEPLOYMENT_BALANCED=balanced-model
    DEPLOYMENT_ADVANCED=premium-model

### 3) Run app

    streamlit run app.py

## Dashboard Metrics

- Requests Processed
- Cost-Efficient Selections
- Standard Selections
- Premium Selections
- AI Capability Allocation
- Request Volume Over Time
- Workload Distribution
- Estimated Cost Avoidance
- Actual spend/savings summary when actual usage is available

## Security and Privacy

- Do not commit real secrets
- Keep .env local only
- Telemetry stores metadata only
- Raw prompts and generated responses are not persisted in telemetry

## Known Constraints

- In some enterprise subscriptions, key-based auth may be disabled by policy
- In that case, use Entra-based auth path or request policy change
- Pricing values should be verified against official Azure pricing before business use

## Test Status

Current validation:

- 166 tests passing
- Compile checks passing

## Demo Walkthrough

1. Enter a routine prompt and run analysis
2. Review selected capability and explanation
3. Compare estimated and actual cost sections
4. Open AI Usage Dashboard for allocation and savings trends

## Future Enhancements

- Foundry Entra auth native path
- Prompt cost-optimization assistant
- Dynamic pricing sync
- Policy-aware routing profiles by business unit
- Exportable FinOps reports

## Disclaimer

This project is a hackathon prototype for demonstration and learning purposes.  
It is not an official Microsoft product, service, or recommendation.  
Pricing values shown should be verified against current Azure pricing before production or business decision use.
