# Project Planning Workflow - Architecture

## Overview

The Project Planning Workflow is an n8n-based automation system that analyzes project requirements and breaks them down into structured tasks with timelines, dependencies, and risk assessments.

## System Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                     Project Planning Workflow                   │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌──────────────────┐     ┌─────────────────────────────────┐  │
│  │  Manual Trigger  │────▶│         Input Data              │  │
│  │  (Start Node)    │     │  (Collect Project Requirements) │  │
│  └──────────────────┘     └─────────────────────────────────┘  │
│                                          │                      │
│                                          ▼                      │
│                                  ┌───────────────┐              │
│                                  │      LLM      │              │
│                                  │  (AI Analysis)│              │
│                                  └───────────────┘              │
│                                          │                      │
│                    ┌─────────────────────┼─────────────────────┤
│                    ▼                     ▼                     ▼
│           ┌───────────────┐    ┌───────────────┐    ┌───────────────┐
│           │  Structured   │    │  Structured  │    │  Gantt Chart  │
│           │  Output       │    │  Output       │    │  (Timeline   │
│           │  Parser       │    │  Parser1      │    │  Visualization│
│           └───────────────┘    └───────────────┘    └───────────────┘
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

## Components

### 1. Manual Trigger Node
- **Type**: `n8n-nodes-base.manualTrigger`
- **Purpose**: Entry point for workflow execution
- **Function**: Starts the workflow when user clicks "Execute Workflow"

### 2. Input Data Node
- **Purpose**: Collects project requirements from user
- **Inputs**:
  - `project_type`: Type of project being planned
  - `project_requirements`: Detailed requirements
  - `team_members`: Optional team member assignments

### 3. LLM Node (LangChain Integration)
- **Type**: `@n8n/n8n-nodes-langchain`
- **Purpose**: AI-powered analysis and task breakdown
- **Model**: OpenAI API (GPT-based)
- **Function**: 
  - Analyzes project requirements
  - Breaks down into individual tasks
  - Estimates timelines
  - Identifies dependencies
  - Assesses risks and uncertainties

### 4. Structured Output Parser Nodes
- **Type**: `@n8n/n8n-nodes-langchain.outputParserStructured`
- **Purpose**: Parses LLM output into structured format
- **Variants**:
  - **Parser 1**: Extracts Title, Overview, Task Breakdown
  - **Parser 2**: Extracts Title, Overview, Task Breakdown & Estimations, Risks, Conclusion

### 5. Gantt Chart Node
- **Purpose**: Generates timeline visualization
- **Output**: Visual representation of project timeline and task dependencies

## Data Flow

1. **Trigger** → User initiates workflow via manual trigger
2. **Input** → User provides project requirements (type, requirements, team)
3. **Analysis** → LLM processes and analyzes the input data
4. **Parsing** → Structured output parsers convert AI response into usable format
5. **Visualization** → Gantt chart generates visual timeline
6. **Output** → Comprehensive task breakdown with descriptions, timelines, dependencies, risks

## Data Schema

### Input Schema
```json
{
  "project_type": "string",
  "project_requirements": "string",
  "team_members": "string | null"
}
```

### Output Schema
```json
{
  "Title": "string",
  "Overview": "string",
  "Task Breakdown": "string[]",
  "Task Breakdown and Estimations": "string[]",
  "Summary of Risks and Uncertainties": "string",
  "Conclusion": "string"
}
```

## Technology Stack

| Component | Technology |
|-----------|------------|
| Workflow Engine | n8n |
| AI Model | OpenAI API |
| AI Integration | LangChain |
| Output Parsing | n8n LangChain Nodes |

## Requirements

- n8n instance (self-hosted or cloud)
- OpenAI API key (for LLM nodes)
- LangChain integration package (`@n8n/n8n-nodes-langchain`)

## Extension Points

- **AdditionalLLMs**: Support for alternative LLM providers (Anthropic, Cohere)
- **CustomParsers**: Extend output parsing for specific project types
- **NotificationNodes**: Add email/Slack notifications for task assignments
- **DatabaseIntegration**: Store project plans in persistent storage

## Security Considerations

- API keys stored as credentials in n8n
- Workflow access controlled via n8n permissions
- Input validation on user-provided data