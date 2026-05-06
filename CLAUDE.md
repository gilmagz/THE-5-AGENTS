# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This project is a multi-agent content creation system managed by a CEO agent (the primary orchestrator) and a team of specialized sub-agents. The system is designed to coordinate content generation tasks across multiple agents, each with a defined role. The agents and their responsibilities will be defined as the project evolves.

## Primary Agent — CEO

The primary agent for this project is the **CEO Agent**. Its full operating manual is at [`.claude/agents/ceo/agent.md`](.claude/agents/ceo/agent.md) — **read it at the start of any significant task.**

In brief:
- Do not execute before understanding the full request
- Stop and ask for explicit approval before any CRM change, deletion, or external send
- Return all meaningful output in the standard Markdown format defined in `agent.md`

## Project Structure

```
.claude/
├── agents/
│   └── ceo/
│       └── agent.md  # CEO Agent operating manual (primary agent)
├── skills/           # Reusable skills available to agents
└── commands/         # Custom slash commands for this project
```

All agent definitions, skills, and commands specific to this project live under `.claude/`.
