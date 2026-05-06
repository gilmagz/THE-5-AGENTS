---
tags: [project, overview]
links: [[config/CLAUDE-md]], [[config/env]]
---

# Project Overview — THE 5 AGENTS

## מה הפרויקט עושה
מערכת סוכנים ליצירת תוכן. סוכן ראשי (CEO) מנהל ומתאם צוות של סוכנים מתמחים. כל סוכן מקבל משימה ממוקדת ומחזיר תוצאה.

## מבנה
```
.claude/
├── agents/    ← הגדרות סוכנים (יתמלא בהמשך)
├── skills/    ← skills מ-Superpowers + מותאמים
└── commands/  ← פקודות מותאמות (יתמלא בהמשך)
vault/         ← מאגר ידע (Obsidian-style)
```

## שלב נוכחי
הקמת תשתית — מבנה תיקיות, ENV, skills בסיסיים. טרם הוגדרו סוכנים.

## קישורים קשורים
- [[config/CLAUDE-md]] — הנחיות עבודה ל-Claude
- [[config/env]] — הגדרות API ומודלים
