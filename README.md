# IR Navigator — PICERL

A living incident response reference tool. Browse phases, actions, and playbooks at the live URL. Update this repository to instantly update what everyone sees.

---

## Live URL

```
https://YOUR-USERNAME.github.io/ir-navigator
```

Replace `YOUR-USERNAME` with your actual GitHub username after setup.

---

## How this works (the short version)

The website loads data from the files in this repository. When you edit a file here and save it (called "committing"), the live page updates automatically within about 60 seconds. No technical skills required beyond knowing how to edit a file in GitHub.

---

## Repository structure

```
ir-navigator/
│
├── index.html              ← The app itself. Do not edit this.
├── index.json              ← Master list of phases and playbooks.
├── categories.json         ← Color-coded category definitions.
│
├── phases/                 ← One folder per PICERL phase.
│   ├── 01-preparation/
│   │   ├── _phase.json     ← Phase name and list of actions.
│   │   ├── PR-001-asset-inventory.json
│   │   └── PR-002-ir-playbook-development.json
│   │
│   ├── 02-identification/
│   │   ├── _phase.json
│   │   ├── ID-001-alert-triage.json
│   │   └── ...
│   │
│   ├── 03-containment/
│   ├── 04-eradication/
│   ├── 05-recovery/
│   └── 06-lessons-learned/
│
└── playbooks/              ← One file per incident playbook.
    ├── phishing.json
    └── ddos.json
```

---

## The golden rule

> **Edit the JSON files to update the site. Never edit `index.html`.**

Everything an analyst would want to change — action names, procedures, URLs, categories, playbook steps — lives in the JSON files. The HTML is just the interface.

---

## Common tasks

### Update the procedure for an existing action

1. Navigate into the correct phase folder (e.g. `phases/03-containment/`)
2. Click the action file you want to update (e.g. `CO-005-email-quarantine.json`)
3. Click the **pencil icon** (✏️) in the top right of the file view
4. Edit the `sections` array — each section has a `title` and a `body`
5. Scroll down, write a short commit message describing your change
6. Click **Commit changes**

The live page updates within about 60 seconds.

**Example action file:**
```json
{
  "id": "CO-005",
  "name": "Email Quarantine",
  "url": "https://your-wiki.com/email-quarantine",
  "category": "email",
  "sections": [
    {
      "id": "s1",
      "title": "Procedure",
      "body": "1. Log into the mail admin portal\n2. Search for the sender domain\n3. Select all matching messages\n4. Move to quarantine folder\n5. Notify the security team lead"
    },
    {
      "id": "s2",
      "title": "Tools",
      "body": "- Exchange Admin Center (EAC)\n- Microsoft 365 Defender portal\n- Ticket system: create ticket type EMAIL-QUARANTINE"
    },
    {
      "id": "s3",
      "title": "Escalation",
      "body": "If quarantine fails or volume exceeds 500 messages, escalate to Tier 2 and open a P1 incident."
    }
  ]
}
```

**Tips for writing the `body` field:**
- Use `\n` to start a new line
- Use `- ` at the start of a line for a bullet point
- Use `1. ` at the start of a line for a numbered step
- Keep it plain text — no special formatting needed

---

### Add a brand new action to a phase

1. Go to the phase folder (e.g. `phases/03-containment/`)
2. Click **Add file → Create new file**
3. Name it following the pattern: `CO-006-my-action-name.json`
   - Use the phase prefix (`CO` for Containment, `PR` for Preparation, etc.)
   - Use the next available number
   - Use hyphens, not spaces
4. Paste this template and fill it in:

```json
{
  "id": "CO-006",
  "name": "Your Action Name",
  "url": "",
  "category": "network",
  "sections": [
    {
      "id": "s1",
      "title": "Procedure",
      "body": "Describe the steps here."
    }
  ]
}
```

5. Commit the file
6. **Important:** Open `_phase.json` in the same folder and add your new filename to the `actions` list:

```json
{
  "id": "t3",
  "name": "Containment",
  "shortcode": "C",
  "url": "",
  "actions": [
    "CO-001-host-isolation.json",
    "CO-002-account-lockout.json",
    "CO-003-network-segmentation.json",
    "CO-004-block-malicious-ips-domains.json",
    "CO-005-email-quarantine.json",
    "CO-006-your-action-name.json"
  ]
}
```

7. Commit `_phase.json`

The action now appears on the live page.

---

### Phase prefixes reference

| Folder | Prefix | Phase |
|--------|--------|-------|
| `01-preparation` | `PR` | Preparation |
| `02-identification` | `ID` | Identification |
| `03-containment` | `CO` | Containment |
| `04-eradication` | `ER` | Eradication |
| `05-recovery` | `RE` | Recovery |
| `06-lessons-learned` | `LL` | Lessons Learned |

---

### Update a playbook

Playbooks define which actions belong to a specific incident type and in what order they should be run.

Open `playbooks/phishing.json`:

```json
{
  "id": "pb1",
  "name": "Phishing",
  "color": "#e94560",
  "techIds": ["ID-001", "CO-005", "ER-003", "LL-001"]
}
```

The `techIds` array lists action IDs in the order they should appear in the playbook. To add an action, add its ID to the list. To remove one, delete it from the list. To reorder, move the IDs around.

---

### Add a new playbook

1. Go to the `playbooks/` folder
2. Click **Add file → Create new file**
3. Name it (e.g. `ransomware.json`) and fill it in:

```json
{
  "id": "pb3",
  "name": "Ransomware",
  "color": "#68d391",
  "techIds": []
}
```

Available colors: `#e94560` (red), `#f6ad55` (amber), `#68d391` (green), `#63b3ed` (blue), `#b794f4` (purple), `#81e6d9` (teal)

4. Commit the file
5. Open `index.json` and add `"playbooks/ransomware.json"` to the `playbooks` array
6. Commit `index.json`

---

### Add or update a category

Open `categories.json`. Each entry looks like this:

```json
{
  "id": "network",
  "label": "Network",
  "color": "#2b6cb0",
  "bg": "#1a365d",
  "text": "#63b3ed"
}
```

- `id` — used in action files (e.g. `"category": "network"`)
- `label` — what appears in the UI
- `color` — the border/accent color (hex)
- `bg` — the background color of the badge
- `text` — the text color inside the badge

To add a new category, copy an existing entry, paste it at the end (before the closing `]`), and change all four fields. Add a comma after the previous entry.

---

### Add a reference link to an action

Open the action file and set the `url` field:

```json
{
  "id": "CO-005",
  "name": "Email Quarantine",
  "url": "https://your-internal-wiki.com/email-quarantine",
  ...
}
```

Clicking the action card on the live page will then open that URL in a new tab.

---

## JSON formatting rules (important)

JSON has strict formatting rules. If you break them, the page will fail to load. Here are the most common mistakes:

**Always use double quotes, never single quotes:**
```json
"name": "Email Quarantine"   ✓
'name': 'Email Quarantine'   ✗
```

**Every item in a list except the last needs a comma:**
```json
"actions": [
  "CO-001-host-isolation.json",
  "CO-002-account-lockout.json",
  "CO-003-network-segmentation.json"
]
```
Note: no comma after the last item.

**The same rule applies to objects inside lists:**
```json
"sections": [
  {
    "id": "s1",
    "title": "Procedure",
    "body": "Step 1..."
  },
  {
    "id": "s2",
    "title": "Tools",
    "body": "Tool list..."
  }
]
```

**If you're unsure your JSON is valid**, paste it into [jsonlint.com](https://jsonlint.com) before committing.

---

## How to check if your change worked

1. After committing, go to the **Actions** tab in GitHub (even though we don't use Actions, GitHub Pages deployment status shows there)
2. Wait for the yellow dot to turn green — usually under 60 seconds
3. Reload the live URL (hard refresh: Ctrl+Shift+R on Windows, Cmd+Shift+R on Mac)

---

## Something broke

**The page shows a load error:** A JSON file has invalid formatting. Check what you last committed and look for missing commas, mismatched quotes, or unclosed brackets.

**My change isn't showing:** Hard refresh the page (Ctrl+Shift+R). GitHub Pages sometimes caches aggressively.

**I accidentally deleted something:** Go to the file in GitHub, click **History**, find the version before your change, and copy the content back.

---

## Who maintains what

| File / Folder | Who edits it |
|---|---|
| `index.html` | Senior engineer only |
| `index.json` | Senior analyst (adding new phases or playbooks) |
| `categories.json` | Senior analyst |
| `phases/*/[action].json` | Any analyst |
| `phases/*/_phase.json` | Senior analyst (when adding/removing actions) |
| `playbooks/*.json` | Any analyst |
