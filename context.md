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

- **Current Working Hypothesis — "Vocal Prominence / Level Gap":**
  - _Ground Truth Measurement (Elissa Stems):_ When active, Elissa's vocal sits **~+2.6 dB median above the instrumental track**.
  - _Proxy Estimation (Suno Tracks):_ Center-channel proxy measurements show Suno tracks (`SONG_A` and `SONG_C`) sit **~2.7–3.0 dB lower** in relative vocal prominence compared to Elissa.
  - _Conclusion:_ The perceived quality gap ("lack of vocal authority/presence") is primarily a **mix level / headroom issue**, not a failure of Suno's vocal synthesis.

---

## 4. Active Research Questions & Next Steps

1. **Vocal Level Verification:** Verify if the ~3 dB vocal prominence gap holds true across `SONG_B` and `SONG_D`.
2. **Frequency Presence Analysis:** Beyond raw loudness (dB), what specific frequency bands (e.g., 2 kHz – 5 kHz presence boost) give Elissa's vocal its centered clarity?
3. **Reverb & Spatial Separation:** How does the stereo width and reverb density of the backing track differ between Elissa and Suno during vocal passages?
