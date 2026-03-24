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
| Block delimiters `[[ ]]` | `#1589F0` | `#1270C0` | `entity.name.tag.block.slop` |
| Control keywords `!KEYWORD!` | `#f03c15` | `#C0392B` | `keyword.control.slop` |
| Mode declarations `\|MODE\|` | `#9B59B6` | `#8E44AD` | `keyword.other.mode.slop` |
| Variable commands `SET` | `#2ECC71` | `#27AE60` | `keyword.other.variable-command.slop` |
| Variable references `{VAR}` | `#E67E22` | `#D35400` | `variable.other.reference.slop` |
| Bold emphasis `**text**` | `#FFFFFF` | `#1A1A2E` | `markup.bold.slop` |
| Italic emphasis `*text*` | `#95A5A6` | `#7F8C8D` | `markup.italic.slop` |
| Step declarations `# STEP N:` | `#F1C40F` | `#B7950B` | `entity.name.function.step.slop` |
| Comments `//` | `#555577` | `#95A5A6` | `comment.line.double-slash.slop` |

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

- `[[ BLOCK ]]` / `[[ /BLOCK ]]` block delimiters
- `!STOP!`, `!GATE!`, `!CONFIRM!`, `!ASSUME!`, `!LOOP!`, `!DONE!`, `!IGNORE!` control keywords
- `!GATE! GOTO: STEP N` jump instructions
- `RESUME: STEP N THEN GOTO: STEP M` / `THEN CONTINUE`
- `|EXECUTE|`, `|PLAN|`, `|REVIEW|`, `|ITERATE|`, `|EXPLAIN|`, `|DRAFT|` mode declarations
- `SET [VAR] = value`, `RECALL [VAR]`, `DEFAULT: [VAR] = value` variable commands
- `{VAR_NAME}` inline variable references
- `# STEP N:` and `# STEP N.M:` step and sub-step declarations
- `**bold**`, `*italic*`, `` `code` `` inline emphasis
- `// comments`
- Fenced code blocks with internal SLOP highlighting
- Schema header block (`[[ SLOP v1.4 ]]` ... `[[ /SLOP v1.4 ]]`) with section labels

## SLOP Spec

Full specification: [github.com/seandotsonama/SLOP](https://github.com/seandotsonama/SLOP)

## License

MIT
