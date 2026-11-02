# Agents.md — loup-macos-notepad

A sample macOS GUI application written in Loup: a plain-text editor (text
area, File menu with New / Open… / Save / Save As…, unsaved-changes alert,
Find with Next and a match count, font size and monospaced controls, a
lines / words / characters status bar). It is built on the appkit library,
https://github.com/myronccai/loup-macos-appkit, which must be checked out next
to this repository at `../loup-macos-appkit` (a path dependency in
`loup-macos-notepad.lppkg`). This repository is
https://github.com/myronccai/loup-macos-notepad (MIT, see `LICENSE`).

Read first:

- `README.md` — what the app does, the requirements (Loup newer than 1.27.0) and
  setup ("Getting started"), how to build, run and test it, its
  structure and the appkit modules it uses (it uses no raw Objective-C and
  works around no toolchain bug).
- `src/main.lphc` — the AppKit layer (`host.native`) and the `--self-test`
  driver; its header comment explains the state layout.
- `src/textstats.loup`, `src/find.loup`, `src/document.loup` — the logic
  (`host.dual`), each with a header comment; `src/*_test.lpscr` test them.
- `../loup-macos-appkit/*.lphc` header comments and
  `../loup-macos-appkit/docs/*.md` — the library (dialogs and canned test
  answers in `docs/dialogs.md`); its `README.md` and `Agents.md` describe the
  whole collection of example apps.
- `../loup-macos-appkit/run_all.sh` — the gate: both native drivers, both
  self-tests against `selftest.out`, and `loup test --strict src`.
- `../loup-macos-appkit/skills/loup-macos-developer-skill/SKILL.md` — the
  bundled Loup macOS developer skill: host modes, `extern native`,
  Objective-C and the toolchain on macOS.

Rules for changing it:

- Keep logic in the `host.dual` modules, with tests; `main.lphc` only wires
  AppKit to it, through the appkit proxies. Raw Objective-C (`appkit.objc`)
  only where the library has no API, each use commented with what is missing
  (and listed in README.md); today there is none.
- Run the binary only with `--self-test`, under lldb or through
  `bash ../loup-macos-appkit/run_all.sh loup-macos-notepad`. A self-test
  queues an answer for every dialog (`setTestAnswers`) and writes files only
  under `.temp/`.
- When the self-test's output changes on purpose, update `selftest.out`
  (both drivers must print it) and refresh the screenshot with
  `bash ../loup-macos-appkit/run_all.sh --screenshots loup-macos-notepad`.
- Leave no build output behind (`.temp/`, `.loup-cache/`, the binary,
  `*.loupbuild.fp`; see `.gitignore`).
