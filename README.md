# prcanvas.review

[![downloads](https://img.shields.io/github/downloads/kadetXx/prcanvas-releases/total?style=flat-square&color=a371f7&label=downloads)](https://github.com/kadetXx/prcanvas-releases/releases/latest) [![latest](https://img.shields.io/github/v/release/kadetXx/prcanvas-releases?style=flat-square&color=2ea043&label=latest)](https://github.com/kadetXx/prcanvas-releases/releases/latest) ![macOS](https://img.shields.io/badge/macOS-Apple%20Silicon-555?style=flat-square&logo=apple&logoColor=white)

A pull request as a canvas.

**[Download for Mac](https://github.com/kadetXx/prcanvas-releases/releases/latest/download/PRCanvas.dmg)** · Apple Silicon, notarised. Free.

Open a PR and you get what it does in a sentence, then its steps left to right in the order
the logic runs: one card per changed function, each saying what it does, wired to what it
calls, with a short list of questions to answer before you approve. You read it with the
arrow keys, card by card, instead of scrolling a diff sorted alphabetically by file.

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

From your agent: `/prcanvas 123` in Claude Code or Cursor, `$prcanvas 123` in Codex, or ask
Gemini CLI or opencode for a canvas of a PR. The app installs that for each agent it finds.

### Ready before you look

While the app is open, PRs that ask for your review, and your own open PRs, are read in the
background, one at a time, so they open complete in a few seconds rather than half a
minute. Your own are read on their first push, then again once they have gone ten minutes
without one. Drafts, bots and PRs untouched for days are left alone, and nothing is read on
battery or in Low Power Mode.

### Reading a canvas

- It opens in Story, on the whole PR. The first column says what it does and lists its steps
  down a timeline; click one to go there, and the step you are on stays lit. The steps run
  left to right after it. Press `→` to start reading in order, `←` to go back.
- Click a card to read it: its wires show, the rest dims. **More** in its footer opens the
  lines that matter (the declaration, every call, every change); **Fold** folds back any you
  opened, **Less** closes it. Click the word on a wire to open both its cards at once.
- `⏎` or `s` switches between Story and Code, on the card you are on; every card and frame
  also has a button that opens it in the other mode. `esc` goes back to the overview.
- If you pan around and lose your place, `shift+→` resumes from whatever is under the
  middle of the screen.
- Each frame has a rail of questions. Click one to mark it checked, then issue, then
  n/a. Nothing here is a finding; they are things to look at.
- Comments already on the PR, whoever left them and wherever they left them, appear as
  bubbles on their lines. Click one to read the thread and reply. Unread ones are marked.
- Hover a line of code for a `+` to comment on it. Press `c` and click anywhere for a
  free comment. Both post to the PR on GitHub, and replies are ordinary GitHub threads.
- Every wire says how two frames meet (calls, awaits, renders, uses). Hover it to see the
  code where it happens: the call, the await, the `<Tag />`, the name marked in both places.
- `⌘F` or `/` searches the whole PR: summaries, every line of code, questions, comments.
- Approve posts your review to GitHub, with your note as its only text. Request changes
  and comment-only are in the same menu. On your own PR the button is Merge, and anyone who
  can push finds Merge in the same menu; it merges only what the canvas shows.

### Agents at work

On a line, switch the comment box to an agent's tab and ask for a change ("make this return
early"); it offers a short plan, and once you approve, it works in a copy of the PR. While it writes, its avatar turns on the right edge of the
canvas. Tap it to follow: the canvas switches to Code and goes wherever it edits, with its
cursor and name at the line it is on, new files included; the avatar glows while you follow. Story shows the fix once it is written, on the card
it changed. Accept all or Discard when it is done.

## How it works

1. The PR is fetched with the GitHub CLI, without checking out the repo.
2. Every changed file is parsed and split into frames: one per top-level declaration,
   the function or component as it stands after the change, with the diff marked inside.
3. The code itself gives the arrows between frames (who calls, awaits or renders whom)
   and the reading order. The canvas is served from a local port and opens in your
   browser with all of that already drawn.
4. Your agent then fills it in, in parallel: what the PR does, which frames belong together
   when it does several things and the steps to read them in, a summary per frame, and the
   frames graded in small batches against a
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
                              │ your agent fills in the summaries and    │
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

**Order.** Frames run in the order the logic does, callers before callees, from the entry
point of the change. Curved wires join them, each with its word: calls, awaits, renders,
uses. Hover one for the line where it happens. (Prefer right angles? Advanced settings,
Curved wires.)

**Story.** Where the canvas opens. First, a column with the PR's title, what it does in a
sentence or two, and its steps down a timeline. Then the steps left to right, numbered, each
tagged (entry point, main change, data…) with a line on what is in it and how many questions
are left to check, and its cards stacked inside: a summary, what it does in plain words, its
file with its language icon, and its counts. The main change is in GitHub's ready-to-merge
green, and turns merged purple once the PR is in. The cards are not numbered; the steps are.

**Code.** The workspace: every frame with its whole diff, line comments, the questions
beside the lines they are about, agents writing live. Frames are numbered in reading order,
the entry point marked `start`, so you can find frame 12 on the map at a glance.

**Threads.** When a PR does more than one thing, Code groups the frames into bands: the
one the title is about on top, then the others, labelled. That is the "please split this
PR" comment, as a count.

**The rail.** To the right of each frame in Code mode, and under the code of an opened Story
card: up to four questions, each sitting beside the lines it is about, with a bracket when it covers a block. They come
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
| `⏎` `s` | switch Story and Code, on the card or frame you are looking at |
| `esc` | overview of the whole PR |
| `⌘F` `/` | search the whole PR; `n` and `N` step through the hits |
| `c` | place a free comment with the next click |
| `h` | show or hide comments |
| double-click | open that frame |

Drag a frame by its header. Two fingers pan and a pinch zooms, as in Figma (Advanced
settings, Scroll to pan, to scroll-zoom instead). Click the minimap to jump.

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

**Why your coding agent and not an API key?** You already have it, it is already allowed at
your company, and its cost is already on your plan. Nothing to set up.

**Updates?** The app updates itself from the menu bar, in green. Betas are off unless you turn
on Beta updates at the bottom of settings; then one is offered in purple, and once on it, the
arrow beside the version goes back to the last full release.

**Private repos?** Yes, anything your GitHub CLI login can see.

**Intel Macs?** Not yet. The bundled runtime is Apple Silicon.
