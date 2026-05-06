---
tags: [skill, meta, vault, workflow]
file: .claude/skills/obsidian-vault-workflow/SKILL.md
links: [[project/overview]], [[skills/writing-skills]]
---

# Skill: obsidian-vault-workflow

## מה הskill עושה
מנהל את ה-vault כ"זיכרון ארוך טווח" של הפרויקט. מחייב קריאת ה-vault בתחילת כל session ועדכון vault note בסיום כל משימה.

## למי שייך
Claude — חובה להפעיל בתחילת כל session ואחרי כל פקודה.

## שתי פאזות

### Phase 1 — לפני כל משימה
1. זהה את נושא המשימה
2. מצא/צור topic file מתאים ב-vault
3. קרא Meeting Notes אחרונות + Content Briefs רלוונטיים + Brand Guidelines
4. דווח מה נטען לפני שמתחילים

### Phase 2 — אחרי כל משימה
1. בחר תיקיית vault מתאימה
2. עדכן/צור topic file עם Session Log entry ממוין
3. כלול `[[wikilinks]]` לנושאים קשורים
4. קרא בחזרה לאימות
5. **רק אז** הכרז על סיום

## מבנה topic file
```
# Topic Title
## Overview
## Open Questions
## Session Log
### YYYY-MM-DD — title [status]
- What was done / Decisions / Notes / Related
```

## תיקיות ב-vault
| תיקייה | מתי |
|---------|------|
| `vault/Meeting Notes/` | קוד, ארכיטקטורה, החלטות |
| `vault/Content Briefs/` | תוכן, עריכה |
| `vault/Publishing Log/` | פרסום ותוצאות |
| `vault/Brand Guidelines/` | עיצוב, קול, טון |

## קישורים קשורים
- [[project/overview]] — הפרויקט שה-vault מתעד
- [[skills/writing-skills]] — כיצד נכתב skill כזה
