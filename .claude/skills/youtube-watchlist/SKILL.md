---
name: youtube-watchlist
description: Lists the latest videos of a saved YouTube channel or playlist from its public RSS feed, flags uploads from the last 7 days (or a chosen number of days), and translates the titles into English. Use when the user asks for new videos from one of the channels or playlists in the channels/ folder (food, travel, comedy, ...).
argument-hint: <channel file name> [days], e.g. food or food 3
---

# YouTube Watchlist

Checks a YouTube channel or playlist for new uploads and lists its 5 most recent videos, using the public RSS feed (no API key required).

## Input

The user names the channel or playlist to check, optionally followed by a number of days: `$ARGUMENTS`

- First word: the channel or playlist file name (for example `food`).
- Optional number: how many days count as new (for example `food 3`). If there is no number, use **7**. In plain-language requests ("anything new from food in the last 3 days?"), take the number of days from the sentence.

Each channel or playlist has its own file in the `channels/` folder next to this file (for example `channels/food.md`). Read the matching file and use the RSS feed URL in it. If no name is given, or no file matches, list the available files in `channels/` and ask which one to check.

## Steps

### 1. Fetch the feed

Retrieve the RSS/XML content from the `RSS feed` URL in the channel file. A file can describe either:

- **a channel** (`## Channel info`, feed URL with `channel_id=`): all uploads of the channel, or
- **a playlist** (`## Playlist info`, feed URL with `playlist_id=`): only the videos in that playlist.

Both feeds have the same format, so the next steps are the same.

### 2. Parse the entries

Each `<entry>` in the feed contains:

- `<title>`: video title
- `<yt:videoId>`: unique video ID
- `<published>`: publish date and time
- `<link rel="alternate" href="...">`: video URL

The feed is ordered newest first and only holds the last 15 uploads.

### 3. Skip Shorts

Skip any entry whose link contains `/shorts/`. Channels often post many Shorts that only promote their latest video, and they would push the real videos out of the list. Keep the first 5 remaining entries. If fewer than 5 are left, show the ones there are.

### 4. Check recency

For each video, compare its `<published>` date with the current date, using the number of days from the input (7 by default):

- **That many days or less**: mark it as new.
- **More than that**: do not mark it.

This is calculated on every run, so the skill does not need to store anything between runs.

### 5. Translate the titles

If a title is not in English, translate it into English. Show only the English title, not the original.

### 6. Report to the user

Start with the most recent video, whether or not it is new. Then list the videos as Markdown, with 🆕 in front of each new video. Keep the title as plain text and put the URL on the word "link":

```markdown
**Latest videos: <channel or playlist name>**

Most recent: Translated title 1 · [link](url)

- 🆕 Translated title 1 · published 2 days ago · [link](url)
- Translated title 2 · published 9 days ago · [link](url)
- Translated title 3 · published 3 weeks ago · [link](url)
- Translated title 4 · published 1 month ago · [link](url)
- Translated title 5 · published 2 months ago · [link](url)
```

If none of the videos is new, say "No new videos in the last N days." at the top, before the list, with N the number of days used.

Only output the list: no extra commentary, summaries or video descriptions.

## Notes

- No YouTube API key is needed; the RSS feeds are public.
- Summarizing a video's content is out of scope for this skill.
