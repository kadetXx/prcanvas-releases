# prcanvas.review

[![downloads](https://img.shields.io/github/downloads/kadetXx/prcanvas-releases/total?style=flat-square&color=a371f7&label=downloads)](https://github.com/kadetXx/prcanvas-releases/releases/latest) [![latest](https://img.shields.io/github/v/release/kadetXx/prcanvas-releases?style=flat-square&color=2ea043&label=latest)](https://github.com/kadetXx/prcanvas-releases/releases/latest) ![macOS](https://img.shields.io/badge/macOS-Apple%20Silicon-555?style=flat-square&logo=apple&logoColor=white)


https://github.com/user-attachments/assets/39d77a1d-c5ad-435a-897c-ee08e19218dc


*Trial one. Thirty seconds of a PR being read on the canvas; the questions get sharper from here.*

A pull request as a canvas.

**[Download for Mac](https://github.com/kadetXx/prcanvas-releases/releases/latest/download/PRCanvas.dmg)** · Apple Silicon, notarised. Free.

Open a PR and you get one frame per changed function, wired in the order the logic runs,
with a one-line summary on each and a short list of questions to answer before you
approve. You read it with the arrow keys, frame by frame, instead of scrolling a diff
sorted alphabetically by file.

It runs on your Mac, through the GitHub CLI (`gh`) and the coding agent you already use: Claude Code,
Codex, Gemini CLI, Cursor or opencode. There is
no server of ours, no account, and nothing is stored anywhere but GitHub.

Which cuts both ways: every comment already on the PR, from a teammate on github.com or
from a bot, shows up on your canvas as a bubble on its line, and you answer it right
there. Your replies go back to the PR. Nobody has to switch tools for you to use this.

## Install

Apple Silicon only for now.

1. [Download `PRCanvas.dmg`](https://github.com/kadetXx/prcanvas-releases/releases/latest/download/PRCanvas.dmg).
2. Open it and drag the app to Applications.
3. Open the app. It lives in the menu bar.

It needs two tools you may already have, and it tells you if either is missing: the GitHub
CLI, and one coding agent, signed in.

```sh
brew install gh && gh auth login
curl -fsSL https://claude.ai/install.sh | bash && claude   # or Codex: npm install -g @openai/codex && codex login
```

The GitHub CLI (`gh`) fetches the PR and posts your comments as you. Your agent writes the
summaries and the questions, on your own plan: Claude Code on a Claude plan, Codex on a
ChatGPT plan. With more than one installed, pick in the app's settings (the gear); every
one of them is also a tab in the comment box, so you can ask one and have another check it.

## Use

Click the menu bar icon, paste a PR link or `owner/repo#123`, press Return. The canvas
opens in your browser in seconds, and your agent fills in the summaries and questions over
the next ten to twenty-five. The panel lists what is running; each
canvas has an Open, a copy-link, and a stop. Quitting the app stops them all.

### Reading a canvas

- It opens on the whole PR. Press `→` to start reading in order, `←` to go back.
- `⏎` opens the code on the frame you are on. `s` switches between Story and Code.
  `esc` goes back to the overview.
- If you pan around and lose your place, `shift+→` resumes from whatever is under the
  middle of the screen.
- Each frame has a rail of questions. Click one to mark it checked, then issue, then
  n/a. Nothing here is a finding; they are things to look at.
- Comments already on the PR, whoever left them and wherever they left them, appear as
  bubbles on their lines. Click one to read the thread and reply. Unread ones are marked.
- Hover a line of code for a `+` to comment on it. Press `c` and click anywhere for a
  free comment. Both post to the PR on GitHub, and replies are ordinary GitHub threads.
- Approve posts your review to GitHub, with your note as its only text. Request changes
  and comment-only are in the same menu.

## How it works

1. The PR is fetched with the GitHub CLI, without checking out the repo.
2. Every changed file is parsed and split into frames: one per top-level declaration,
   the function or component as it stands after the change, with the diff marked inside.
3. The code itself gives the arrows between frames (who calls, awaits or renders whom)
   and the reading order. The canvas is served from a local port and opens in your
   browser with all of that already drawn.
4. Claude then fills it in, in parallel: which frames belong together when a PR does
   several things, a summary per frame, and the frames graded in small batches against a
   fixed list of concerns (a query inside a loop, a public contract that changed, a URL
   that went away, and so on), with a PR-specific question where one fits.

## Who does what

```
 you                          your Mac
 ┌────────────────────┐       ┌──────────────────────────────────────────┐
 │ paste a PR link    │──────▶│ the GitHub CLI fetches the PR            │
 └────────────────────┘       │        │                                 │
                              │        ▼                                 │
                              │ the canvas opens in your browser with    │
                              │ its frames, arrows and reading order     │
                              │        │                                 │
                              │        ▼                                 │
                              │ Claude Code fills in the summaries and   │
                              │ the questions while you read             │
                              └────────────────────┬─────────────────────┘
                                                   │
                                                   ▼
                              ┌──────────────────────────────────────────┐
                              │ you read it in order, answer the         │
                              │ questions, comment, approve              │
                              └────────────────────┬─────────────────────┘
                                                   │ as you, through the GitHub CLI
                                                   ▼
 ┌────────────────────┐       ┌──────────────────────────────────────────┐       ┌────────────────────┐
 │ anyone else        │──────▶│ GitHub: the PR                           │◀─────▶│ a teammate         │
 │ comments on        │       │ comments, reviews, threads               │       │ opens the same PR  │
 │ github.com, or a   │       └────────────────────┬─────────────────────┘       │ in their app; your │
 │ bot does           │                            │                             │ comments are on    │
 └────────────────────┘                            ▼                             │ their canvas       │
                              ┌──────────────────────────────────────────┐       └────────────────────┘
                              │ it shows on your canvas, on its line,    │
                              │ and you answer it there                  │
                              └──────────────────────────────────────────┘
```

Everything you write lands on the PR as you, through the GitHub CLI. A teammate with the app runs
the same PR and gets their own canvas with your comments already on it, because the
comments never lived anywhere but GitHub.

## Where your code goes

Two places, both of which it already goes to. GitHub, through the GitHub CLI, to fetch the PR and
to post what you write. And the company behind the agent you picked (Anthropic for Claude
Code, OpenAI for Codex, Google for Gemini CLI, and so on), through your own session, which
reads the frames of the PR the same way it reads a repo when you use it on one. That
agent's login, plan and data settings apply. Asking a second agent in the comment box sends
that line and the conversation to that agent's company too. Nowhere else: there is no prcanvas.review
server, comments live on the PR, and what you have checked on the rail lives in your
browser.

## Honest notes

- **Cost.** One PR is a handful of small calls to your agent, on your plan. With Claude Code,
  a 34-file PR was about twenty cents and under half a minute. Codex does not report a
  price; it counts against your ChatGPT plan's limits, and sends more tokens per call.
- **Quality.** The summaries and questions come from a model. They are usually right and
  sometimes not. That is why they are questions, not verdicts.

## Feedback

Open an issue here. Screenshots help.

---

## In more detail

### Why

Reviewing hasn't changed since we started working with agents. The diff is still a list
of files in alphabetical order, and you still read every line to work out what the change
does and what could go wrong. The tools that add AI to this mostly add findings: comments
that assert something is wrong, on the same alphabetical diff. Reviewers learn to skip
them.

prcanvas.review changes the reading surface instead. The change is shown in the order it
runs, from the entry point outward, so you read it the way it executes. Each piece has a
sentence saying what it does after the change. And instead of findings, each piece has
questions: things a careful colleague would look at, that you answer, and that you can
mark as an issue if the answer is bad. The top bar keeps count of how many of those
questions you have not opened yet. That number is for you, not a trail on the PR.

### What you are looking at

**Frames.** One per changed declaration: a function, a component, a type, a config
block. The frame shows the whole declaration as it stands after the change, with added
lines marked `+` and removed lines `-`, and long unchanged stretches folded. A frame is
not a file; a file with four changed functions is four frames.

**Order.** Frames are numbered in the order the logic runs, callers before callees,
starting from the entry point of the change, which carries a `start` badge. Arrows
between frames say how they relate: calls, awaits, renders, uses. The number
badges are coloured so you can find frame 12 on the map at a glance.

**Story and Code.** Story mode shows only the summaries, one line per frame, so the whole
PR fits on a screen. Code mode shows the diff inside each frame. The canvas opens in
Story.

**Threads.** When a PR does more than one thing, the frames are grouped into bands: the
one the title is about on top, then the others, labelled. The top bar says "3 changes".
That is the "please split this PR" comment, as a count.

**The rail.** To the right of each frame in Code mode: up to four questions, each
sitting beside the lines it is about, with a bracket when it covers a block. They come
from a fixed list of concerns plus questions written for this PR specifically. Hover one
to highlight its lines. Frames with nothing to ask say so.

**Comments.** Existing GitHub review comments show up as bubbles on their lines. Bubbles
you have not opened yet are highlighted; the button under the zoom controls hides them
all.

### Keys

| Key | Does |
| --- | --- |
| `→` `j` | next frame in reading order |
| `←` `k` | previous frame |
| `shift+→` | resume from the frame under the middle of the screen |
| `⏎` `s` | switch Story and Code, on the frame you are looking at |
| `esc` | overview of the whole PR |
| `c` | place a free comment with the next click |
| `h` | show or hide comments |
| double-click | open that frame |

Drag a frame by its header. Drag anywhere else to pan. Scroll to zoom. Click the minimap
to jump.

### Troubleshooting

**"Needs the GitHub CLI, signed in."** Install `gh` and run `gh auth login`, then Check
again. If your org uses SSO, `gh auth login` walks you through it.

**"Claude Code, signed in."** Install it with the command in the panel (the native
installer; `brew install --cask claude-code` also works), then run `claude` once to sign in.

**"No pull request #123 on owner/repo, or no access to it."** Check the number, and that
`gh auth status` shows an account that can see that repo. Some orgs require an admin to
approve third-party access for the GitHub CLI.

**A very large PR.** Layout has been tested to about 35 frames. Beyond that the overview
gets dense; the arrow keys still work.

**macOS asks about folders or the network.** It should not; the app runs everything from
your home folder. If it does, say so in an issue with what it asked for.

### Questions people ask

**Why not just ask Claude to review the PR?** You can, and you get a page of findings.
This is not that. The summaries put the change in the order it runs so you can read it;
the questions are a checklist you answer, not claims you argue with; and the count of
what you never opened is something a review has never had before.

**Why Claude Code and not an API key?** You already have it, it is already allowed at
your company, and its cost is already on your plan. Nothing to set up.

**Private repos?** Yes, anything your GitHub CLI login can see.

**Intel Macs?** Not yet. The bundled runtime is Apple Silicon.
