---
name: learn norsk video
description: use this skill when I give you a link to a video in Norwegian and tell you I want to learn its Norwegian. You use this skill to download the subscription and translate it to English.
---

The steps include:

1. Use the following command to download the subtitles (run in /tmp to avoid cluttering the project directory):
cd /tmp && yt-dlp --skip-download --write-subs --sub-format vtt --convert-subs srt "link_I_give_you"
2. Find the downloaded .srt file with: ls /tmp/*.srt
3. Read the .srt file to extract all Norwegian subtitle lines.
4. Translate all the subtitles to English.
5. Display each sentence both in Norwegian and English. Line by line, one line in Norwegian and one line in English.

Note: yt-dlp and ffmpeg must be installed (brew install yt-dlp ffmpeg). ffmpeg is required for the vtt-to-srt conversion step.
