# Claude Norsk Learning

A collection of Claude Code skills to help me learn Norwegian.

## Skills

### 1. learn-norsk-video (`SKILL.md`)

Downloads subtitles from a Norwegian video, translates them to English, and turns
the episode into learning material.

**What it does:**

1. Downloads the subtitles with `yt-dlp` and converts them to `.srt`
2. Shows every sentence line by line — one line Norwegian, one line English
3. Reads `vocabulary.txt` to see which words are already known, and lists only the
   genuinely new words from the episode
4. Saves a transcript to `~/AItools/LearnNorsk/<episode-title>.txt` (bold Norwegian
   sentences + translations, with a "New Words" section)
5. Appends the new words to `~/AItools/LearnNorsk/vocabulary.txt` as
   `norsk ord | English meaning`

**Practice Mode:** After the transcript is saved, Claude offers a practice session
run entirely in Norwegian. It alternates between "translate this English sentence
to Norwegian" and fill-in-the-blank exercises drawn from the episode. The session
runs until you send a new video link or say "stop".

**Sentence-by-Sentence Mode:** Walk through a saved transcript one sentence at a
time. Each sentence is shown with its translation plus any words not yet in
`vocabulary.txt`. You can ask questions about grammar or meaning at any point —
words you ask about get added to `vocabulary.txt` automatically. Enter it by
saying "sentence by sentence" or pointing at a transcript file.

**Single Sentence Mode:** Paste a Norwegian sentence and say "learn norwegian
sentence". Claude translates it, checks it against `vocabulary.txt`, teaches only the
words that are actually new (adding them to the vocabulary list), and records the
sentence in this repo under `sentences/`. Log files hold 50 sentences each —
`sentences-001.md` covers 1–50, `sentences-002.md` covers 51–100, and so on.

**Works with:** NRK videos (e.g. tv.nrk.no)

**Requirements:** `yt-dlp` and `ffmpeg` (install via `brew install yt-dlp ffmpeg`).
ffmpeg is needed for the vtt-to-srt conversion.

**Usage:** Give Claude a link to an NRK video and say you want to learn Norwegian
from it.
