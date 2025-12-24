# BrickedUp Save Studio — Features Spec (Source of Truth)

## Core Purpose

BrickedUp Save Studio is a Windows desktop Save & Inventory Editor designed to:

- Open and edit `.sav` files safely.
- Build/roll items using game data.
- Apply items to inventory slots systematically.
- Work offline wherever possible.

---

## Primary Features

### 1) Inventory Manager (Save Workflow)

- Open `.sav` files from disk.
- Decrypt `.sav` → YAML.
- Edit YAML (manual or via tools).
- Encrypt YAML → `.sav`.
- Recent files list + reopen last file option.
- Backups supported (recommended before writing).

**Expected behavior**
- Save operations should never crash the app.
- Errors should be shown as readable toasts with actionable next steps.

---

### 2) Vault Tools (Inventory Operations)

Tools operate on inventory slot ranges and must be safe.
- **Auto-Assign Slots**: Places items sequentially into available slots.
- **Protect Existing Items**: Skip occupied slots when applying.
- **Clear Slots**: Removes items from a selected range.
- **Optimize Inventory**: Compacts inventory by removing gaps.
- **Export Inventory Range**: Export items in a range to JSON.
- **Import Inventory Range**: Import JSON items into a range.
- **Transfer Inventory**: Move items between Backpack/Bank/Vault (macro).

**Expected behavior**
- Range operations must never overwrite protected slots.
- Transfer should support "remove from source" only when explicitly enabled.

---

### 3) Parts Library (API-Backed + Cached)

- Loads parts/game data from local cache.
- Can fetch updated data from API and write cache.
- Supports browsing/searching parts by categories/type.
- Supports sending selected parts to Builder.

**Update policy**
- App may check for updates on startup (lightweight).
- App should only download data when the user clicks:
  - "Download Resources" or "Check for Updates" in Settings.

---

### 4) BrickedUp Builder (Preset → Item Code)

Builder supports building items from selected parts:
- Select base item and parts (manual or from Parts Library).
- Forge an Item Code (decoded).
- Encode item code to serial (base85).
- Send/apply to Inventory Manager pipeline.

**Expected behavior**
- UI language should be "Forge / Encode / Apply Items".
- No "hack/cheat" language.

---

### 5) Brick Roll (Random Roller — MUST BE FINAL-QUALITY)

**Rules**
- Must be family-safe: never mixes item families (e.g., guns with shields).
- Must follow a systematic category order per family.
- Must be no-throw: never crashes the app, ever.
- Must output:
  - Decoded item code (when possible).
  - Encoded serial (when possible).
  - Breakdown / debug summary.

**Family support**
- Weapon, Shield, Grenade, Class Mod, Artifact, Enhancement.
- Family selection supports Auto mode (remembers last selection).

**Unlock all parts**
Toggle exists in Settings.
- Unlock All Parts must be systematic:
  - Expands pools within the SAME family only.
  - Does NOT enable cross-family part mixing.
  - Does NOT “spray random IDs”.
  - Does NOT break roll order.

---

### 6) Offline-first UX

- Local cache is the default source for game/parts data.
- Local Guide works fully offline.
- App should remain usable even if API is down.

---

### 7) Local Guide (Offline Help)

- Built-in guide stored locally (`guides/index.json` + `*.md`).
- Viewable inside the app.
- Markdown-rendered.
- Search supported.

**Optional future**
- Guide update manifest (download updates only on user action).

---

## Settings Features

### Data & Updates
- **Download Resources** (first-time).
- **Check for Updates** (manual).
- (Optional) Toggle: "Check for updates on startup" (check-only, not download).

### BrickedUp Options
- **Unlock all parts** (systematic expansion).
- Offline preference + remote fallback (if applicable).

---

## Safety and Reliability Requirements (Non-negotiable)

- Never crash on malformed input; fail safely with toasts.
- Never silently overwrite user data without backup options.
- Never mix item families in rolling/building.
- All network calls must be optional/non-blocking for core offline functions.

---

## Suggested “Release Notes” Feature List (For GitHub Releases)

- **Inventory Manager**: Decrypt/edit/encrypt `.sav`.
- **Vault Tools**: Range clear/optimize/export/import/transfer.
- **Parts Library**: Cached + manual update.
- **BrickedUp Builder**: Forge → encode → apply.
- **Brick Roll**: Family-safe systematic rolling + unlock-all-parts toggle.
- **Local Guide**: Offline documentation + search.
- **Themes**: Classic / Obsidian / Neon / Rose.

---

## Copilot-Friendly TODO / Roadmap (Optional Section)

- Startup update check (manifest-only) + "Update available" pill in header.
- Onboarding modal (first run).
- Resource manager panel: version, last updated, cache size, reset cache.
- Strict mode in Brick Roll (skip invalid parts instead of widening pool).
- Preset export/import for Builder.