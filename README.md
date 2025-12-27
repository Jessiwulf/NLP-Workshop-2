# Regular Expressions (Regex) Explanation

## 1. HTML Tag Removal
**Code:** `re.sub(r'<[^>]*>', ' ', text)`

**Translation:** "Find any text that starts with `<` and ends with `>`, and replace it with a space."

| Symbol | Function |
| :--- | :--- |
| **`<`** | Matches the starting angle bracket. |
| **`[^>]*`** | Matches any character that is **NOT** a closing bracket (`>`), repeated zero or more times. |
| **`>`** | Matches the closing angle bracket. |

* **Before:** `"This movie<br />was terrible."`
* **After:** `"This movie was terrible."`

---

## 2. Grammar Fixer (The "Space Inserter")
**Code:** `re.sub(r'(?<=[.,!?])(?=[a-zA-Z])', ' ', text)`

**Translation:** "Find the **position** right after a punctuation mark and right before a letter, and insert a space there."

This uses **Lookarounds** (Zero-width assertions). It doesn't consume characters; it only checks boundaries.

| Symbol | Name | Function |
| :--- | :--- | :--- |
| **`(?<=...)`** | **Positive Lookbehind** | Checks if the previous character matches the set `[.,!?]`. |
| **`(?=...)`** | **Positive Lookahead** | Checks if the next character matches a letter `[a-zA-Z]`. |

* **Why?** Many reviews have typos like `"movie.It"` which confuse NLP models.
* **Before:** `"I loved the film.It was great."`
* **After:** `"I loved the film. It was great."`

---

## 3. Smart Special Character Cleaning
**Code:** `re.sub(r'[^a-zA-Z0-9\s.,!?\'"]', '', text)`

**Translation:** "Delete any character that is **NOT** a letter, number, space, or standard punctuation mark."

Unlike standard cleaning (which deletes everything), this "Whitelist" approach preserves sentence structure.

| Symbol | Function |
| :--- | :--- |
| **`^`** | When inside `[...]`, it means **NOT**. Matches everything *except* what follows. |
| **`a-zA-Z`** | Matches all English letters. |
| **`0-9`** | Matches numbers. |
| **`\s`** | Matches whitespace (spaces, tabs). |
| **`.,!?\'"`** | **The Whitelist:** Explicitly keeps dots, commas, exclamation marks, question marks, and quotes. |

* **Before:** `"Wow!!! It cost $100 & was 100% fun :)"`
* **After:** `"Wow!!! It cost 100  was 100 fun "` (Note: `&`, `$`, `%` are removed; `!` is kept).

---

## 4. Extra Space Removal
**Code:** `re.sub(r'\s+', ' ', text)`

**Translation:** "Find any sequence of one or more spaces and replace them with a single space."

| Symbol | Function |
| :--- | :--- |
| **`\s`** | Matches whitespace (space, tab, newline). |
| **`+`** | Quantifier meaning "one or more times". |

* **Before:** `"Movie    was  good."`
* **After:** `"Movie was good."`

---

### 5. Sentence Splitting (For Manual Score)
**Code:** `re.split(r'[.!?]+', text)`

**Translation:** "Split the text into a list whenever you see one or more periods, exclamation marks, or question marks."

| Symbol | Function |
| :--- | :--- |
| **`[...]`** | Character set matching dot, exclamation, or question mark. |
| **`+`** | Matches one or more (handles multiple marks like `..` or `?!`). |

* **Input:** `"Really?! I didn't know."`
* **Output List:** `["Really", " I didn't know", ""]`

---

## 6. Emoticon Escaping
**Code:** `re.escape(emo)`

**Translation:** "Automatically add backslashes `\` before special characters in a string so Regex treats them as text, not commands."

* **Input:** `":)"`
* **Output:** `"\:\)"` (Tells regex to look for a literal colon and parenthesis, not a group).

---
