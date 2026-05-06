---
tags: [config, git]
file: .gitignore
links: [[config/env]]
---

# .gitignore

## מה הקובץ עושה
מגדיר אילו קבצים git לא יעקוב אחריהם.

## למי שייך
git — נקרא אוטומטית בכל פעולת commit/status.

## מה מוחרג
| קובץ/תבנית | סיבה |
|-------------|-------|
| `.env` | מכיל מפתחות API — אסור ב-git |
| `__pycache__/` | קבצי Python מקומפלים |
| `*.pyc` | bytecode של Python |
| `.DS_Store` | מטא-דאטה של macOS |

## קישורים קשורים
- [[config/env]] — הקובץ המוחרג העיקרי
