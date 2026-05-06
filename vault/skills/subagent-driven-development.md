---
tags: [skill, agents, development]
file: .claude/skills/subagent-driven-development/SKILL.md
links: [[skills/dispatching-parallel-agents]], [[skills/executing-plans]], [[project/overview]]
---

# Skill: subagent-driven-development

## מה הskill עושה
מפתח פיצ'רים באמצעות sub-agents מתמחים: spec reviewer, implementer, code quality reviewer.

## למי שייך
Claude (CEO) — מופעל למשימות פיתוח מורכבות שדורשות חלוקת עבודה.

## קבצים נלווים
| קובץ | תפקיד |
|------|--------|
| `spec-reviewer-prompt.md` | prompt לסוכן שבודק את המפרט |
| `implementer-prompt.md` | prompt לסוכן המממש |
| `code-quality-reviewer-prompt.md` | prompt לסוכן בדיקת איכות |

## זרימה
`spec` → `spec reviewer` → `implementer` → `quality reviewer` → `done`

## קישורים קשורים
- [[skills/dispatching-parallel-agents]] — הפצה במקביל
- [[skills/executing-plans]] — ביצוע התוכנית
- [[project/overview]] — ארכיטקטורת הסוכנים בפרויקט
