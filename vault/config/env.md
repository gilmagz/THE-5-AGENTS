---
tags: [config, env, secrets]
files: [.env, .env.example]
links: [[project/overview]]
---

# ENV Files

## מה הקבצים עושים
- `.env` — ערכי הסביבה האמיתיים (לא מועלה ל-git)
- `.env.example` — תבנית לשיתוף עם מפתחים אחרים (מועלה ל-git)

## למי שייך
כל רכיב בפרויקט שצורך API key או הגדרת סביבה.

## משתנים מוגדרים
| משתנה | תפקיד |
|--------|--------|
| `ANTHROPIC_API_KEY` | גישה ל-Claude API |
| `CEO_MODEL` | המודל של סוכן ה-CEO (ברירת מחדל: claude-opus-4-7) |
| `AGENT_MODEL` | המודל של סוכני הצוות (ברירת מחדל: claude-sonnet-4-6) |
| `LOG_LEVEL` | רמת לוגים (INFO/DEBUG) |

## קישורים קשורים
- [[config/gitignore]] — מוודא ש-.env לא עולה
- [[project/overview]] — הקשר הכולל
