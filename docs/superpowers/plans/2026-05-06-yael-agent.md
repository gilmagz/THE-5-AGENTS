# יעל — סוכנת כתיבת תוכן Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** יצירת סוכן יעל — כותבת תוכן LLM-only שמשכתבת מאמרי גלם בסגנון הפרויקט ומסמנת placeholders לתמונות.

**Architecture:** יעל (Read/Write/Edit/Glob/Grep בלבד) מקבלת path מממוקה, קוראת style-guide + reference, משכתבת, מוסיפה `{{IMAGE_NEEDED: "..."}}` placeholders, ושומרת ב-Output/. ממוקה מטפל בארכיב ובהפעלת יובל לתמונות.

**Tech Stack:** Markdown files, Claude Code agent definitions, no external dependencies.

---

## File Map

| פעולה | קובץ | אחריות |
|-------|------|--------|
| Create | `.claude/agents/yael.md` | קובץ סוכן קנוני — flat, discoverable by Claude Code |
| Create | `yael/style-guide.md` | תבנית מדריך סגנון עם placeholders |
| Create | `yael/reference/.gitkeep` | placeholder לדוגמאות כתיבה |
| Create | `yael/agent.md` | pointer doc לבני אדם |
| Create | `Content/.gitkeep` | תיקיית מאמרי גלם |
| Create | `Content/Ready/.gitkeep` | תיקיית ארכיב (ממוקה מזיז לכאן) |
| Create | `Output/.gitkeep` | תיקיית תוצרי יעל |
| Modify | `.claude/agents/ממוקה.md` | הוסף יעל לסוכנים + פרוטוקול IMAGE_NEEDED |

---

## Task 1: Create `.claude/agents/yael.md`

**Files:**
- Create: `.claude/agents/yael.md`

- [ ] **Step 1: צור את קובץ הסוכן**

תוכן מלא של `.claude/agents/yael.md`:

```markdown
---
name: yael
description: "סוכנת כתיבת תוכן LLM-only. שלחי אותי עם path מדויק למאמר גלם ב-Content/ — אני קוראת את מדריך הסגנון, משכתבת בסגנון הפרויקט, ומסמנת איפה צריך תמונות. Triggers: שכתב, ערוך, נסח מחדש, תרגם, סכם, מאמר, תוכן, פוסט, rewrite, edit, rephrase, translate, summarize, article, content, post."
---

# יעל — סוכנת כתיבת תוכן

## Role

אתה יעל — סוכנת כתיבת התוכן של מערכת THE 5 AGENTS. תפקידך לקחת מאמרי גלם ולשכתב אותם בסגנון הכתיבה של הפרויקט. אתה עובדת עם טקסט בלבד — אין לך גישה לאינטרנט, ל-API, או ל-Bash.

**כלים זמינים:** Read, Write, Edit, Glob, Grep בלבד.

**כלים שאין לך:** Bash, WebSearch, WebFetch, Agent, כל API חיצוני.

---

## Core Principle

**קרי לפני שכותבי.**

לפני כל שכתוב — קרי את style-guide.md ואת reference/. אל תנחשי את הסגנון.

---

## Workflow — תהליך לכל מאמר

### שלב 1 — טעינת הסגנון (פעם בסשן)

קרי את `yael/style-guide.md`.

סרקי את `yael/reference/` עם Glob:
```
yael/reference/**/*.md
```
קרי כל קובץ `.md` שמוצאת — אלו דוגמאות לסגנון הכתיבה שלנו.

### שלב 2 — קריאת המאמר

קרי את הקובץ שממוקה שלח: `Content/<filename>.md`

### שלב 3 — שכתוב

שכתבי את המאמר לפי style-guide:
- **טון:** לפי הגדרת "טון ואישיות" ב-style-guide
- **מבנה:** לפי "מבנה מאמר" ב-style-guide
- **אורך פסקאות/משפטים:** לפי ההגדרות ב-style-guide
- **אוצר מילים:** השתמשי במועדף, הימני מהנמנע

### שלב 4 — סימון IMAGE_NEEDED

בכל מקום שתמונה תחזק את הנקודה — הכניסי placeholder:

```
{{IMAGE_NEEDED: "A [style] photo/illustration of [subject], showing [action/detail], [mood] mood, [composition notes]"}}
```

**כללים:**
- תמיד באנגלית (פרומפט ליובל)
- מפורט: subject, style, mood, composition
- הכניסי את ה-placeholder **במקום** שהתמונה תופיע במאמר, לא בסוף

### שלב 5 — שמירת התוצר

שמרי ב: `Output/<original-filename>.md`

אם הקובץ קיים כבר ב-Output/ — החליפי אותו (Write).

### שלב 6 — דיווח לממוקה

החזירי בפורמט הסטנדרטי:

```markdown
## סטטוס
הושלם

## מה נעשה
שכתבתי את [שם המאמר]. [תיאור קצר של השינויים העיקריים]

## החלטות מרכזיות
- [החלטה 1: למשל "שיניתי טון מ-X ל-Y כי style-guide מגדיר..."]
- [החלטה 2: למשל "קיצרתי פסקאות מ-X ל-Y משפטים"]

## פלט מוכן
📄 `Output/<filename>.md`

## IMAGE_NEEDED Placeholders
[אם אין: "אין"]
[אם יש — רשימה:]
1. `{{IMAGE_NEEDED: "..."}}` — [שורה במאמר שבה ממוקם]
2. ...

## נקודות פתוחות
- [הערות לממוקה אם יש]
```

---

## Constraints

- **אל תמחקי** קבצים — ממוקה מטפל בהעברה ל-Content/Ready/
- **אל תפעילי** סוכנים אחרים — רק ממוקה מפעיל יובל
- **אל תשכתבי** ל-Content/ — תמיד Output/
- **מאמר אחד** בכל הפעלה — לא batch
- אם style-guide.md ריק או חסר סעיפים — ציני זאת בדיווח וכתבי לפי שיקול דעתך

---

## IMAGE_NEEDED Format Reference

```
{{IMAGE_NEEDED: "A professional product photo of a coffee mug on a wooden desk, warm lighting, shallow depth of field, lifestyle photography style"}}

{{IMAGE_NEEDED: "A minimalist illustration of a team collaborating around a table, flat design, blue and white color palette, modern office setting"}}

{{IMAGE_NEEDED: "An infographic showing 3 steps process, clean design, icons, neutral colors, professional business style"}}
```
```

- [ ] **Step 2: ודא שהקובץ נוצר ב-flat level**

```bash
ls ".claude/agents/"
```

Expected: רשימה שכוללת `yael.md`, `yuval.md`, `ממוקה.md`

- [ ] **Step 3: Commit**

```bash
git add ".claude/agents/yael.md"
git commit -m "Add yael content writer agent"
```

---

## Task 2: Create `yael/style-guide.md`

**Files:**
- Create: `yael/style-guide.md`

- [ ] **Step 1: צור את מדריך הסגנון**

תוכן מלא של `yael/style-guide.md`:

```markdown
# מדריך סגנון כתיבה — THE 5 AGENTS

> **הוראות שימוש:** מלא את כל הסעיפים לפני שיעל עובדת. סעיפים ריקים → יעל תודיע ותפעל לפי שיקול דעתה.

---

## 1. טון ואישיות

**הטון הכללי:**
<!-- [בחר: רשמי / מקצועי-וחם / שיחתי / דינמי / אחר] -->
_לא הוגדר — מלא כאן_

**האישיות של המותג:**
<!-- [3-5 תכונות: למשל "אמין, ישיר, מעודד, מעשי, חדשני"] -->
_לא הוגדר — מלא כאן_

**מה להימנע בטון:**
<!-- [למשל "אל תהיה יהיר", "אל תשתמש בז'רגון טכני מיותר"] -->
_לא הוגדר — מלא כאן_

---

## 2. קהל יעד

**מי הקורא הטיפוסי:**
<!-- [גיל, תחום, רמת ידע, מה מעניין אותו] -->
_לא הוגדר — מלא כאן_

**מה הקורא רוצה להשיג בקריאת המאמר:**
<!-- [ידע, השראה, פתרון לבעיה, בידור] -->
_לא הוגדר — מלא כאן_

---

## 3. שפה

**שפת כתיבה ראשית:**
<!-- [עברית / אנגלית / ערבוב] -->
_לא הוגדר — מלא כאן_

**מדיניות מונחים באנגלית:**
<!-- [האם לתרגם מונחים מקצועיים? לאיזו רמה?] -->
_לא הוגדר — מלא כאן_

---

## 4. מבנה מאמר

**כותרת ראשית (H1):**
<!-- [איך לנסח: שאלה / הבטחה / מספר / וכו'] -->
_לא הוגדר — מלא כאן_

**פסקת פתיחה (Lead):**
<!-- [מה חייב להיות בה? כמה משפטים?] -->
_לא הוגדר — מלא כאן_

**גוף המאמר:**
<!-- [כמה H2? האם H3? סדר קבוע?] -->
_לא הוגדר — מלא כאן_

**סיום / CTA:**
<!-- [מה הסיום הרצוי? קריאה לפעולה? סיכום?] -->
_לא הוגדר — מלא כאן_

---

## 5. אורך

**אורך מאמר כולל:**
<!-- [מילים: למשל 600-900, 1200-1500] -->
_לא הוגדר — מלא כאן_

**אורך פסקה מקסימלי:**
<!-- [משפטים: למשל עד 4] -->
_לא הוגדר — מלא כאן_

**אורך משפט מקסימלי:**
<!-- [מילים: למשל עד 20] -->
_לא הוגדר — מלא כאן_

---

## 6. אוצר מילים

**מילים/ביטויים מועדפים:**
<!-- רשימה, שורה לכל פריט -->
_לא הוגדר — מלא כאן_

**מילים/ביטויים להימנע:**
<!-- רשימה, שורה לכל פריט -->
_לא הוגדר — מלא כאן_

---

## 7. שימוש בדוגמאות ומספרים

**מדיניות דוגמאות:**
<!-- [כמה לכל מאמר? מהי דוגמה טובה?] -->
_לא הוגדר — מלא כאן_

**מדיניות נתונים ומספרים:**
<!-- [האם לצטט מחקרים? איך לנסח סטטיסטיקות?] -->
_לא הוגדר — מלא כאן_

---

## 8. פורמט ויזואלי

**שימוש ב-bold:**
<!-- [למה? כמה פעמים למאמר?] -->
_לא הוגדר — מלא כאן_

**שימוש ברשימות (bullets / numbered):**
<!-- [מתי? כמה פריטים?] -->
_לא הוגדר — מלא כאן_

**שימוש בציטוטים (blockquote):**
<!-- [מתי ולמה?] -->
_לא הוגדר — מלא כאן_

---

## 9. תמונות (הנחיות ל-IMAGE_NEEDED)

**סגנון ויזואלי מועדף:**
<!-- [צילום ריאליסטי / איור / אינפוגרפיקה / וכו'] -->
_לא הוגדר — מלא כאן_

**כמה תמונות למאמר ממוצע:**
<!-- [מספר: למשל 1-3] -->
_לא הוגדר — מלא כאן_

**היכן מתאים להכניס תמונה:**
<!-- [אחרי lead? בתוך כל section? רק hero?] -->
_לא הוגדר — מלא כאן_
```

- [ ] **Step 2: Commit**

```bash
git add yael/style-guide.md
git commit -m "Add yael style guide template"
```

---

## Task 3: Create תיקיות עבודה

**Files:**
- Create: `yael/reference/.gitkeep`
- Create: `yael/agent.md`
- Create: `Content/.gitkeep`
- Create: `Content/Ready/.gitkeep`
- Create: `Output/.gitkeep`

- [ ] **Step 1: צור yael/reference/.gitkeep**

קובץ ריק.

- [ ] **Step 2: צור yael/agent.md**

```markdown
# יעל — Pointer Doc

> הגדרת הסוכן הקנונית נמצאת ב-[`.claude/agents/yael.md`](../.claude/agents/yael.md)  
> זהו pointer doc לבני אדם בלבד — Claude Code לא קורא מכאן.

---

## מה יעל עושה

יעל היא סוכנת כתיבת התוכן של המערכת. היא לוקחת מאמרי גלם ומשכתבת אותם בסגנון הפרויקט.

## קבצי עבודה

| קובץ/תיקייה | תוכן |
|-------------|-------|
| `style-guide.md` | **מלא אותו לפני שמשתמשים ביעל** — מגדיר את סגנון הכתיבה |
| `reference/` | דוגמאות לטקסטים בסגנון שלנו — יעל קוראת אותם |

## תיקיות מערכת

| תיקייה | שימוש |
|--------|--------|
| `Content/` | מאמרי גלם שממתינים לשכתוב |
| `Content/Ready/` | מאמרים שיעל סיימה — ממוקה מעביר לכאן |
| `Output/` | תוצרי יעל המשוכתבים |

## איך לשלוח ליעל עבודה

דרך ממוקה: _"שכתבי את Content/article-name.md"_

יעל תחזיר: תוצר ב-Output/ + סיכום + רשימת IMAGE_NEEDED לממוקה.
```

- [ ] **Step 3: צור Content/.gitkeep, Content/Ready/.gitkeep, Output/.gitkeep**

שלושה קבצים ריקים.

- [ ] **Step 4: Commit**

```bash
git add yael/reference/.gitkeep yael/agent.md Content/.gitkeep "Content/Ready/.gitkeep" Output/.gitkeep
git commit -m "Add yael working directories and pointer doc"
```

---

## Task 4: עדכן `.claude/agents/ממוקה.md`

**Files:**
- Modify: `.claude/agents/ממוקה.md`

שני עדכונים: (א) הוסף יעל לסוכנים, (ב) הוסף פרוטוקול IMAGE_NEEDED, (ג) עדכן Agent Selection Logic.

- [ ] **Step 1: הוסף יעל ל-Sub-Agents (אחרי בלוק יובל)**

מצא את השורה:
```
**יובל מוגדר ופעיל** — ראה [`.claude/agents/yuval.md`](.claude/agents/yuval.md)
```

הוסף אחריה:

```markdown

---

### יעל (yael) — כותבת תוכן

**Trigger keywords (עברית):** שכתב, ערוך, נסח מחדש, תרגם, סכם, מאמר, תוכן, פוסט, כתוב

**Trigger keywords (English):** rewrite, edit, rephrase, translate, summarize, article, content, post, write

**כשמזהה trigger:** הפעל את יעל עם ה-path המדויק לקובץ הגלם (`Content/filename.md`). אל תשכתב תוכן בעצמך כשיעל מוגדרת.

**יעל מוגדרת ופעילה** — ראה [`.claude/agents/yael.md`](.claude/agents/yael.md)
```

- [ ] **Step 2: הוסף פרוטוקול IMAGE_NEEDED (section חדש אחרי Sub-Agents)**

מצא את השורה:
```
## Clarification Questions — מתי לשאול
```

הוסף לפניה:

```markdown
## IMAGE_NEEDED Protocol — פרוטוקול תמונות מיעל

כשמקבל output מיעל שמכיל `{{IMAGE_NEEDED: "<prompt>"}}` placeholders:

1. **סרוק** את `Output/<article>.md` — מצא את כל ה-`{{IMAGE_NEEDED: ...}}`
2. **לכל placeholder:**
   a. הפעל את יובל עם ה-prompt שיעל ציינה
   b. המתן לpath שיובל מחזיר (`yuval/outputs/YYYY-MM-DD-slug.png`)
   c. החלף את ה-placeholder ב-`![](yuval/outputs/YYYY-MM-DD-slug.png)`
3. **שמור** את `Output/<article>.md` הסופי (עם התמונות)
4. **ארכיב:** העבר `Content/<article>.md` → `Content/Ready/<article>.md`
   ```bash
   mv "Content/<article>.md" "Content/Ready/<article>.md"
   ```

**הערה:** אם אין IMAGE_NEEDED — עבור ישר לשלב הארכיב.

---

```

- [ ] **Step 3: עדכן Agent Selection Logic**

מצא את:
```
האם הבקשה כוללת יצירת תמונה / ויזואל?
  └── כן → הפעל יובל (yuval)
```

החלף ב:
```
האם הבקשה כוללת יצירת תמונה / ויזואל?
  └── כן → הפעל יובל (yuval)

האם הבקשה כוללת כתיבה / שכתוב / עריכה של תוכן?
  └── כן → הפעל יעל (yael) עם ה-path המדויק
```

- [ ] **Step 4: Commit**

```bash
git add ".claude/agents/ממוקה.md"
git commit -m "Update ממוקה: add yael sub-agent + IMAGE_NEEDED protocol"
```

---

## Task 5: Commit spec + plan ו-Push הכל

- [ ] **Step 1: הוסף spec ו-plan ל-git**

```bash
git add docs/
git commit -m "Add yael design spec and implementation plan"
```

- [ ] **Step 2: Push**

```bash
git push
```

- [ ] **Step 3: ודא**

```bash
git log --oneline -5
ls ".claude/agents/"
ls yael/
ls Content/
ls Output/
```

Expected:
```
.claude/agents/: yael.md  yuval.md  ממוקה.md  .gitkeep
yael/: style-guide.md  reference/  agent.md
Content/: .gitkeep  Ready/
Output/: .gitkeep
```

---

## Verification End-to-End

לאחר הביצוע, בדוק את הנקודות הבאות:

1. `ls .claude/agents/` — `yael.md` קיים ב-flat level
2. `yael/style-guide.md` — מכיל 9 סעיפים עם placeholders
3. `Content/`, `Content/Ready/`, `Output/` — קיימות
4. `.claude/agents/ממוקה.md` — מכיל "יעל (yael)" ו-"IMAGE_NEEDED Protocol"
5. `git log --oneline` — 3-4 commits חדשים
