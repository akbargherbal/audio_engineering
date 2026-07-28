# context.md — Persistent Project Memory & Knowledge Base

This file stores the project's cumulative audio engineering findings, reference track data, track lineage, and active research hypotheses. **Read this file at the start of every session to load current project state.**

---

## 1. Project Reference Data

- **Gold Reference Track:** Elissa — "Maktouba Leek" (`data/elisa_maktooba_leek.mp3`).
  - **Isolated Stems:** `data/elisa_stems/elisa_maktooba_leek-Instrumental.mp3` and `-Vocals.mp3`.
- **Primary Suno Test Track:** "Ya Dar Maya" (`data/يا دار مية - 28-07-2026.mp3`).

---

## 2. Critical Audio Data Caveats

- **`.srt` Timestamp Offset:** `.srt` files have a known ~3-4 second timing offset. **Never trust `.srt` timestamps for sub-second audio analysis.** Always verify onset timing by ear or waveform analysis first.
- **Track Lineage (Cover Confound):** `data/يا دار مية - 28-07-2026.mp3` is the original base take. Tracks `SONG_A`, `SONG_B`, `SONG_C`, and `SONG_D` are **covers** generated from this base take with low audio influence (~25%). They are NOT independent fresh prompt generations.

---

## 3. Key Findings to Date

- **Debunked Hypothesis — "Swell Clash" (Lab 04):**
  - _Earlier belief:_ Backing orchestral swells clashing with vocal entrances cause poor quality.
  - _Finding:_ Testing against the Elissa reference track showed low-mid energy and stereo width actually _increase_ at vocal entrance. What happens in professional mixes is compositional "call-and-response," not a frequency crash.

- **Current Working Hypothesis — "Vocal Prominence / Level Gap"  [CONFIRMED Session 2]:**
  - _Ground Truth Measurement (Elissa Stems):_ When active, Elissa's vocal sits **~+2.6 dB median above the instrumental track**.
  - _Proxy Estimation (All Suno Tracks):_ Mid/side proxy measurements show ALL four Suno tracks (A, B, C, D) and the original "Ya Dar Maya" track sit **2.1–3.1 dB lower** in centered-content prominence than Elissa's full mix on the same metric.
  - _Breakdown by Track:_
    - Ya Dar Maya (orig): gap = -2.10 dB
    - SONG_A: gap = -2.74 dB
    - SONG_B: gap = -3.06 dB
    - SONG_C: gap = -3.06 dB
    - SONG_D: gap = -2.61 dB
  - _2-5 kHz Presence Band:_ All Suno tracks have EQUAL or HIGHER energy ratio in 2-5 kHz (5.4%–6.9%) compared to Elissa (5.1%). The issue is NOT a lack of high-frequency presence — it's purely a **level balance** problem.
  - _Conclusion:_ The perceived quality gap ("lack of vocal authority/presence") is primarily a **mix level / headroom issue**, not a failure of Suno's vocal synthesis nor a lack of high-frequency content.

---

## 4. Active Research Questions & Next Steps

1. ~~**Vocal Level Verification:** Verify if the ~3 dB vocal prominence gap holds true across `SONG_B` and `SONG_D`.~~ **DONE** — confirmed across all four covers (2.6–3.1 dB gap).
2. ~~**Frequency Presence Analysis:** Beyond raw loudness (dB), what specific frequency bands (e.g., 2 kHz – 5 kHz presence boost) give Elissa's vocal its centered clarity?~~ **DONE** — not a frequency presence issue; Suno tracks have equal/higher 2-5 kHz energy.
3. **Reverb & Spatial Separation:** How does the stereo width and reverb density of the backing track differ between Elissa and Suno during vocal passages?
4. **Actionable Remix:** Now that we've confirmed the ~3 dB vocal prominence gap, the next step is to TEST if boosting the vocal/center channel by ~3 dB in a Suno track actually makes it sound as "authoritative" as Elissa. This would validate the hypothesis by intervention, not just measurement.
