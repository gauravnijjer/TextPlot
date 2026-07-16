# TextPlot

A live text instrument for performance and installation. Speech, typed text, or a theatre script becomes text on a dark canvas: floating as fading apparitions, gathered as subtitles, or triggered cue by cue as surtitles.

Companion to Light Plot, Projection Plot and Stage Plot.

Made by [Gaurav Singh Nijjer](https://gauravnijjer.com) with Claude.

## What it is

TextPlot is a single self-contained HTML file. It runs in a browser, needs no install, no login, and no build step. Open it, choose a mode, and it should make sense within thirty seconds.

There are three modes:

**Live** listens to the microphone and renders words as they are spoken, either drifting across the canvas as ghostly apparitions or organised as subtitles at the bottom of the screen.

**Text** plays any pasted text out word by word with the same visual language as live mode. With the record button, this doubles as a render pipeline for text-based video work.

**Manual** is a surtitle operator's console. Upload a script, and it is auto-split into cues that you trigger with the arrow keys. Cues are editable, translatable, and exportable.

## Live mode

Three recognition engines, switchable in settings:

| Engine | Needs | Character |
|---|---|---|
| Web Speech | Chrome + internet | Instant, word by word |
| Whisper (browser) | Internet once, for the model download | Runs fully offline afterwards, auto-detects language, handles code-switched speech. Phrase by phrase rather than word by word |
| Whisper (local server) | A running whisper.cpp server | Best accuracy |

Languages: English, Hindi, Bengali, Tamil, Telugu, Marathi, Gujarati, Kannada, Malayalam, Punjabi, Urdu. Web Speech recognises one language at a time; the Whisper engines can auto-detect.

For the local server: `brew install whisper-cpp`, download a model, run `whisper-server -m ggml-base.bin`, and the default URL in settings should work.

Translation in live mode: Whisper can translate any language into English offline, inside the model itself. Alternatively MyMemory translates online into the Indian languages, shown beneath the original or alone.

Appearance is broadly tunable: entry styles (fade, typewriter, blur, grow, collapse, letters assembling), exit styles (fade, scatter, particle dissolve, blur), motion fields (drift, rise, rain, flow field, still), spectral treatments (echo, colour split, breathing glow), trails, voice reactivity, silence darkening, fonts, colours, and the canvas colour itself.

## Text mode

Paste text in the settings panel, set a pace in words per second, press play. Words feed through the same pipeline as live speech, so every appearance setting applies. Commas force a break, so comma-separated phrases appear as separate apparitions. Loop mode replays forever, which suits gallery installation.

## Manual mode (surtitles)

**Import.** Upload a script as .txt or .pdf. By default, TextPlot extracts dialogues only: speaker names and stage directions are stripped. The extractor expects standard stage layout (speaker names on their own line, directions as plain paragraphs), detects characters by how often their name recurs, and understands parentheticals, scene headings, page numbers, and wrapped lines. It is exact on standard layouts and never perfect on unusual ones; the cue editor is the final pass. A "full text" import mode is available for anything else.

**Cues.** The script is split into cues of a set maximum word count (default 8, adjustable, with a recalculate button). Blocks never merge across blank lines or speaker changes. The cue editor (press **C**) holds the list: every cue is an editable text box, click a number to jump, delete with the × button, add empty cues by hand. As you step through the show, the active cue parks near the top of the drawer so the next few cues stay visible for the operator.

**Triggering.** Down arrow fires the next cue, up arrow goes back. Show and Hide buttons sit pinned at the top of the drawer; Hide blanks the screen and the next trigger brings text back.

**Characters.** Speaker names detected during import populate the Characters section automatically. Each character can carry its own style (normal, italic, bold, bold italic) and colour. Names can be shown before each cue or hidden, which is the default.

**Translation.** Cues can be machine-translated into the Indian languages using a free online service, with an automatic fallback service behind it. Display options: original only, translation only, both stacked in one bounding box for a single projector, or both split into two boxes so two crops can feed two screens. Upcoming cues translate a few ahead as you step; "translate all cues now" prepares the whole show in one pass. Do that once on venue wifi, then export cues: translations travel inside the cue file, and the show runs offline afterwards.

**Safe-area box.** A toggle draws the maximum extent of the current cue list as a dashed rectangle with pixel dimensions, one box per language block. Set the crop in OBS or Isadora against it once and every cue of the show stays in frame. Measure with the menus closed, then switch it off for the show.

**Export and load.** Export cues writes the full list, characters, translations, and settings to a JSON file. Load restores it. Export early and often; it is the insurance against lost editing work.

## Recording

The record button (bottom right, any mode) captures the canvas to a .webm video at 30 fps. Panels and buttons are never in the recording. Browsers record WebM, not MP4; WebM drops straight into OBS, Resolve and Premiere, and if MP4 is required:

## Keyboard

| Key | Does |
|---|---|
| S | Settings panel |
| C | Cue editor (manual mode) |
| ↑ ↓ | Previous / next cue (manual mode) |
| Esc | Close the about popup |


## Internals worth knowing

Everything tunable lives in a `CONFIG` object at the top of the script, wired to the panel through a single `BINDINGS` table. The about text and footer credit live in `CONFIG` as well, are not editable from the interface, and are never overwritten by loaded cue files.

Built with p5.js, the Web Speech API, transformers.js (in-browser Whisper), and pdf.js for script import.

## Credits

Conceived and directed by Gaurav Singh Nijjer, built in collaboration with Claude (Anthropic).

© Gaurav Singh Nijjer 2026 · [gauravnijjer.com](https://gauravnijjer.com)
