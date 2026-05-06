---
name: gpt-image-gen
description: "Use when any agent needs to generate an image via OpenAI Images API (gpt-image-2). Wraps the curl call, base64 decode, and file save. Requires OPENAI_API_KEY in .env."
---

# gpt-image-gen — Image Generation Skill

## Overview

מעטפת לקריאת OpenAI Images API (`gpt-image-2`). כל סוכן שצריך ליצור תמונה משתמש בסקיל הזה — לא קורא ל-API ישירות.

---

## Prerequisites

`OPENAI_API_KEY` חייב להיות מוגדר ב-`.env` בשורש הפרויקט.

```bash
# .env
OPENAI_API_KEY=sk-...
```

טען אותו לפני הקריאה:

```bash
source .env   # Git Bash / bash
# או
export $(grep -v '^#' .env | xargs)
```

---

## API Call

### Primary Method — curl + jq

```bash
curl -s -X POST "https://api.openai.com/v1/images/generations" \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gpt-image-2",
    "prompt": "<the prompt>",
    "size": "1024x1024",
    "quality": "medium",
    "output_format": "png"
  }' | jq -r '.data[0].b64_json' | base64 --decode > <output-path>.png
```

### Python Fallback — כשאין jq (Git Bash / Windows)

```bash
curl -s -X POST "https://api.openai.com/v1/images/generations" \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gpt-image-2",
    "prompt": "<the prompt>",
    "size": "1024x1024",
    "quality": "medium",
    "output_format": "png"
  }' | python3 -c "
import json, base64, sys
d = json.load(sys.stdin)
b64 = d['data'][0]['b64_json']
open('<output-path>.png', 'wb').write(base64.b64decode(b64))
"
```

---

## Parameters

| Parameter | Value | Notes |
|-----------|-------|-------|
| `model` | `gpt-image-2` | קבוע — אל תשנה |
| `prompt` | string | מלא, תיאורי, באנגלית |
| `size` | `1024x1024` | ברירת מחדל; אפשרויות: `1024x1024`, `1024x1536`, `1536x1024` |
| `quality` | `medium` | `low` / `medium` / `high` |
| `output_format` | `png` | קבוע |

---

## Output Convention

שמור תמיד לפי הפורמט:

```
<directory>/outputs/<YYYY-MM-DD>-<slug>.png
<directory>/outputs/<YYYY-MM-DD>-<slug>.txt   ← prompt ששימש (לאיטרציה)
```

**דוגמה:**
```
yuval/outputs/2026-05-06-product-launch-hero.png
yuval/outputs/2026-05-06-product-launch-hero.txt
```

---

## Validation

אחרי כל שמירה — וודא:

```bash
# קובץ קיים + גודל > 0
[ -s "<output-path>.png" ] && echo "OK" || echo "ERROR: file missing or empty"
```

בפייתון:
```python
import os
assert os.path.getsize("<output-path>.png") > 0, "Image file is empty or missing"
```

---

## Error Handling

| שגיאה | סיבה | פתרון |
|-------|------|--------|
| `401 Unauthorized` | מפתח API חסר / שגוי | בדוק `OPENAI_API_KEY` ב-`.env` |
| `400 Bad Request` | prompt ריק / פרמטר לא חוקי | בדוק את שדות ה-JSON |
| `null` מה-jq | תגובת API שגויה | הדפס את ה-raw response לדיבאג |
| קובץ ריק | jq / base64 נכשל בשקט | השתמש ב-Python fallback |

**דיבאג — הדפס raw response:**
```bash
curl -s ... -d '...' | tee /tmp/img_response.json
```
