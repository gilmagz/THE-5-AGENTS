---
tags: [skill, agents, parallelism]
file: .claude/skills/dispatching-parallel-agents/SKILL.md
links: [[skills/subagent-driven-development]], [[project/overview]]
---

# Skill: dispatching-parallel-agents

## מה הskill עושה
מפיץ משימות מרובות ובלתי-תלויות לסוכנים שעובדים במקביל.

## למי שייך
Claude (סוכן ראשי / CEO) — משמש כשיש 2+ בעיות עצמאיות שאפשר לפתור בו-זמנית.

## מתי להשתמש
- 3+ קבצי בדיקות נכשלים מסיבות שונות
- מספר מערכות שבורות באופן עצמאי
- אין state משותף בין המשימות

## מתי לא להשתמש
- כשלונות קשורים זה לזה
- צריך הקשר מלא של המערכת
- debugging חקרני (לא ידוע מה שבור)

## מבנה prompt לסוכן
1. scope ממוקד (קובץ אחד / מערכת אחת)
2. כל ההקשר הנדרש (self-contained)
3. פלט מוגדר — מה הסוכן צריך להחזיר

## קישורים קשורים
- [[skills/subagent-driven-development]] — פיתוח מונחה sub-agents
- [[project/overview]] — ארכיטקטורת הסוכנים
