# cangjie-fuzzy-linux

**Linux（ibus-rime、fcitx5-rime、fcitx-rime）：倉頡（三代為預設，五代可選）＋相鄰鍵容錯，容錯方案就是預設方案。**
**Linux (ibus-rime, fcitx5-rime, fcitx-rime): Cangjie (3rd generation by default, 5th optional) with adjacent-key typo tolerance as the DEFAULT schema.**

---

## 繁體中文

打字時按錯**一個相鄰的按鍵**也沒關係：例如想打「顏」(`yhmbc`) 卻打成 `yhmbv`，「顏」照樣會出現在候選裡；正確的候選永遠排在最前面。安裝完直接就是這個方案，**不必按 Ctrl+` 或 F4 切換**。

這個專案內含完整的 cj*fluency：倉頡三代／五代碼表、詞表、語言模型都在 release 裡，**不依賴任何第三方**。同一份內容也單獨發布成 [githubhjs/cangjie-fluency-linux](https://github.com/githubhjs/cangjie-fluency-linux)，那裡的預設方案是普通的 cangjie3_fluency（容錯方案排第二）。

### 一行安裝

```bash
curl -fsSL https://raw.githubusercontent.com/githubhjs/cangjie-fuzzy-linux/main/install.sh | bash
```

加選項（例如五代、較寬的容錯範圍）：

```bash
curl -fsSL https://raw.githubusercontent.com/githubhjs/cangjie-fuzzy-linux/main/install.sh | bash -s -- --with-cangjie5 --fuzzy-range wide
```

選項：`--with-cangjie5` 也裝五代（同樣有容錯版）· `--fuzzy-range narrow|normal|wide` 範圍 · `--frontend ibus|fcitx5|fcitx` · `--no-deps` 不自動裝引擎套件 · `--no-model` · `--no-sweep` 不安裝倉頡滑行輸入板 · `--pad-ref REF` · `--offline` · `--dry-run` · `--no-deploy`（完整清單：`--help`）

內附倉頡滑行輸入板（Cangjie Sweep Pad，`~/.local/bin/cangjie-sweep-pad`），預設一併安裝；使用 `--no-sweep` 可略過。相依套件為 python3-tk 與 xdotool（X11）或 ydotool（Wayland）。

離線：下載 release 的 `-full.zip`，解開後執行 `bash install.sh`。

### 預設值與可調整

容錯範圍（兩個鍵的中心距離不超過幾個鍵寬算相鄰）：`narrow` = 只有左右相鄰；`normal`（預設）= 再加斜向相鄰；`wide` = 再多一圈。每碼最多容錯 1 個鍵；每句上限為碼數的 40%（至少 2）僅在我們自己的比對器（Android、程式庫）生效，桌面 Rime 只有每碼容錯。預設值的研究依據：[DEFAULTS.md](https://github.com/githubhjs/cangjie-fuzzy-core/blob/main/DEFAULTS.md)。

### 已驗證 / 未驗證

已驗證：
- 引擎層（用系統的 librime）：20 項檢查全過。
- 基礎安裝程式（cangjie-fluency-linux）的安裝程式測試 106 項全過；IBus 實機打字測試（上一版）通過。

**沒有驗證**：
- fcitx5 / fcitx 前端的實際打字；apt 以外的套件管理員；x86_64（測試機是 ARM64）。

### 測試

`tests/headless/run_matrix.sh` 會安裝真正的 release 包，再用各平台自己的 librime 無介面地檢查 20 項（含：正確的候選仍排第一、相鄰鍵打錯能找回、兩個位置都錯或不相鄰則不會、方案清單第一個是容錯方案）。

### 授權

本專案自己的檔案為 MIT。隨附的碼表、詞表與語言模型是 LGPL-3.0 的資料，原樣放入，授權文字隨基礎專案附上；詳見 githubhjs/cangjie-fluency-linux。

---

## English

One wrong key is fine **when it is a physical neighbour**: type `yhmbv` instead of `yhmbc` and 顏 is still offered; the exact candidates always come first. After the install this is simply the default schema, **no Ctrl+` or F4 needed**.

The project contains the full cj*fluency (3rd and 5th generation tables, word list, language model) in its release, with **no third-party dependency**. The same content is also published as [githubhjs/cangjie-fluency-linux](https://github.com/githubhjs/cangjie-fluency-linux), where the default is plain cangjie3_fluency (the typo-tolerant schema is listed second).

### One-line install

```bash
curl -fsSL https://raw.githubusercontent.com/githubhjs/cangjie-fuzzy-linux/main/install.sh | bash
```

With options (for example 5th generation, a wider tolerance range):

```bash
curl -fsSL https://raw.githubusercontent.com/githubhjs/cangjie-fuzzy-linux/main/install.sh | bash -s -- --with-cangjie5 --fuzzy-range wide
```

Options: `--with-cangjie5` also install 5th generation (with its typo-tolerant version) · `--fuzzy-range narrow|normal|wide` · `--frontend ibus|fcitx5|fcitx` · `--no-deps` do not install engine packages · `--no-model` · `--no-sweep` skip Cangjie Sweep Pad · `--pad-ref REF` · `--offline` · `--dry-run` · `--no-deploy` (all options: `--help`)

Includes the Cangjie Sweep Pad (`~/.local/bin/cangjie-sweep-pad`), installed by default; pass `--no-sweep` to skip. Dependencies: python3-tk and xdotool (X11) or ydotool (Wayland).

Offline: download the release `-full.zip`, unpack it and run `bash install.sh`.

### Defaults and what you can change

Range (how far apart two key centres may be to count as neighbours): `narrow` = left/right only; `normal` (default) = plus diagonals; `wide` = one more ring. One wrong key per code. The per-phrase limit (40% of the codes, at least 2) only applies in our own matcher (Android, the library); desktop Rime has the per-code tolerance only. Evidence for the defaults: [DEFAULTS.md](https://github.com/githubhjs/cangjie-fuzzy-core/blob/main/DEFAULTS.md).

### Verified / not verified

Verified:
- Engine level (the distro's librime): all 20 checks pass.
- The base installer's tests: 106 of 106 pass; the earlier IBus live typing test passed.

**Not verified**:
- Typing through fcitx5 / fcitx; package managers other than apt; x86_64 (the test host is ARM64).

### Tests

`tests/headless/run_matrix.sh` installs the real release bundle and checks 20 things headlessly with each platform's own librime (exact candidates still first, one adjacent wrong key is recovered, two wrong keys or a non-adjacent key is not, the first schema in the list is the typo-tolerant one).

### Licence

Our own files are MIT. The bundled tables, word list and language model are LGPL-3.0 data, shipped unchanged with the licence texts in the base project; see githubhjs/cangjie-fluency-linux.

## Changelog / 更新紀錄

- **v0.2.1** — Sweep Pad v0.3.0: tolerance for backtracks. / 滑行輸入板 v0.3.0：容許回頭滑行。
- **v0.2.0** — Sweep Pad added (base installer v0.4.0); default on, `--no-sweep` to skip. / 新增倉頡滑行輸入板（基礎安裝程式 v0.4.0）；預設安裝，加 `--no-sweep` 可略過。
