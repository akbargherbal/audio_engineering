# context.md — Shared Memory Across Sessions

This file is the project's persistent memory between sessions. It records key findings, decisions, workflow rules, and open questions. **Any AI assistant/agent running via OpenCode MUST read this file at the start of every session before taking any action.**

---

## 1. Project Goal & Philosophy

- **Goal:** Generating music for classical Arabic poetry using AI (Suno), fusing Western orchestral/rock arrangements with Eastern maqam-based vocal performance (excluding Eastern instruments, screaming, and Western-style noise/chaos).
- **Philosophy:** **"A journey, not a destination."** The primary focus is **learning audio engineering fundamentals through practical debugging**, using Elissa's "Maktouba Leek" as the gold-standard reference track.
- **No Premature Prompt Patching:** Do NOT propose or jump to Suno prompt edits unless explicitly requested by the project owner. The current focus is understanding _why_ the reference track sounds clear/pure and translating ear perception into measurable metrics.

---

## 2. Arabic Communication Protocol

- OpenCode's terminal interface does **not** support RTL or Arabic text rendering. To work around this:
  - **User → AI:** Write Arabic instructions in `user_input.txt`. The AI reads this file and acts on it.
  - **AI → User:** Write Arabic responses in `llm_input.txt`. The AI replies in English in the chat, and writes any Arabic content inside `llm_input.txt`.
  - English can still be used in either file when convenient, but `llm_input.txt` is primarily for Arabic output from the AI.
- Both files live at the project root (`user_input.txt`, `llm_input.txt`).

## 3. Core Interaction Rules

- **Do NOT ask open-ended or technical "expert" preference questions.** The project owner is a hobbyist driven by taste and ear perception, not audio engineering theory.
- **Ask specific, concrete questions:** Point to an exact timestamp/file and ask a simple, plain-language question about what he hears (e.g., _"In section X, is the vocal in front or buried?"_).
- **Connect ears to numbers:** Let the user describe what he hears in plain language first, then write Python scripts to measure and validate that perception. Numbers serve the ears, not vice versa.

---

## 4. Local Environment & OpenCode Workflow

- **Execution Environment:** Running locally via **OpenCode CLI**.
- **Direct File System Access:** All code edits, script creations, and updates to `context.md` take effect directly in the local repository. No zip archiving or manual file transfers needed.
- **Terminal & Python Execution:** OpenCode can directly run terminal/bash commands, execute Python scripts inside the active virtual environment (`venv`), install packages via `pip`, and perform Git commits/operations locally.
- **Off-Machine / Heavy Compute:** If a task requires heavy RAM or GPU (e.g., training or running large source-separation ML models), OpenCode should provide ready-to-run Google Colab notebooks or GCP scripts with clear instructions.
- **Audacity Integration:** The project owner uses Audacity locally. OpenCode should suggest Audacity manual steps when they are more efficient than scripting, providing simple step-by-step guidance.

---

## 5. Reference Track & File Caveats

- **Gold Reference Track:** Elissa — "Maktouba Leek" (`data/elisa_maktooba_leek.mp3`).
- **Isolated Stems Available:** `data/elisa_stems/elisa_maktooba_leek-Instrumental.mp3` and `-Vocals.mp3`.
- **⚠️ CRITICAL CAVEAT (.srt Timing):** Do NOT rely strictly on `.srt` timestamps for sub-second audio analysis. `.srt` files have a known time offset (~3-4 seconds). Always verify onset timing by ear or audio waveform analysis first.

---

## 6. Key Findings to Date

- **Debunked Hypothesis ("Swell Clash"):** Earlier assumptions (Lab 04) that backing swells clashing with vocal entrances cause poor quality were proven incorrect when tested against the Elissa reference track.
- **Current Working Hypothesis ("Vocal Prominence / Level Gap"):**
  - Real Elissa stems show active vocal sits **~+2.6 dB median above the instrumental**.
  - Proxy center-channel estimations show Suno tracks (SONG_A/C) sit **~2.7–3.0 dB lower** in relative vocal prominence compared to Elissa.
  - _Conclusion:_ The quality gap is primarily a mix level/headroom issue ("authority" and loudness of the vocal relative to backing), not a failure of Suno's vocal synthesis.
- **Track Lineage Confound:** `data/يا دار مية - 28-07-2026.mp3` is the original base take. SONG_A, B, C, and D are **"covers"** generated from this base with varying parameters (audio influence kept low at ~25%). They are not independent fresh prompts.

---

## 7. Session & Lab Directory Structure

- Every session writes its temporary analysis scripts, outputs, and plots inside a dedicated session folder: `LABS/session_<YYYY-MM-DDTHHMMSS>/`.
- Only major, validated conclusions and new protocols from a session get promoted up into this `context.md` file.
