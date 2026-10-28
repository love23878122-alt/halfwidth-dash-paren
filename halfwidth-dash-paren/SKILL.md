---
name: halfwidth-dash-paren
description: "撰寫或修改要交給使用者的文件時，破折號、範圍線、科學指數負號一律用半形 -，括號用半形 ( )，不要用全形 －（）或 – — −；科學單位、化學式、離子電荷要用真正的上下標排版。中文和英文內容都適用，包括自傳、履歷、報告、論文段落、CV、Word、HTML 或 PDF 交付檔。產出或潤飾文件時套用，交付前檢查一次。Use when writing or editing any Chinese or English document for the user: half-width \"-\" and \"( )\" only, and proper superscript/subscript formatting for units, formulas and charges."
---

# 破折號和括號用半形

破折號和括號一律用鍵盤直接打得出來的半形符號。中文和英文內容都一樣。

| 要用 | 不要用 |
|---|---|
| `-` | `－`，也不要用 `–` 或 `—` |
| `(` `)` | `（` `）` |

符號照用就好，不需要為了避開它們而改寫句子。

## 例子

| 原本寫法 | 改成 |
|---|---|
| 持續探索可行方法（進行中） | 持續探索可行方法(進行中) |
| 退火溫度 100 °C（10 min） | 退火溫度 100 °C (10 min) |
| 研究計畫書（proposal） | 研究計畫書(proposal) |
| 2021.09–2025.06 | 2021.09-2025.06 |
| 第三章－結果與討論 | 第三章-結果與討論 |
| small–angle X–ray scattering （GISAXS） | small-angle X-ray scattering (GISAXS) |

空格的處理：括號前面接英文或數字時，中間空一格，例如 `100 °C (10 min)`、`GISAXS (同步輻射)`。括號前面接中文時不空格，例如 `計畫書(proposal)`。

## 科學單位、化學式、上下標

AI 產生的上下標常常看起來不自然，所以照下面的規則排版：

- **指數負號也用半形 `-`**，不要用數學負號 `−` (U+2212)。寫成 `cm<sup>-1</sup>`、`10<sup>-3</sup> m<sup>2</sup> V<sup>-1</sup> s<sup>-1</sup>`。負號要放在上標裡，不能掉回基線。
- **只用真正的上下標排版**：HTML 用 `<sup>`/`<sub>`，Word 用字元格式的上標/下標。不要用 Unicode 上下標字元 (`⁻ ¹ ² ³ ₂ ₃ ₓ`)，也不要兩種混用，或用 CSS 再額外升降一次。
- **上下標不能撐高行距**，HTML 用這組 CSS：
  ```css
  sup, sub { font-size: 68%; line-height: 0; position: relative; vertical-align: baseline; white-space: nowrap; }
  sup { top: -0.48em; }
  sub { top: 0.26em; }
  ```
- **離子電荷同時有上下標時上下疊排**，例如 I₃⁻：
  ```html
  I<span class="ss"><sup>-</sup><sub>3</sub></span>
  ```
  ```css
  .ss { display: inline-block; vertical-align: -0.38em; font-size: 68%; line-height: 0.95; margin-left: .05em; }
  .ss sup, .ss sub { display: block; position: static; font-size: 100%; line-height: 0.95; }
  ```
- **單位整組不斷行**：數值加單位、或一整串單位，用 `<span style="white-space:nowrap">` 包住，例如 `2919 cm<sup>-1</sup>`、`(×10<sup>-3</sup> m<sup>2</sup> V<sup>-1</sup> s<sup>-1</sup>)`。
- **數值和單位要照原論文核對**：cm² 和 m² 不能搞混，指數和數值都不能改。不要寫「依原文」這類製作註記。
- 其他符號保留正常字元，並確認字型有這個字：`×`、`μ`、`°`、`→`、`π`、`ε`。用 Times New Roman 加標楷體時，這些字都由 Times New Roman 顯示。

| 項目 | 正確寫法 (HTML) |
|---|---|
| 波數 | `141 cm<sup>-1</sup>` |
| 遷移率 | `m<sup>2</sup> V<sup>-1</sup> s<sup>-1</sup>` |
| 面積 | `10 × 10 μm<sup>2</sup>` |
| 陽離子 | `Sn<sup>2+</sup>`、`Pb<sup>2+</sup>` |
| 化學式 | `CsPbI<sub>2</sub>Br`、`MAPbI<sub>3</sub>`、`NiO<sub>x</sub>`、`I<sub>2</sub>` |
| 元件參數 | `V<sub>oc</sub>`、`V<sub>bi</sub>`、`R<sub>sh</sub>`、`ε<sub>r</sub>` |

論文原圖裡的文字保留原貌，不要重繪或修改；這些規則只適用於可編輯的文字。

## 交付前檢查

用 PowerShell 掃描最終文字。HTML 要先去掉標籤，Word 檔可以用 Word COM 讀 `$doc.Content.Text`。結果要是 0 筆：

```powershell
$t = Get-Content "<檔案路徑>" -Raw -Encoding UTF8
$t = $t -replace '<[^>]*>',''
[regex]::Matches($t, '[（）－–—−‐‑⁻¹²³₂₃ₓ]') |
  ForEach-Object { $s=[Math]::Max(0,$_.Index-15); $t.Substring($s,[Math]::Min(30,$t.Length-$s)) }
```

一次全部換掉：

```powershell
$t = [regex]::Replace($t, '(?<=[A-Za-z0-9.])（', ' (')
$t = $t.Replace('（','(').Replace('）',')').Replace([string][char]0xFF0D,'-').Replace([string][char]0x2013,'-').Replace([string][char]0x2014,'-').Replace([string][char]0x2212,'-').Replace([string][char]0x2010,'-').Replace([string][char]0x2011,'-')
```

Unicode 上下標字元 (`⁻¹²³₂₃ₓ`) 不能直接替換成普通字，要改成 `<sup>`/`<sub>` 標記，所以這類要手動處理。

PDF 交付檔還要做兩件事：
1. 用 pypdfium2 抽出 PDF 文字，再用上面的字元類別掃一次。
2. 把含上下標的那幾行放大 4 倍以上渲染成圖片，目視確認指數在上標位置、單位沒有斷行。

Git Bash 的 grep 遇到中文字元類別會誤判，不要用它檢查。
