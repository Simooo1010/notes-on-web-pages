# SPEC — Notes on Web Pages (DRAFT, not approved)

Status: DRAFT. The owner must reply "Spec approved" before any product code is written (WORKFLOW §2.6).

## 1. Goal
A Chrome extension (Manifest V3) for desktop Chrome. The user adds sticky notes (post-its) to any web page. A note can be moved, tilted and edited. Text can be highlighted inside a note. Notes stay in the browser and come back after a page reload.
v1 is done when the owner can create, move, tilt, write in and highlight a post-it on any page, reload the page, see the post-it again, and see no console errors.

## 2. Data
No external data system. Notes are stored locally with `chrome.storage.local`, keyed by page URL (origin + path, no hash).

| Field | Type | Notes |
|---|---|---|
| id | string | unique per note |
| x, y | number | position in page coordinates |
| rotation | number | degrees, tilt |
| color | string | one of a fixed palette |
| text | string | note text |
| highlights | array | ranges of highlighted text inside the note |

## 3. Features
- Create a post-it on the current page (toolbar popup button and context menu).
- Drag to move. A handle rotates the note (tilt).
- Edit text in place. Highlight selected text with a marker color.
- Optional 3D look (paper curl, shadow, perspective tilt) with CSS only.
- Delete a note.
- Render inside a Shadow DOM so page styles do not change the note.

## 4. Edge cases
- Pages with strict CSP, iframes, and `chrome://` pages (not supported; the extension does nothing there).
- Page layout changes after reload: the note keeps its absolute page position.
- Storage quota: show a clear error, never lose existing notes.

## 5. Build order
1. **Step 1 — Scaffold:** manifest, content script, storage layer, lint/test setup. Check: extension loads without errors.
2. **Step 2 — Post-it create/move/tilt:** Check: owner creates, moves and tilts a note; reload keeps it.
3. **Step 3 — Text and highlights.** Check: owner writes and highlights text; reload keeps both.
4. **Step 4 — 3D look and polish; QA gates 3–5.**

## 6. Workflow settings
- **Playbook source/ref:** https://github.com/Simooo1010/claude-project-playbook at 3b4b7ae0c292237cc436d6aa58166e90ee377393 (copy in `docs/`).
- **Profile:** Standard.
- **Approval mode:** Strict (default).
- **Owner:** Simone.
- **Test device(s):** to decide (proposal: desktop Chrome, current stable).
- **Allowed external writes:** none.
- **Approved dependencies:** none. Proposal: vanilla TypeScript or JavaScript, with Vitest and ESLint as development tools only. Needs owner approval.
- **Production merge:** owner approval required.
- **Project commands:** not defined yet (set in Step 1).

## 7. Decision log
| Date | Decision | Decided by | § |
|---|---|---|---|
| 2026-11-19 | Draft spec written from the Notion task brief; awaiting approval | worker | all |

## 8. Open questions for the owner
1. Approve this spec? (Standard profile blocks code until approved.)
2. Approve dependencies: TypeScript, Vitest, ESLint (dev only), or plain JavaScript?
3. Should notes be per exact URL, or per site (origin)? Proposal: per exact URL.
4. Which test device and Chrome version?
