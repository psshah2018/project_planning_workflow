# Project Planning Workflow

An n8n-based workflow for comprehensive project planning and task breakdown.

## Overview

This workflow helps analyze project requirements and break them down into individual tasks with detailed descriptions, timelines, dependencies, and deliverables.

## Features

- Manual trigger workflow execution
- Project requirements analysis
- Task breakdown with estimations
- Risk and uncertainty assessment
- Gantt chart/timeline visualization
- Structured output parsing

## Usage

1. Open this workflow in n8n
2. Click "Execute Workflow" to start
3. Provide project requirements including:
   - Project type
   - Project requirements
   - Team members (optional)
4. Receive comprehensive task breakdown with:
   - Detailed task descriptions
   - Timelines
   - Dependencies
   - Risk assessment
   - Conclusion

## Workflow Nodes

- **Manual Trigger**: Starts the workflow
- **Input Data**: Collects project requirements
- **LLM**: Analyzes and processes project data
- **Structured Output Parser**: Parses the output into structured format
- **Gantt Chart**: Generates timeline visualization

## Requirements

- n8n instance
- OpenAI API key (for LLM nodes)
- LangChain integration

## License

MIT