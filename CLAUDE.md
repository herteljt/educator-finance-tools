# Educator Finance Tools — Project Instructions

## What This Project Is

A collection of free, browser-based tools to help K–12 teachers understand their salary, benefits, and financial options. Built by Josh Hertel as part of the Personal Finance for Educators project at https://personalfinanceforeducators.substack.com

Tools live in the GitHub repo: https://github.com/herteljt/educator-finance-tools
Deployed at: https://herteljt.github.io/educator-finance-tools

## When Asked to Build a Tool

Build a single self-contained HTML file. No frameworks, no build steps, no dependencies except Google Fonts and (if the tool needs AI) the Google Gemini SDK loaded via `https://esm.run/@google/generative-ai`. The file must work when opened from a local HTTP server or GitHub Pages.

Name the file `tool-[short-name].html` (e.g. `tool-salary-calculator.html`, `tool-retirement-estimator.html`).

---

## Required Meta Tags

Every tool must include these four meta tags in `<head>`. They are read by the GitHub Action that auto-updates `tools.json` and the homepage.

```html
<meta name="tool-name" content="Tool Name Here">
<meta name="tool-description" content="One sentence description for the homepage card.">
<meta name="tool-icon" content="📄">
<meta name="tool-tags" content="tag1, tag2, tag3">
```

---

## Required Header Structure

Every tool uses this exact header. Replace TOOL NAME and VERSION as appropriate. Start at v1.0 and increment by 0.01 for each update.

```html
<header>
  <div class="header-left">
    <h1>Tool Name</h1>
    <div class="subtitle">Educator Finance Tools · v1.0</div>
  </div>
  <div class="header-right">
    <a class="back-link" href="index.html">← All Tools</a>
  </div>
</header>
```

If the tool has a document/file status indicator, add `<div id="doc-status">No document loaded</div>` inside `.header-right` before the back link.

---

## Design System

### Fonts
```html
<link href="https://fonts.googleapis.com/css2?family=Lora:ital,wght@0,400;0,600;1,400&family=DM+Mono:wght@400;500&display=swap" rel="stylesheet">
```
- Body text: `'Lora', Georgia, serif`
- Monospace labels, badges, inputs: `'DM Mono', monospace`

### CSS Variables
```css
:root {
  --ink:          #1c1c2e;
  --paper:        #f4f0e8;
  --cream:        #ece7da;
  --white:        #ffffff;
  --accent:       #8b1a1a;
  --accent-light: #f0dede;
  --muted:        #7a7262;
  --border:       #d4cebe;
  --green:        #2d5a3d;
  --green-light:  #d8ece1;
}
```

### Layout
```css
body {
  font-family: 'Lora', Georgia, serif;
  background: var(--paper);
  color: var(--ink);
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}
main {
  flex: 1;
  display: flex;
  flex-direction: column;
  max-width: 820px;
  width: 100%;
  margin: 0 auto;
  padding: 28px 24px 60px;
  gap: 20px;
}
```

### Header CSS
```css
header {
  background: var(--ink);
  padding: 14px 28px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  border-bottom: 3px solid var(--accent);
  flex-shrink: 0;
}
.header-left { display: flex; flex-direction: column; gap: 2px; }
header h1 { color: var(--paper); font-size: 1.1rem; font-weight: 600; letter-spacing: 0.02em; }
.subtitle { font-family: 'DM Mono', monospace; font-size: 0.62rem; color: #5a5a7a; letter-spacing: 0.1em; text-transform: uppercase; }
.header-right { display: flex; align-items: center; gap: 14px; }
.back-link { font-family: 'DM Mono', monospace; font-size: 0.68rem; color: #5a5a7a; text-decoration: none; letter-spacing: 0.06em; transition: color 0.15s; }
.back-link:hover { color: var(--paper); }
```

### Panels
Content is organized into panels with a labeled header.

```css
.panel { background: var(--white); border: 1px solid var(--border); border-radius: 4px; overflow: hidden; }
.panel-header { background: var(--cream); border-bottom: 1px solid var(--border); padding: 11px 20px; display: flex; align-items: center; gap: 10px; }
.panel-header h2 { font-size: 0.88rem; font-weight: 600; }
.panel-body { padding: 20px; }
.step-badge { font-family: 'DM Mono', monospace; font-size: 0.62rem; background: var(--ink); color: var(--paper); padding: 2px 8px; border-radius: 2px; letter-spacing: 0.06em; flex-shrink: 0; }
```

```html
<div class="panel">
  <div class="panel-header">
    <span class="step-badge">STEP 1</span>
    <h2>Section Title</h2>
  </div>
  <div class="panel-body">
    <!-- content -->
  </div>
</div>
```

### Buttons
```css
button { font-family: 'Lora', serif; font-size: 0.85rem; padding: 10px 18px; border: none; border-radius: 3px; cursor: pointer; transition: opacity 0.15s, transform 0.1s; white-space: nowrap; }
button:active { transform: scale(0.98); }
button:disabled { opacity: 0.38; cursor: not-allowed; }
.btn-primary { background: var(--ink); color: var(--paper); }
.btn-primary:hover:not(:disabled) { opacity: 0.82; }
.btn-accent { background: var(--accent); color: white; }
.btn-accent:hover:not(:disabled) { opacity: 0.85; }
.btn-ghost { background: transparent; border: 1px solid var(--border); color: var(--muted); }
.btn-ghost:hover:not(:disabled) { border-color: var(--ink); color: var(--ink); }
```

- `btn-primary` — dark background, main actions
- `btn-accent` — red, primary CTA (submit, analyze, calculate)
- `btn-ghost` — outlined, secondary actions (reset, back)

### Inputs
```css
input[type="text"], input[type="number"], input[type="password"], textarea {
  font-family: 'DM Mono', monospace;
  font-size: 0.82rem;
  padding: 10px 14px;
  border: 1px solid var(--border);
  border-radius: 3px;
  background: var(--paper);
  color: var(--ink);
  outline: none;
  transition: border-color 0.15s;
}
input:focus, textarea:focus { border-color: var(--ink); }
```

### Status Lines
```css
.error-line { font-family: 'DM Mono', monospace; font-size: 0.74rem; color: var(--accent); margin-top: 8px; min-height: 18px; }
.success-line { font-family: 'DM Mono', monospace; font-size: 0.74rem; color: var(--green); margin-top: 6px; min-height: 18px; }
```

### Loading Animation
```css
.dots { display: flex; gap: 5px; }
.dot { width: 7px; height: 7px; background: var(--muted); border-radius: 50%; animation: bounce 1.2s ease-in-out infinite; }
.dot:nth-child(2) { animation-delay: 0.2s; }
.dot:nth-child(3) { animation-delay: 0.4s; }
@keyframes bounce { 0%,60%,100% { transform: translateY(0); opacity: 0.4; } 30% { transform: translateY(-5px); opacity: 1; } }
```

```html
<div class="dots"><div class="dot"></div><div class="dot"></div><div class="dot"></div></div>
```

---

## Tools That Use the Gemini API

Some tools use Google Gemini AI for analysis or Q&A. Use this pattern:

```javascript
async function callGemini(prompt, pdfBase64 = null) {
  const { GoogleGenerativeAI } = await import('https://esm.run/@google/generative-ai');
  const genAI = new GoogleGenerativeAI(apiKey);
  const model = genAI.getGenerativeModel({
    model: document.getElementById('model-select').value,
    generationConfig: { temperature: 0.3, maxOutputTokens: 1500 }
  });

  const parts = [];
  if (pdfBase64) {
    parts.push({ inlineData: { mimeType: 'application/pdf', data: pdfBase64 } });
  }
  parts.push({ text: prompt });

  const result = await model.generateContent({ contents: [{ role: 'user', parts }] });
  const text = result.response.text();
  if (!text) throw new Error('Empty response from Gemini.');
  return text;
}
```

### API Key UI Pattern
Always use this Step 1 panel for API key entry. The key validation checks for `AIza` prefix and minimum length. Enabling the action button happens in `saveKey()` if a file/input is already ready, and in the file/input handler if the key is already set.

```html
<div class="panel">
  <div class="panel-header">
    <span class="step-badge">STEP 1</span>
    <h2>Enter Your Gemini API Key</h2>
  </div>
  <div class="panel-body">
    <div class="key-instructions">
      <p>Free tier available — no credit card needed.</p>
      <ol>
        <li>Go to <a href="https://aistudio.google.com/app/apikey" target="_blank">aistudio.google.com/app/apikey</a></li>
        <li>Sign in with any Google account</li>
        <li>Click <strong>Create API key</strong> and copy it</li>
        <li>Paste it below — stored in this browser tab only</li>
      </ol>
    </div>
    <div class="input-row">
      <input type="password" id="api-key-input" placeholder="AIzaSy…" autocomplete="off" />
      <button class="btn-primary" onclick="saveKey()">Save Key</button>
    </div>
    <div class="error-line" id="key-error"></div>
    <div class="success-line" id="key-success"></div>
    <div class="model-row">
      <label style="font-family:'DM Mono',monospace;font-size:0.72rem;color:var(--muted);white-space:nowrap;letter-spacing:0.04em;">MODEL</label>
      <select id="model-select" style="flex:1;font-family:'DM Mono',monospace;font-size:0.78rem;padding:9px 12px;border:1px solid var(--border);border-radius:3px;background:var(--paper);color:var(--ink);outline:none;cursor:pointer;">
        <option value="gemini-2.5-flash-lite" selected>gemini-2.5-flash-lite — fastest, most generous free tier (recommended)</option>
        <option value="gemini-2.5-flash">gemini-2.5-flash — more capable, free tier available</option>
        <option value="gemini-2.5-pro">gemini-2.5-pro — most capable, limited free quota</option>
      </select>
    </div>
  </div>
</div>
```

### saveKey() function
```javascript
function saveKey() {
  const input = document.getElementById('api-key-input');
  const val   = input.value.trim();
  const errEl = document.getElementById('key-error');
  const okEl  = document.getElementById('key-success');
  errEl.textContent = '';
  okEl.textContent  = '';
  if (!val) { errEl.textContent = 'Please paste your API key.'; return; }
  if (!val.startsWith('AIza')) { errEl.textContent = 'Key should start with "AIza". Copy it directly from Google AI Studio.'; return; }
  if (val.length < 20) { errEl.textContent = 'That key looks too short — make sure you copied the full key.'; return; }
  apiKey = val;
  input.value = '●'.repeat(16);
  okEl.textContent = '✓ Key saved.';
  // Enable action button if inputs are already ready
  checkReady();
}
```

Use a `checkReady()` function to enable the primary action button only when all required inputs are present (key + any required user inputs).

---

## Versioning

- Start every new tool at `v1.0`
- Increment by `0.01` for each update (v1.01, v1.02, etc.)
- Version appears in the subtitle: `Educator Finance Tools · v1.0`

---

## Existing Tools

| File | Name | Description |
|------|------|-------------|
| `tool-handbook-analyzer.html` | Handbook Analyzer | Upload a district handbook PDF, get a structured summary and Q&A chat using Gemini AI |

---

## Repo Structure

```
educator-finance-tools/
├── index.html                          # Homepage — reads tools.json, renders cards
├── tools.json                          # Auto-updated by GitHub Action
├── tool-handbook-analyzer.html         # Tool files named tool-*.html
├── tool-[next-tool].html
└── .github/
    └── workflows/
        └── update-tools.yml            # Scans tool-*.html, rebuilds tools.json on push
```

The GitHub Action reads these meta tags from each `tool-*.html` to populate `tools.json`:
- `tool-name`
- `tool-description`
- `tool-icon`
- `tool-tags`

---

## Audience & Tone

Tools are for K–12 teachers, often not financially sophisticated. Use plain language. Avoid jargon. If a concept needs explaining (e.g. 403(b), FMLA), add a brief plain-English note inline. Forms should be forgiving — show helpful error messages, not just validation failures.

---

## What to Do When Asked to "Build a Tool"

1. Confirm the tool name and purpose
2. Ask any clarifying questions needed (what inputs, what outputs, does it need AI?)
3. Build a single `tool-[name].html` file following all conventions above
4. Include all four meta tags
5. Use the standard header with correct version
6. Follow the design system exactly — same fonts, colors, panel structure, button styles
7. Name the file `tool-[short-name].html`
