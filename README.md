# <img src="pics/clapperboard.svg" alt="" height="30"> YouTube Watchlist Organizer: a Claude Skill for tracking chosen channels

![Claude skill](https://img.shields.io/badge/Claude_skill-D97757?style=for-the-badge)
![Prompt Engineering](https://img.shields.io/badge/Prompt_Engineering-1F4E79?style=for-the-badge)
![AI Automation](https://img.shields.io/badge/AI_Automation-2E7D32?style=for-the-badge)

**Your chosen YouTube channels, in a feed you control: no algorithm, no Shorts, just new videos.**

A Claude skill: a Markdown file with instructions that Claude follows. Pick the YouTube channels or playlists you want to follow, and it gives you a short text list of their latest videos, with the new ones flagged, right in your Claude chat.

## 📸 Example

Example output of `/youtube-watchlist food`:

<table><tr><td>

**Latest videos: Chef Giacomo**

Most recent: Is it worth paying more for these famous turrones? | Food review · [link](https://www.youtube.com/watch?v=dhlK8c-BtpY)

- 🆕 Is it worth paying more for these famous turrones? | Food review · published 1 day ago · [link](https://www.youtube.com/watch?v=dhlK8c-BtpY)
- 🆕 Why your fried chicken always turns out dry: the secret to juiciness · published 3 days ago · [link](https://www.youtube.com/watch?v=S3ZKjVm4e4s)
- Is the most famous turrón the best? | Turrón showdown · published 8 days ago · [link](https://www.youtube.com/watch?v=oGR-C4jlpgc)
- The Fridays ribs you always wanted to make: the real recipe at home · published 10 days ago · [link](https://www.youtube.com/watch?v=Hmk9TY3LdwQ)
- My grandfather received the city medal: the most emotional day of my life in Tacna | Vlog · published 13 days ago · [link](https://www.youtube.com/watch?v=aXhPoMPiqEA)

</td></tr></table>



## ✨ Features

- Reads a channel or playlist from a short Markdown file and fetches its RSS feed.
- Lists the 5 most recent videos with title, publish date and link.
- Flags new videos with 🆕: the last 7 days by default, or any number of days you add after the channel name (`food 3`).
- Translates titles that are not in English.
- Stores no state: "new" is calculated against today's date on every run.
- Skips Shorts.

## ⚙️ How it works

1. Each channel or playlist is saved as a short Markdown file in `channels/`, with its name, topic and RSS feed:
   ```markdown
   ## Channel info

   - Name: Chef Giacomo
   - Topic: cooking
   - RSS feed: https://www.youtube.com/feeds/videos.xml?channel_id=UCZB2JfwFvccwQdpTwgE5idA
   ```
2. In Claude, the user types `/youtube-watchlist food` or simply asks *"any new videos from the food channel?"*
3. Following the instructions in `SKILL.md`, Claude reads the feed, skips Shorts, flags the new videos, translates the titles and replies with a list like the one above.

Adding a new channel only takes a new Markdown file, with no code changes.

## 🛠️ Tech stack

- Claude Skills
- Prompt Engineering
- YouTube RSS feeds
- Markdown

## 📁 Project structure

- `.claude/skills/youtube-watchlist/SKILL.md` — skill instructions: read the feed, skip Shorts, format the output
- `.claude/skills/youtube-watchlist/channels/` — one file per channel or playlist (`food`, `travel`, `comedy`)
