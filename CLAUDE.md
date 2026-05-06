# CLAUDE.md

This repository holds **quiz templates** and quiz instances built from them. Templates are working quizzes used as scaffolds for new projects. They are **not** to be edited when creating a new quiz — they are starting points to be **copied**.

---

## Available templates

| File | Georgian name | Quiz type |
|------|--------------|-----------|
| `templates/gamoicani-wignebi-ydyt` | გამოიცანი წიგნები ყდით | Guess the book by its cover (image → title) |

New template types may be added to `templates/` as needed. When the user asks to create a quiz, confirm which template to use if more than one exists.

---

## Critical rule: templates are read-only

**Never modify any file inside `templates/` when the user asks to create a new quiz.**

If a request would result in editing a template directly, stop and ask for confirmation. The only valid reasons to edit a template are explicit instructions like:

- "update the template"
- "fix a bug in the template"
- "add a feature to the template"

For everything else — especially anything that sounds like "make a quiz about X", "create a new quiz", "build me a quiz" — the workflow is always **copy first, then modify the copy**.

---

## Repository layout

```
/templates/                        ← read-only template files
  gamoicani-wignebi-ydyt           ← "guess the book by cover" template
/quizzes/                          ← quiz instances, one file each
  <quiz-name>                      ← a copy of a template with placeholders filled in
```

Each template is a single self-contained HTML/CSS/JS file designed to be pasted into a WordPress Custom HTML block. New quizzes are individual files in `/quizzes/`, not subdirectories.

---

## Workflow for creating a new quiz

When the user requests a new quiz:

1. **Confirm the quiz name.** Used as the file name. kebab-case ASCII. e.g. `world-capitals`, `georgian-literature-1`.
2. **Identify the template.** Default to `templates/gamoicani-wignebi-ydyt` unless the user specifies another.
3. **Copy the template** to `/quizzes/<quiz-name>`. Use `cp "templates/<template-name>" "quizzes/<quiz-name>"`.
4. **Modify only the copy.** Fill in every placeholder listed in "Placeholder reference" below.
5. **Verify** the content is complete and correct before finishing.

Never skip step 3. Never edit a template file to "save a step".

---

## Placeholder reference

Every `[PLACEHOLDER]` in the template must be replaced when creating a new quiz. Here is the full list:

**UI labels**
- `[START_BUTTON_TEXT]` — label on the start button (e.g. "ტესტის დაწყება")
- `[QUESTION_HEADING]` — static heading shown above every question image
- `[NEXT_BUTTON_TEXT]` — label on the next button during the quiz
- `[SEE_RESULTS_BUTTON_TEXT]` — label on the button after the last question
- `[CTA_LINK_TEXT]` — link label on the results screen (e.g. "წიგნების ნახვა")
- `[CTA_LINK_URL]` — full URL the results-screen link points to

**Questions (repeat for Q1–Q10)**
- `[QN_IMAGE_URL]` — URL of the question image
- `[QN_ALT_TEXT]` — alt text for the image (e.g. "Question 1")
- `[QN_OPTION_1]`, `[QN_OPTION_2]`, `[QN_OPTION_3]` — the three answer choices
- `correct` field — integer index of the correct option: `0` = first, `1` = second, `2` = third

**Result tiers**
- `[RESULT_LOW_TITLE]` / `[RESULT_LOW_DESCRIPTION]` / `[RESULT_LOW_IMAGE_URL]` — score 0–3
- `[RESULT_MID_TITLE]` / `[RESULT_MID_DESCRIPTION]` / `[RESULT_MID_IMAGE_URL]` — score 4–6
- `[RESULT_HIGH_TITLE]` / `[RESULT_HIGH_DESCRIPTION]` / `[RESULT_HIGH_IMAGE_URL]` — score 7–10

---

## What to replace (content)

These are the parts that change between quizzes. Replace these freely in the copy:

- All placeholders listed above
- Any hardcoded strings used as labels, CTAs, or display copy

---

## What to preserve (structure)

These stay identical across quizzes unless the user explicitly asks to change them:

- File and folder layout
- Component names, props shapes, function signatures
- State management and quiz logic (scoring, navigation, validation)
- Routing
- Styling system, colors, fonts, animations, transitions, CSS classes
- Accessibility markup (aria attributes, semantic tags, focus handling)
- Build tooling: `package.json`, configs, scripts

If the user wants a structural change for one quiz only, that's a signal to ask whether the change should go into the template instead.

---

## Cleaning a template

If the user asks to "clean up the template" or "strip the template":

- Replace question, option, and explanation text with placeholders: `"[Question 1]"`, `"[Option A]"`, `"[Explanation]"`.
- Replace title/intro/result copy with `"[Quiz Title]"`, `"[Intro text]"`, `"[Result message]"`.
- Remove quiz-specific images. Either delete the files and clear `src` attributes (`src=""`), or replace with a single neutral placeholder image referenced by all slots.
- **Do not remove** components, routing, styling, logic, or build files.
- After cleaning, the template must still run and display the empty/placeholder content without errors.

---

## Language

- Code, comments, commit messages, and CLAUDE.md instructions are in **English**.
- Quiz content (what the end user sees) is often in **Georgian**. Preserve whatever language the user provides — do not translate unless asked.
- HTML `lang` attribute should match the quiz content language; update it per quiz.

---

## Naming conventions

- Quiz file: kebab-case ASCII (`georgian-history-quiz`, not `ქართული-ისტორია`).
- Template file: kebab-case ASCII transliteration of the Georgian template name.
- Quiz title (displayed): free-form, any language.

---

## Pre-completion checklist

Before declaring a new quiz done, verify:

- [ ] No file inside `templates/` was modified. (`git status` shows no changes there.)
- [ ] `/quizzes/<quiz-name>` exists and is a copy of the chosen template.
- [ ] Every `[PLACEHOLDER]` has been replaced with real content (grep for `\[[A-Z_]\+\]` to confirm none remain).
- [ ] All image URLs are reachable.
- [ ] The file pastes into WordPress and plays through end-to-end without console errors.

---

## When in doubt, ask

If a request is ambiguous between "make a new quiz" and "edit a template", ask before touching any files. Reverting an edit to a template is more painful than asking one clarifying question.
