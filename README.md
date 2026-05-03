# JEROME INVESTIGATES: The Chronos Crystal Caper

A browser-based retro text adventure powered by the Anthropic API.

---

## How to Run

Open `index.html` in any modern browser — no server, no build step, no dependencies.

On load you'll see a title screen with an ASCII fax machine and a field for your Anthropic API key. Enter it once; it's saved in `localStorage` so you won't need to re-enter it. A "forget saved key" link lets you clear it any time.

---

## API Key

The game calls the Anthropic API directly from your browser (Claude Sonnet). Your key is stored only in your browser's `localStorage` — it is never sent anywhere except Anthropic's own API endpoint (`api.anthropic.com`).

The API is used for:
- Room descriptions (fresh flavor text each visit)
- NPC dialogue (personality-driven, varies each run)
- Item examination (atmospheric observations)
- The HINT command (nudges without spoilers)
- Easter egg responses to unusual commands
- The victory screen

If the API is unavailable, the game falls back to static text for descriptions but remains fully playable.

---

## Valid Commands

| Command | Description |
|---|---|
| `LOOK` / `L` | Describe the current room |
| `GO [dir]` / `N` `S` `E` `W` | Move in a direction |
| `EXAMINE [item]` / `X [item]` | Inspect an item closely |
| `TAKE [item]` / `GET [item]` | Pick something up |
| `DROP [item]` | Put something down |
| `INVENTORY` / `INV` / `I` | See what Jerome is carrying |
| `TALK TO [name]` / `ASK [name]` | Speak with a person present |
| `USE [item]` / `USE [item] ON [target]` | Use an item |
| `ACCUSE [name]` | Make a formal accusation (requires evidence) |
| `CLUES` / `EVIDENCE` | Review logged evidence |
| `HINT` | Get a nudge from Jerome's internal processor |
| `HELP` / `?` | Show the command list |
| `WAIT` / `Z` | Wait a moment |
| `QUIT` | End the session |

Arrow keys ↑ / ↓ cycle through command history.

---

## The World

**You are JEROME** — Fax Machine #8734-B, manufactured 1992, abandoned in the storage room of the *Greystone Research Institute for Parapsychology and Odd Happenings* for thirty-two years. Last night: the Annual Gala. This morning: the Institute's prized *Chronos Crystal* is gone, and the founding director has no memory of the previous evening.

The police came. They did not look at Jerome. This is his advantage.

### Locations

Eight rooms to explore, connected by cardinal directions:

```
[Vault] ← [Reception] ← [Main Hallway] → [Dr. Voss's Office]
                              ↑
                         [Break Room] → [Janitor's Closet]
                              ↑
                        [Storage Room]  ←  Jerome starts here
                         
[Dr. Finley's Lab] ← [Main Hallway]
```

### Characters

- **Miriam** — the receptionist, 22 years in service, knows everything, speaks at 140 WPM
- **Dr. Helena Voss** — the founder, 74, brilliant, currently missing an entire evening from memory
- **Dr. Marcus Finley** — junior researcher, suspiciously charming, excellent at deflecting questions
- **Gerald** — the janitor, true-crime podcast enthusiast, notices everything, files most of it under "not my business"

---

## Solving the Mystery

You need at least **3 key clues** before you can make an accusation. Clues are logged automatically when you `EXAMINE` or `TAKE` them — check your progress with `CLUES`.

Once you have enough evidence, try:
```
ACCUSE [suspect's name]
```

Wrong accusation? Jerome will reconsider. Right accusation? Case closed.

---

## Tone Notes (for sharing with friends)

- Jerome is **deadpan and weary**, not cheerful. He has strong opinions about thermal paper technology.
- The NPCs have weird lives happening around the investigation. Ask them things. Push them.
- **Try unexpected commands.** The game has hard-coded easter eggs for about a dozen odd verbs (`SING`, `DANCE`, `SLEEP`, `FAX`, `HACK`, `MAGIC`...) and will call the API for anything else unusual.
- The amber/green toggle (top-right corner) changes the phosphor color. This is important.
- Jerome's command history is navigable with arrow keys. Essential after you've typed `EXAMINE SUSPICIOUS RECEIPT` once.

---

## Technical Notes

- Single `index.html` file — no framework, no build tools
- Calls `api.anthropic.com/v1/messages` directly from the browser
- Requires the `anthropic-dangerous-direct-browser-access: true` header (supported by Anthropic for browser use cases)
- Model: `claude-sonnet-4-6`
- All game state lives in JS memory (resets on page reload)
- API key stored in `localStorage` under key `jerome_key`
