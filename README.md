# ZMK config for beekeeb Toucan Keyboard

[The beekeeb Toucan Keyboard](https://beekeeb.com/toucan-keyboard/) is a wireless split 42-key column‑stagger keyboard that a display and a trackpad, with an aggressive stagger on the pinky columns.

# License

The code in this repo is available under the MIT license.

The included shield nice_view_gem is modified from https://github.com/M165437/nice-view-gem licensed under the MIT License.

ZMK code snippets are taken from the ZMK documentation under the MIT license.

The embedded font QuinqueFive is designed by GGBotNet, licensed under under the SIL Open Font License, Version 1.1.

# Claude Code Commands

This repo includes custom [Claude Code](https://claude.com/claude-code) skills to automate the firmware workflow.

## `/deploy`
Commits your keymap changes, pushes to GitHub, and monitors the build.

```
/deploy
```
1. Detects changes in `config/toucan.keymap` (or other files)
2. Creates a commit with a descriptive message
3. Pushes to `origin main`
4. Watches the GitHub Actions build until it finishes

## `/build`
Downloads the compiled firmware from GitHub Actions.

```
/build
```
1. Checks if the latest build succeeded
2. Downloads the `.uf2` artifacts
3. Renames them to simple names in `firmware/`:
   - `toucan_left.uf2`
   - `toucan_right.uf2`
   - `settings_reset.uf2`

## `/install`
Interactive guided flashing for both halves of the Toucan.

```
/install
```
1. Prompts you to connect the **left side** via USB and double-tap RST
2. Flashes `toucan_left.uf2` to the XIAO bootloader
3. Prompts you to connect the **right side** and repeat
4. Flashes `toucan_right.uf2`

## Full Workflow

```
# 1. Edit your keymap
# 2. Deploy changes
/deploy

# 3. Download firmware
/build

# 4. Flash the keyboard
/install
```
