# SLOP Language Support for VS Code

Syntax highlighting and color themes for [SLOP](https://github.com/seandotsonama/SLOP) (Simple Language of Prompting) `.slp` files.

## Features

- Syntax highlighting for all 7 SLOP syntax categories
- Two bundled color themes (Dark and Light) matching the official SLOP color scheme
- File association for `.slp` files
- Comment toggling (`//`)
- Bracket matching for `[[ ]]` blocks and `{ }` variable references
- Code folding on STEP declarations

## Color Mapping

| Category | Dark | Light | Scope |
|---|---|---|---|
| Block delimiters `[[ ]]` | ![#1589F0](https://placehold.co/15x15/1589F0/1589F0.png) `#1589F0` | ![#1270C0](https://placehold.co/15x15/1270C0/1270C0.png) `#1270C0` | `entity.name.tag.block.slop` |
| Control keywords `!KEYWORD!` | ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) `#f03c15` | ![#C0392B](https://placehold.co/15x15/C0392B/C0392B.png) `#C0392B` | `keyword.control.slop` |
| Mode declarations `\|MODE\|` | ![#9B59B6](https://placehold.co/15x15/9B59B6/9B59B6.png) `#9B59B6` | ![#8E44AD](https://placehold.co/15x15/8E44AD/8E44AD.png) `#8E44AD` | `keyword.other.mode.slop` |
| Variable commands `SET` | ![#2ECC71](https://placehold.co/15x15/2ECC71/2ECC71.png) `#2ECC71` | ![#27AE60](https://placehold.co/15x15/27AE60/27AE60.png) `#27AE60` | `keyword.other.variable-command.slop` |
| Variable references `{VAR}` | ![#E67E22](https://placehold.co/15x15/E67E22/E67E22.png) `#E67E22` | ![#D35400](https://placehold.co/15x15/D35400/D35400.png) `#D35400` | `variable.other.reference.slop` |
| Bold emphasis `**text**` | ![#FFFFFF](https://placehold.co/15x15/FFFFFF/FFFFFF.png) `#FFFFFF` | ![#1A1A2E](https://placehold.co/15x15/1A1A2E/1A1A2E.png) `#1A1A2E` | `markup.bold.slop` |
| Italic emphasis `*text*` | ![#95A5A6](https://placehold.co/15x15/95A5A6/95A5A6.png) `#95A5A6` | ![#7F8C8D](https://placehold.co/15x15/7F8C8D/7F8C8D.png) `#7F8C8D` | `markup.italic.slop` |
| Step declarations `# STEP N:` | ![#F1C40F](https://placehold.co/15x15/F1C40F/F1C40F.png) `#F1C40F` | ![#B7950B](https://placehold.co/15x15/B7950B/B7950B.png) `#B7950B` | `entity.name.function.step.slop` |
| Comments `//` | ![#555577](https://placehold.co/15x15/555577/555577.png) `#555577` | ![#95A5A6](https://placehold.co/15x15/95A5A6/95A5A6.png) `#95A5A6` | `comment.line.double-slash.slop` |

## Install from VSIX (local)

1. Clone or download this repo
2. Install `vsce` if you do not have it: `npm install -g @vscode/vsce`
3. From the `vscode-slop/` directory, run: `vsce package`
4. In VS Code: Extensions > `...` menu > "Install from VSIX" > select the generated `.vsix` file

## Install from source (development)

1. Copy or symlink the `vscode-slop/` directory into `~/.vscode/extensions/slop-language`
2. Restart VS Code
3. Open any `.slp` file

## Activate the theme

After installing, open Command Palette (`Ctrl+Shift+P` / `Cmd+Shift+P`), type "Color Theme", and select either:

- **SLOP Dark** (recommended for dark editor backgrounds)
- **SLOP Light** (for light editor backgrounds)

The syntax highlighting works with any theme, but the SLOP themes use the exact color mapping from the SLOP specification.

## Highlighted syntax elements

- ![#1589F0](https://placehold.co/15x15/1589F0/1589F0.png) `[[ BLOCK ]]` / `[[ /BLOCK ]]` block delimiters
- ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) `!STOP!`, `!GATE!`, `!CONFIRM!`, `!ASSUME!`, `!LOOP!`, `!DONE!`, `!IGNORE!` control keywords
- ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) `!GATE! GOTO: STEP N` jump instructions
- ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) `RESUME: STEP N THEN GOTO: STEP M` / `THEN CONTINUE`
- ![#9B59B6](https://placehold.co/15x15/9B59B6/9B59B6.png) `|EXECUTE|`, `|PLAN|`, `|REVIEW|`, `|ITERATE|`, `|EXPLAIN|`, `|DRAFT|` mode declarations
- ![#2ECC71](https://placehold.co/15x15/2ECC71/2ECC71.png) `SET [VAR] = value`, `RECALL [VAR]`, `DEFAULT: [VAR] = value` variable commands
- ![#E67E22](https://placehold.co/15x15/E67E22/E67E22.png) `{VAR_NAME}` inline variable references
- ![#F1C40F](https://placehold.co/15x15/F1C40F/F1C40F.png) `# STEP N:` and `# STEP N.M:` step and sub-step declarations
- ![#95A5A6](https://placehold.co/15x15/95A5A6/95A5A6.png) `**bold**`, `*italic*`, `` `code` `` inline emphasis
- ![#555577](https://placehold.co/15x15/555577/555577.png) `// comments`
- ![#1589F0](https://placehold.co/15x15/1589F0/1589F0.png) Schema header block (`[[ SLOP v1.4 ]]` ... `[[ /SLOP v1.4 ]]`) with section labels
- Fenced code blocks with internal SLOP highlighting

## SLOP Spec

Full specification: [github.com/seandotsonama/SLOP](https://github.com/seandotsonama/SLOP)

## License

MIT
