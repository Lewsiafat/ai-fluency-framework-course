# ai-fluency-framework-course

一小時互動式課程，改編自 Anthropic 官方免費課程「AI Fluency: Framework & Foundations」（Rick Dakan & Joseph Feller 與 Anthropic 合作設計，CC BY-NC-SA 4.0）。純 static，無後端。

## Dev commands

```bash
python3 -m http.server 8000   # 本機預覽，開 http://localhost:8000/
```

沒有 build/lint/test，改完 `index.html` 直接看瀏覽器結果即可。

## Architecture

單一檔案 `index.html`，HTML/CSS/JS 全部內嵌，無外部依賴、無 CDN import：
- CSS variables 做深色/淺色主題切換（`html.light` class 覆寫變數），與 llm-101 / ai-agent-usage-course 同一套機制，`#theme-toggle` 存 localStorage（key `ai-fluency-framework-course-theme`）
- 內容分 7 個 `<section>`（`#hero` + `#s1` ~ `#s6`），對應 6 章：為什麼需要 AI Fluency（三種互動模式：Automation/Augmentation/Agency）/ Delegation 授權 / Description 描述 / Discernment 鑑別 / Diligence 勤勉 / 結業測驗
- 第 1~5 章各自在章末加了一題隨堂小測驗（`qnum` 用「▸」，跟第 6 章結業考的 `Q1.`~`Q5.` 編號區分開），第 6 章結業考仍是原本的 5 題總複習；全部測驗共用同一套 `data-quiz`/`data-a` 標記機制與 JS（`querySelectorAll('[data-quiz]')`，新增測驗不用額外改 JS）
- 第 1~5 章章末各加一段「🗣️ 白話翻譯一下」callout，用生活化比喻換句話說一次該章核心概念，是**追加**而非取代原本專業內容（2026-07-20 依 Lewis 要求加入，Fable5 生成）
- 內容由 **Fable5 model** 生成，事實依據為 Anthropic 官方 PDF（4D 定義原文）與 ml-digest.com 的框架詳解（具體例子來源），非憑空生成

## 部署資訊

- VPS path: `/srv/projects/ai-fluency-framework-course/`
- URL: https://lewsi.ddns.net/ai-fluency-framework-course/
- nginx: `/etc/nginx/conf.d/projects/ai-fluency-framework-course.conf`，用 `alias`（純 static，無 port/後端程序）
- CI/CD: GitHub Actions → SSH deploy on push to `main`

## Known gotchas

- 沒有 port-registry 條目——static alias 站台不需要分配 port。
- git push 一律走 SSH（nss_wrapper deploy key），HTTPS push 在這個容器裡固定被 proxy 擋 401。
