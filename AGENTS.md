# Minto documentation — authoring instructions

Read this before writing or editing any page.

## About this project

- Mintlify site. Pages are MDX with YAML frontmatter; config is `docs.json`.
- `mint dev` previews locally. `mint broken-links --check-anchors` and
  `mint a11y` must both pass before merging.
- Audience: **placement agent staff** — coordinators and operations people who
  arrange international student internships. Non-technical, often reading English
  as a second language. Write for them, not for developers.

## Terminology

Use the product's words, with no synonyms. Vocabulary drift between the UI, the
docs and the sales deck is how a docs site stops being trustworthy.

| Use | Never |
|---|---|
| intern | student, candidate, participant |
| host firm | company, employer, workplace |
| sending organization | partner, school, agency |
| internship | placement, programme, project |
| placement (an intern at a host firm) | assignment, posting |
| field of expertise | industry, sector, category |

"Placement agent" is the customer — the organization using Minto. Address them
as **you**, never as "the user" or "the coordinator".

## Style rules

Derived from the Google developer documentation style guide and the Microsoft
Writing Style Guide. Where they disagree, the choice is noted.

| Rule | Do | Don't |
|---|---|---|
| Person | "you" | "the user", "the coordinator" |
| Mood | imperative: "Select **Save**." | "You can select Save." |
| Tense | present: "The status changes to…" | "The status will change to…" |
| Verb | **Select** — input-agnostic | Click, tap, press, hit |
| Checkbox | "Clear the **X** checkbox." | "Uncheck X." |
| Condition first | "To export the list, select **Export**." | "Select **Export** if you want to export." |
| UI label | bold, sentence case, no trailing `…` | "the **Save as…** button" |
| Control type | omit unless it adds clarity | "the **Firms** tab button" |
| Icons | by label: "Select **Save**." | "Select the bell icon." |
| Direction | "the following table" | "the table below", "on the right" |
| Headings | sentence case, verb first, no end period | "How to use the…" |
| Errors | real, searchable text | a screenshot of the error |
| Language | short sentences, plain words | idioms, humour, "commence" |

We follow Microsoft over Google on **Select** rather than **Click**: Minto is
used on laptops and tablets, "Select" is what screen-reader users experience,
and it translates cleanly. Do not mix the two.

Contractions are fine and preferred — "it's", "you'll", "don't".

## Page structure

Every how-to page follows this shape. Do not invent a new section order.

```mdx
---
title: "Reassign a placement to a different host firm"   # verb first, sentence case
description: "One line, keywords front-loaded."
last_reviewed: 2026-07-19
---

One or two sentences: what this does and when you'd do it. This is the "why" —
without it a reader can't tell whether they're on the right page.

## Before you begin

- Which permission or position you need.
- What must already exist.
- What state the record must be in.

## <Verb the task>

<Steps>
  <Step title="…">Location first, then the action.</Step>
</Steps>

## Result

What the reader should now see, so they can confirm it worked.

## Troubleshooting

Symptom in bold, then cause, then fix. Only failures specific to this task.
```

Rules that matter more than the shape:

- **State the permission in "Before you begin".** Minto is multi-tenant with
  per-position permissions; "who can do this" is the single most common reason a
  reader is on the wrong page.
- **Location before action** — "In the sidebar, select **Interns**." not
  "Select **Interns** in the sidebar."
- **Mark optional steps** by starting them with "Optional: ".
- **Include the final save.** Procedures that omit the last confirm step are a
  reliable source of support tickets.
- **Cap procedures at about 7 steps.** Longer means splitting the page.

### Reference pages

Reference pages (statuses, phases, roles, how firms are recommended) explain a
concept rather than walk through a task. They follow a different shape:

- **Title is a noun phrase** naming the thing explained — "Internship statuses",
  "Roles and permissions" — not verb-first like a how-to.
- **No "Before you begin", "Steps", or "Result".** Sections describe facets of
  the concept, in whatever order reads best.
- **Explain, don't instruct.** The moment a reference page turns into numbered
  steps, it's a how-to — move it, and link to it instead.
- End with **Troubleshooting** only if there are common confusions to clear up.

The style rules and terminology above apply unchanged.

### Templates

Start a new page by copying a template rather than from a blank file, so the
shape is right by default:

- `templates/how-to.mdx`
- `templates/reference.mdx`

They live under `templates/` (excluded from the build) and carry the section
headers plus fill-in guidance. Delete the guidance comments as you write.

## Screenshots

The governing rule:

> **Every screenshot must be deletable without loss of instruction.**

Images aren't translated, aren't searchable, and aren't read by screen readers.
If deleting an image would leave a step incomprehensible, the step is
under-written — fix the text first, then keep the image as reinforcement. This
also means screenshot rot is a cosmetic problem rather than a correctness one.

- **Only include a screenshot when the element is genuinely hard to find.** Not
  for "Select **Save**".
- Screenshots live in `/images/screens/` and are **generated, not hand-made**.
  Never edit them. Regenerate from the MINTO repo with:
  ```bash
  dotnet run --project Minto.Api -- seed-docs
  pnpm --dir Minto/minto-web run docs:shots
  ```
- To find out whether the product has drifted away from the committed
  screenshots, run the same thing with `docs:shots:check`. It captures to a
  temporary directory, compares, and names the pages that embed anything that
  moved — so you learn which pages to reread, not just that pixels changed. It
  exits non-zero on drift, so it can gate a merge.
- Wrap in `<Frame>` with a caption. Click-to-zoom is on by default, so a tightly
  cropped image is still readable.
- **Alt text**: 155 characters or fewer, names the screen and what's being
  pointed at, ends with a period, never starts with "Image of".
- Never bake explanatory text into an image.
- Never screenshot production data. The demo tenant is the only source.

```mdx
<Frame caption="The internships list, showing placement progress per internship.">
  <img
    src="/images/screens/internships-list.png"
    alt="The Minto internships list showing four internships with status, sending organization, and assigned interns."
  />
</Frame>
```

## Content boundaries

- Document **jobs**, not screens. The nav is organised by what someone is trying
  to get done. A page called "The Internships screen" is a reference page, not a
  how-to — don't let one masquerade as the other.
- Don't document `/admin` routes. Those are Minto-internal, not customer-facing.
- Don't invent behaviour. If you're unsure what the product does, check the code
  or ask — a confidently wrong doc is worse than a missing one.
