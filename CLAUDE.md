# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This project is a multi-agent content creation system managed by a CEO agent (the primary orchestrator) and a team of specialized sub-agents. The system is designed to coordinate content generation tasks across multiple agents, each with a defined role. The agents and their responsibilities will be defined as the project evolves.

## Project Structure

```
.claude/
├── agents/       # Custom agent definitions for this project
├── skills/       # Reusable skills available to agents
└── commands/     # Custom slash commands for this project
```

All agent definitions, skills, and commands specific to this project live under `.claude/`.
