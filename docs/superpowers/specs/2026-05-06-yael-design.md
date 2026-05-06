# Design: יעל — סוכנת כתיבת תוכן

**Date:** 2026-05-06  
**Status:** Approved  
**Stakeholders:** ממוקה (orchestrator), יובל (image agent)

---

## Context

המערכת צריכה סוכן שמשכתב מאמרי גלם בסגנון הכתיבה של הפרויקט. יעל היא LLM-only — אין לה גישה ל-Bash, API, או WebSearch — רק Read, Write, Edit, Glob, Grep. היא עובדת על מאמר אחד בכל הפעלה, לפי הנחיית ממוקה.

---

## Architecture

```
ממוקה → "יעל, עבדי על Content/article.md"
         ↓
יעל: קרא style-guide + reference → קרא מאמר → שכתב → סמן IMAGE_NEEDED → שמור Output/
         ↓
יעל מחזירה לממוקה: סיכום + רשימת IMAGE_NEEDED placeholders
         ↓
ממוקה: יובל לכל placeholder → החלף ב-path → שמור Output/ סופי → Bash mv ל-Content/Ready/
```

**מגבלה מכוונת:** יעל אינה מפעילה סוכנים אחרים. רק ממוקה מפעיל יובל.

---

## Files to Create

| קובץ | תוכן |
|------|-------|
| `.claude/agents/yael.md` | קובץ סוכן קנוני (flat, discoverable) |
| `yael/style-guide.md` | תבנית מובנית עם placeholders |
| `yael/reference/.gitkeep` | מקום לדוגמאות כתיבה |
| `yael/agent.md` | pointer doc לבני אדם |
| `Content/.gitkeep` | תיקיית קלט |
| `Content/Ready/.gitkeep` | תיקיית ארכיב |
| `Output/.gitkeep` | תיקיית תוצרים |

---

## Agent Definition — `.claude/agents/yael.md`

**Frontmatter:**
```yaml
name: yael
description: "סוכנת כתיבת תוכן LLM-only. שלחי אותי עם path מדויק למאמר גלם ב-Content/ — אני קוראת את מדריך הסגנון, משכתבת בסגנון הפרויקט, ומסמנת איפה צריך תמונות. Triggers: שכתב, ערוך, נסח מחדש, תרגם, סכם, מאמר, תוכן, פוסט, rewrite, edit, rephrase, translate, summarize, article, content, post."
```

**Tools:** `Read, Write, Edit, Glob, Grep` — ללא Bash, ללא WebSearch, ללא API.

**Workflow:**
1. קרא `yael/style-guide.md`
2. סרוק `yael/reference/` עם Glob — קרא קבצי `.md` שמוצאים
3. קרא את `Content/<article>` שממוקה שלח
4. שכתב: החל טון, מבנה, אורך פסקאות, אוצר מילים לפי style-guide
5. כשמזהה מקום מתאים לתמונה — הכנס `{{IMAGE_NEEDED: "<prompt מפורט באנגלית לפרומפט של יובל>"}}`
6. שמור ב-`Output/<original-name>.md`
7. החזר לממוקה: סיכום שינויים + רשימת IMAGE_NEEDED (אם יש)

---

## IMAGE_NEEDED Placeholder Format

```
{{IMAGE_NEEDED: "A professional photo of X showing Y, style Z, mood W"}}
```

- תמיד באנגלית (לפרומפט של יובל)
- מפורט: subject, style, mood, composition
- מוכנס בתוך המאמר במקום הרלוונטי

---

## ממוקה Updates

### יעל בסוכני-משנה:
```
### יעל (yael) — כותבת תוכן
Triggers עברית: שכתב, ערוך, נסח מחדש, תרגם, סכם, מאמר, תוכן, פוסט
Triggers English: rewrite, edit, rephrase, translate, summarize, article, content, post
כשמזהה trigger: שלח ליעל את ה-path המדויק (Content/filename.md)
```

### פרוטוקול IMAGE_NEEDED (אחרי output מיעל):
1. לכל `{{IMAGE_NEEDED: "<prompt>"}}` — הפעל יובל עם ה-prompt
2. החלף placeholder ב-`![](yuval/outputs/YYYY-MM-DD-slug.png)`
3. שמור `Output/article.md` הסופי
4. `mv Content/article.md Content/Ready/article.md` (Bash)

---

## style-guide.md Template Sections

- טון ואישיות (רשמי / שיחתי / מקצועי וחם / וכו')
- מבנה מאמר (H1, H2, H3, lead paragraph, CTA)
- אורך פסקאות (מקסימום X משפטים)
- אורך משפטים (מקסימום X מילים)
- אוצר מילים מועדף
- מילים/ביטויים להימנע
- איך לפתוח מאמר
- איך לסגור / CTA
- שימוש בדוגמאות ומספרים
- קהל יעד
- שפה (עברית / אנגלית / ערבוב)

---

## Constraints

- יעל **לא מוחקת** קבצים — ממוקה מטפל בארכיב
- יעל **לא מפעילה יובל** — מחזירה placeholders לממוקה
- יעל עובדת על **מאמר אחד** בכל הפעלה
- כל output נשמר ב-`Output/` — לא מחליף את המקור
