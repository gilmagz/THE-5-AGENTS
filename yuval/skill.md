# gpt-image-gen — Pointer Doc

> הגדרת הסקיל הקנונית נמצאת ב-[`.claude/skills/gpt-image-gen/SKILL.md`](../.claude/skills/gpt-image-gen/SKILL.md)  
> זהו pointer doc לבני אדם בלבד.

---

## מה הסקיל עושה

`gpt-image-gen` הוא המעטפת הטכנית לקריאת OpenAI Images API.  
יובל (ושאר הסוכנים) משתמשים בסקיל הזה — לא קוראים ל-API ישירות.

## דרישות

- `OPENAI_API_KEY` מוגדר ב-`.env` בשורש הפרויקט
- `curl` זמין (Git Bash / WSL / Linux)
- `jq` או `python3` לdecode של base64

## תיעוד מלא

ראה [`.claude/skills/gpt-image-gen/SKILL.md`](../.claude/skills/gpt-image-gen/SKILL.md)
