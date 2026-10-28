# halfwidth-dash-paren

A [Claude Code](https://claude.com/claude-code) skill for writing Chinese and English academic documents with consistent punctuation and proper scientific typesetting.

讓 Claude 寫報告、論文、CV 時，破折號和括號固定用半形，科學單位和化學式用真正的上下標。

## What it does / 功能

- Dashes, ranges and exponent minus signs use half-width `-`, never `－` `–` `—` `−`
- Parentheses use half-width `( )`, never `（ ）`
- Units, chemical formulas and ion charges use real superscript/subscript (`<sup>`/`<sub>` in HTML, character formatting in Word), not Unicode characters like `⁻¹ ₃`
- Stacked charge + subscript for ions such as I<sub>3</sub><sup>-</sup>
- CSS that keeps superscripts from stretching line height
- A PowerShell check to run before delivering a file

| Before | After |
|---|---|
| 2021.09–2025.06 | 2021.09-2025.06 |
| small–angle X–ray scattering （GISAXS） | small-angle X-ray scattering (GISAXS) |
| 2919 cm⁻¹ | 2919 cm<sup>-1</sup> |
| MAPbI₃ | MAPbI<sub>3</sub> |

## Install / 安裝

Copy the `halfwidth-dash-paren` folder into your Claude Code skills directory:

- Windows: `C:\Users\<you>\.claude\skills\`
- macOS / Linux: `~/.claude/skills/`

The result should be `.claude/skills/halfwidth-dash-paren/SKILL.md`. Restart Claude Code. The skill triggers automatically when Claude writes or edits a document, or you can call it with `/halfwidth-dash-paren`.

## Notes

- The delivery check uses PowerShell. On macOS/Linux, adapt it to Python or another tool that handles CJK character classes correctly.
- Text inside figures copied from papers is left unchanged; the rules apply only to editable text.
