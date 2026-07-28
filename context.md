# context.md — Shared Memory Across Sessions

This file is the project's memory between sessions (each session is stateless). It does NOT contain every detail of the work — only the most important findings, decisions, and open questions. Read it at the start of every session before doing anything else.

## Project Goal
Using AI (Suno) to generate music for classical Arabic poetry, fusing Western orchestral/rock arrangement with Eastern maqam-based vocal performance (excluding Eastern instruments, screaming, and Western-style noise/chaos). The project owner is a hobbyist, not a trained audio engineer — the approach is trial-and-error and ear/taste-driven, not theory-first. **The goal is "a journey, not a destination"**: learning through iteration, not arriving at one final answer.

## Core Rule for Working with the Project Owner
Do NOT ask him open-ended/expert-preference questions ("what should we look at first?"). Ask **specific, concrete questions** instead — point him to an exact timestamp/file and ask a specific yes/no or short-answer question about what he hears, in plain language, no jargon required. Then connect his description to the numbers. The numbers serve the description, not the other way around.

## Main Reference Track
Elissa's "Maktouba Leek" (`data/elisa_maktooba_leek.mp3`) — the focus is vocal clarity, purity, and how centered/present the vocal sits in the mix. Isolated stems now available: `data/elisa_stems/elisa_maktooba_leek-Instrumental.mp3` and `-Vocals.mp3` (separated by the project owner in Audacity/a stem-splitter, pushed to repo 2026-07-28).

## Available Tools
- ffmpeg is installed. librosa/soundfile/numpy/scipy installed via pip (--break-system-packages).
- .srt files exist for every song in data/, with approximate vocal-entrance timing.
- The official Suno prompt-generation script: `scripts/maqam_prompt_generator_v3_entrance_fix.py`.

## IMPORTANT CAVEAT: .srt timing is NOT reliable
The project owner confirmed the .srt files have a consistent **offset of roughly 3-4 seconds** across most/all tracks (not just Elissa) — likely from how they were generated. **Never treat .srt timestamps as ground truth for precise onset analysis.** Use them only as a rough starting point, then confirm/adjust by ear or via audio-based onset detection before doing any fine-grained (sub-second) analysis.

## Key Finding #1 (revised): the "swell clash" hypothesis is likely wrong
An earlier session (Lab 04) hypothesized that Suno vocal quality suffers when an orchestral swell peaks at the exact moment the vocal enters (frequency/stereo clash), and script v3 ("entrance_fix") was built to fix that. Testing this against the real Elissa reference:
- In Elissa's track, low-mid energy (200-500Hz) and stereo width both **increase** at vocal entrance, not decrease — the opposite of what the "clash is bad" hypothesis predicts.
- By ear, there's no sharp dip/drop in the backing track at all. What actually happens: the intro instrumental ends and shifts into a "call and response" role (answering phrases between vocal lines) — a compositional/arrangement pattern, not a mix-clash problem.
- **Conclusion: the Lab 04 "clash" framing does not hold up against a professional reference. It should not be the guiding hypothesis for further prompt tuning.**

## Key Finding #2 (new, current working hypothesis): vocal PROMINENCE, not clash
Ear-testing SONG_A and SONG_C ("Ya Dar Maya" Suno takes) against Elissa revealed a different, more consistent pattern:
- The Suno vocal is **not** buried or overlapping with instruments — no clash.
- But it is consistently **less powerful/commanding ("جهورية")** than Elissa's vocal, even in the same arrangement role.
- Specifically: during the brief pauses between vocal phrases, the backing music comes back up — and reaches roughly the **same level** as the vocal (not louder than it), whereas with Elissa the vocal keeps its felt authority throughout.
- This pattern repeats across both A and C, suggesting it's systemic, not a one-off.
- **Measured (session 2026-07-28, `LABS/.../scripts/vocal_prominence.py`):**
  - Ground truth from real Elissa stems: when the vocal is active, it sits **~+2.6dB above the instrumental** (median, across 1s windows). The vocal is not just "not buried" — it's measurably, consistently louder than the backing.
  - Proxy estimate (center-channel mid/side, NOT a real separation — see script docstring for caveats) for the same relative comparison: Elissa ≈ +10.7dB, SONG_A ≈ +8.0dB, SONG_C ≈ +7.7dB. Both Suno tracks sit **~2.7-3dB lower** than Elissa on this proxy, in the same direction and similar magnitude as the real ground-truth gap.
  - **This gives real numeric support to the "level/headroom" hypothesis over the "baritone can't project" hypothesis** — the gap looks like a mix/prominence issue, not (necessarily) a vocal-generation limitation. Still not conclusive: the proxy is approximate, and no real Suno stems exist to confirm directly.

## Confound: most SONG variants are "covers," not independent generations
Per the project owner: **all of SONG_A/B/C/D are "covers"** (built on / influenced by a prior generation) — only `data/يا دار مية - 28-07-2026.mp3` (no suffix) is the original base take. The cover settings varied (weirdness %, style influence %, audio influence %), but **audio influence was consistently kept low (~25%)** across the covers.
**Implication:** A/B/C/D are not a clean "same prompt, independent draws" comparison set — they're variations built on top of one base render. Any conclusion drawn by comparing them to each other should account for this (e.g. differences between them may partly reflect the cover settings, not just the prompt).

## Project Direction (set 2026-07-28, applies to all future sessions)
The project owner explicitly deprioritized prompt-patching/tuning for now — calling it "premature" at this stage. **Do not propose or jump to prompt edits unless he asks for that specifically.** The current phase is about **learning audio-engineering fundamentals through debugging practice**, using the Elissa reference as the anchor case study: keep asking "why does this sound pure/high-quality to the ear, and can that be translated into something measurable?" — building general debugging/listening strategies, not chasing a fix. Prompt iteration will come later, "when its time comes."

## Division of Labor / Tooling (set 2026-07-28)
- **Claude's sandbox**: fine for standard analysis (librosa, numpy, scipy, ffmpeg, plotting) — use it freely without asking.
- **Compute limits**: the sandbox has no GPU and unknown/modest RAM. If a task would need heavy compute (e.g. large ML models, real stem-separation/source-separation networks, GPU-based processing), **do not force it into the sandbox** — instead, hand the project owner ready-to-run code (e.g. a Colab notebook or a script for a GCP VM) and clearly flag *why* it needs to move off-sandbox. He will run it there himself.
- **Audacity**: the project owner has Audacity and can manually do things like isolate/export specific frequency bands, export stems, apply plugins, etc. Claude should proactively suggest specific Audacity actions when they're the more efficient path (rather than only reaching for Python), and give him clear step-by-step instructions since he is not a specialist.
- **General rule**: use the right tool for the job across the three (sandbox / off-sandbox compute / Audacity), and Claude should be the one recommending which, since the project owner isn't positioned to know this himself.

## Sync Workflow Between Claude's Sandbox and GitHub (set 2026-07-28)
Claude has no push access to the GitHub repo (no credentials). The agreed sync loop is:
1. Claude does work in its sandbox (analysis, scripts, context.md updates) inside the cloned repo.
2. When there's meaningful progress to save, Claude builds a **zip of the repo excluding audio files (`*.mp3`) and any build artifacts/caches (venv, `__pycache__`, `.pyc`, `.DS_Store`, `.git/`)** — i.e. only code, docs, `context.md`, and small text/data files (srt, json, csv) — and gives it to the project owner as a download.
3. The project owner extracts it over his local clone, commits, and pushes to GitHub himself.
4. He tells Claude he's pushed; Claude then runs `git fetch`/`git pull` in the sandbox to re-sync and confirms the sync explicitly (e.g. compares commit hashes) before continuing.
**Do this sync periodically as work accumulates — don't wait to be reminded.** Proactively offer a sync zip after a meaningful chunk of progress, rather than requiring the project owner to ask every time.

## Open Questions / Needs Investigation
- Real numeric test of vocal prominence/level using the Elissa stems (`data/elisa_stems/`) vs. an estimated vocal level in Suno tracks — is the gap actually a level/headroom issue, and how big is it in dB?
- Is there a baritone-register limitation in Suno's vocal generation, independent of mix/level?
- Why do SONG_A/B .srt labels mention instruments (oud, darbuka) that are in the prompt's EXCLUDE list — mislabeling by Suno, or actually audible? (lower priority than the prominence question)
- Given A/B/C/D are covers of one base take with varying influence settings, how much of the observed prominence gap is attributable to the base take itself vs. each cover's specific settings?

## Session Structure
Each session works inside a folder `LABS/session_<YYYY-MM-DDTHHMMSS>/` (scripts, outputs, working notes). Nothing inside LABS/ should be treated as final or permanent memory — only the important summaries get promoted up into this file.
