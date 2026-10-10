---
name: learn norsk video
description: use this skill when I give you a link to a video in Norwegian and tell you I want to learn its Norwegian. You use this skill to download the subscription and translate it to English. Also use this skill when I paste a Norwegian sentence and say "learn norwegian sentence" — then translate it, teach the new words, and record the sentence.
---

The steps include:

1. Use the following command to download the subtitles (run in /tmp to avoid cluttering the project directory):
cd /tmp && yt-dlp --skip-download --write-subs --sub-format vtt --convert-subs srt "link_I_give_you"
2. Find the downloaded .srt file with: ls /tmp/*.srt
3. Read the .srt file to extract all Norwegian subtitle lines.
4. Translate all the subtitles to English.
5. Display each sentence both in Norwegian and English. Line by line, one line in Norwegian and one line in English.
6. Read /Users/shuangwu/Claude-Norsk-Learning/vocabulary.txt to get the list of already-known words.
7. Identify new vocabulary from the subtitles (notable words/phrases worth learning). Exclude any words already present in vocabulary.txt. Display only the new words at the end.
8. Save a transcript file to /Users/shuangwu/AItools/LearnNorsk/<episode-title>.txt with:
   - The episode title as a heading
   - Every Norwegian sentence in **bold**, followed by its English translation on the next line
   - A "New Words" section at the end listing: **norsk ord** — English meaning
9. Append the new words to /Users/shuangwu/Claude-Norsk-Learning/vocabulary.txt in the format:
   norsk ord | English meaning
   (one word/phrase per line)

Note: yt-dlp and ffmpeg must be installed (brew install yt-dlp ffmpeg). ffmpeg is required for the vtt-to-srt conversion step.
Note: The episode title for the filename should be derived from the .srt filename or the video title — use only safe characters (replace spaces with underscores, remove brackets).

---

## Practice Mode (step 10)

After updating vocabulary.txt, ask the user:
> "Do you want to practice what you learned? / Vil du øve på det du lærte?"

If the user says yes, enter Practice Mode. Rules for Practice Mode:

- **Communicate only in Norwegian** for the entire practice session. Give instructions, corrections, and encouragement all in Norwegian. You may add a short English note in parentheses only when correcting, to clarify the mistake.
- Run 8–10 rounds, mixing the two exercise types below. Alternate between them so the session stays varied.

### Exercise type 1 — Translate to Norwegian
- Pick an English sentence from the episode transcript.
- Present it to the user like: "Oversett til norsk: '[English sentence]'"
- Wait for the user's answer.
- If correct (or close enough): praise in Norwegian, e.g. "Bra jobbet! / Riktig! / Flott!"
- If wrong: show the correct Norwegian sentence, briefly explain the error in Norwegian (with English in parentheses if needed), then move on.

### Exercise type 2 — Fill in the blank
- Pick a Norwegian sentence from the episode that contains a new word OR a word from vocabulary.txt.
- Replace that word with "___".
- Present it like: "Fyll inn blanken: '[sentence with ___ (English meaning)]'"
- Wait for the user's answer.
- If correct: praise in Norwegian.
- If wrong: reveal the answer and briefly explain in Norwegian.

### Ending the session
- Practice continues indefinitely — there is no fixed round limit.
- Keep alternating exercise types and drawing from the episode's sentences and vocabulary.
- Stop only when one of these happens:
  1. The user sends a new video link → process the new video (start from step 1), then offer practice again.
  2. The user says "stop" or "stop practice norwegian" (in any language) → give a short closing summary in Norwegian (how many correct total, encouragement), then switch back to normal English communication.
- Do NOT end the session on your own after a fixed number of rounds.

---

## Sentence-by-Sentence Mode

After saving the transcript and updating vocabulary, also offer this mode:
> "Vil du lese setning for setning? / Do you want to read sentence by sentence?"

The user can also enter this mode at any time by saying "sentence by sentence" or by pointing to a transcript .txt file in /Users/shuangwu/AItools/LearnNorsk/.

### How it works

1. Read the transcript .txt file from /Users/shuangwu/AItools/LearnNorsk/. Each bold Norwegian line followed by its English translation is one sentence pair.
2. Read /Users/shuangwu/Claude-Norsk-Learning/vocabulary.txt to know which words the user already knows.
3. Start from the first Norwegian sentence. For each sentence:
   a. Show the **Norwegian sentence** (in bold) AND its English translation together.
   b. Below it, list any words/phrases in that sentence that are NOT in vocabulary.txt, with their English meaning. Format: `- **word** — meaning`
   c. Wait for the user. The user may ask questions about the sentence (grammar, meaning, context, word usage). Answer their questions.
   d. Any short input (a single letter, empty message, "ja", "next", etc.) means "go to next sentence." Only pause if the user asks a question (contains a question mark, or is clearly a question about meaning/grammar).
4. Continue through the entire transcript in order.

### Vocabulary from questions

When the user asks about a word during the session (e.g. "hva betyr X?"), after explaining the word, also append it to /Users/shuangwu/Claude-Norsk-Learning/vocabulary.txt in the standard format:
   word | English meaning
This way, words the user actively asks about are captured as learned vocabulary.

### Ending the session
- If the user says "stop" or "stopp" (in any language), end the session with a short encouraging summary of how many sentences they covered.
- If the user reaches the end of the transcript, congratulate them and offer Practice Mode.

---

## Single Sentence Mode

Triggered when the user pastes a Norwegian sentence together with "learn norwegian
sentence" (or a close variant). No video is involved. Handle each sentence like this:

1. **Translate** the sentence to English. Show it as:
   - **Norwegian sentence** (bold)
   - English translation on the next line
2. **Read** /Users/shuangwu/Claude-Norsk-Learning/vocabulary.txt to see which words the
   user already knows.
3. **Identify new words** — any word or phrase in the sentence that is NOT already in
   vocabulary.txt and is worth learning. List them as: `- **ord** — English meaning`
   Skip trivial function words (og, i, på, er, det, en/ei/et, …) unless the user asks.
   If every word is already known, say so — that is a win worth pointing out.
   Add a short grammar or usage note when the sentence contains something notable
   (word order, a separable verb, a fixed expression, a tricky preposition).
4. **Append the new words** to /Users/shuangwu/Claude-Norsk-Learning/vocabulary.txt in the
   standard format, one per line:
   ord | English meaning
5. **Record the sentence** in a sentence log file in this repo (see below).

### Sentence log files

Sentence logs live in the `sentences/` directory of the Claude-Norsk-Learning repo
(/Users/shuangwu/Claude-Norsk-Learning/sentences/). Each file holds **50 sentences**:

- `sentences-001.md` → sentences 1–50
- `sentences-002.md` → sentences 51–100
- and so on

Before appending, find the highest-numbered file and count the entries in it. If it
already has 50, create the next file with the standard header. Otherwise append to it.
Sentence numbering is continuous across files — sentence 51 is the first entry in
`sentences-002.md`, and its heading still says `## 51`.

Each entry is recorded as:

```
## <number>

**<Norwegian sentence>**
<English translation>

New words:
- **ord** — English meaning
```

If the sentence produced no new words, write `New words: none` instead of the list.

New log files start with this header:

```
# Norwegian Sentences <first>–<last>

Sentences I asked about, with translations and the new vocabulary from each.
```

### Multiple sentences at once

If the user pastes several sentences in one message, process each one in turn and give
each its own number and its own entry in the log.
