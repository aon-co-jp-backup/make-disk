# make-disk は open-media へ引っ越しました / make-disk has moved to open-media

**日本語** | [English](#english)

## 日本語

このリポジトリ(`make-disk`)の開発は終了し、後継の
**[`open-media`](https://github.com/aon-co-jp/open-media)** に統合しました。
`open-media` は、`make-disk`(CD/DVD/Blu-ray書き込み・音声/動画の変換)と
`open-bar`(高音質の再生エンジン)を1つにしたアプリです。今後の開発・不具合の報告・
新しい版の配布は、すべて `open-media` で行います。

- 新しい場所: <https://github.com/aon-co-jp/open-media>
- 最新版のダウンロード: <https://github.com/aon-co-jp/open-media/releases/latest>
- このリポジトリにあった文書・ソースコード・翻訳(10言語のREADME/CLAUDE/PORTING)・
  インストーラー・ツールは、すべて `open-media` へ移しました。
  文書は [`docs/make-disk-legacy/`](https://github.com/aon-co-jp/open-media/tree/main/docs/make-disk-legacy)
  にあります。
- 過去の版(v0.1.32まで)の配布ファイルは、このリポジトリの
  [Releases](https://github.com/aon-co-jp/make-disk/releases) に残してあります。
  インストール済みの `make-disk` の自動更新はこの最新版(v0.1.32)を見続けるので、壊れませんが、
  新しい版は出ません。新しい版は `open-media` から入れてください。
- 変更の履歴(コミット)は、このリポジトリのGit履歴にすべて残っています。
  引っ越し前の内容を見るには、履歴を `v0.1.32` のタグまでさかのぼってください。

ライセンスは従来どおりです([LICENSE](LICENSE))。

## English

Development of this repository (`make-disk`) has ended. It has been merged into its successor,
**[`open-media`](https://github.com/aon-co-jp/open-media)**, which combines `make-disk`
(CD/DVD/Blu-ray burning and audio/video conversion) with `open-bar` (a high-quality playback
engine) in a single app. All further development, bug reports and new releases happen in `open-media`.

- New home: <https://github.com/aon-co-jp/open-media>
- Latest download: <https://github.com/aon-co-jp/open-media/releases/latest>
- All documents, source code, translations (README/CLAUDE/PORTING in 10 languages), the installer and
  the tools that lived here have been moved to `open-media`. The documents are in
  [`docs/make-disk-legacy/`](https://github.com/aon-co-jp/open-media/tree/main/docs/make-disk-legacy).
- Downloads of the past versions (up to v0.1.32) stay in this repository's
  [Releases](https://github.com/aon-co-jp/make-disk/releases). An already installed `make-disk` keeps
  checking that latest release (v0.1.32), so it does not break, but no newer versions will appear. Please get
  new versions from `open-media`.
- The full commit history stays in this repository's Git history. To see the content before the move,
  go back to the `v0.1.32` tag.

The license is unchanged ([LICENSE](LICENSE)).

## API 方針: REST 全廃・gRPC / Connect へ (2026-11-04)

### 日本語
- **REST は全廃する。新しい API は REST で作らない。** ブラウザや `curl` から呼ぶ API は **Connect の単純呼び出し**(`POST /<パッケージ>.<Service>/<Method>`、`content-type: application/json`、本文は JSON)、サービス間は **gRPC / gRPC-web**(RPoem の管理API `open_runo.admin.v1.Admin` と同じ流儀)。ストリーミングは、必要になってから。
- **既存の REST は、触るときに順に移す。** 移したら旧 REST の経路は消す。移行が終わるまで残るが、**新規には追加しない**。
- **JSON は RS-JSON に統一**する(読み込みは `from_str_secure` / `parse_secure` でサイズ・深さ・重複キーの上限つき、書き出しは `rust_json::fast`)。ブラウザの WASM に `prost` は入れない(サイズが増えるため)。
- **例外(自分たちの API ではなく、相手の規格が決めているもの)**: ACME、OAuth/OIDC のコールバック、第三者(GitHub など)からの Webhook、ヘルスチェック、静的ファイル配信、OTLP など。これらは規格どおりの HTTP を受ける。
- 参考実装: `RPoem`(`open_runo.admin.v1.Admin`)、`open-easy-web`(`server/src/connect.rs`、`proto/open_easy_web/admin/v1/admin.proto`)。
- ※ このリポジトリのコードが既に方針どおりかどうかは、各リポジトリの現状による。この節は方針の記録であり、コードの変更を意味しない。

### English
- **REST is being abolished. New APIs are not built as REST.** APIs called from browsers or `curl` use **Connect unary calls** (`POST /<package>.<Service>/<Method>`, `content-type: application/json`, JSON body); service-to-service calls use **gRPC / gRPC-web** (the same style as RPoem's admin API `open_runo.admin.v1.Admin`). Streaming only when it is actually needed.
- **Existing REST endpoints are migrated one by one, when they are touched.** The old REST route is removed once migrated. Until migration is finished they remain, but **no new ones are added**.
- **JSON is unified on RS-JSON**: reading with `from_str_secure` / `parse_secure` (size, depth and duplicate-key limits), writing with `rust_json::fast`. `prost` is not put into browser WASM (it would grow the binary).
- **Exceptions (shapes dictated by someone else's standard, not our own API)**: ACME, OAuth/OIDC callbacks, webhooks from third parties (e.g. GitHub), health checks, static file serving, OTLP, and so on. These accept plain HTTP as the standard requires.
- Reference implementations: `RPoem` (`open_runo.admin.v1.Admin`) and `open-easy-web` (`server/src/connect.rs`, `proto/open_easy_web/admin/v1/admin.proto`).
- Note: whether the code in a given repository already follows this policy depends on that repository. This section records the policy; it does not mean the code has been changed.

### Deutsch (KI-Übersetzung, noch nicht von Muttersprachlern geprüft)
- **REST wird abgeschafft. Neue APIs werden nicht als REST gebaut.** APIs, die aus dem Browser oder mit `curl` aufgerufen werden, nutzen **Connect-Unary-Aufrufe** (`POST /<Paket>.<Service>/<Methode>`, `content-type: application/json`, JSON-Body); zwischen Diensten gilt **gRPC / gRPC-web** (wie bei der Admin-API von RPoem, `open_runo.admin.v1.Admin`). Streaming erst, wenn es wirklich nötig ist.
- **Bestehende REST-Endpunkte werden einzeln migriert, sobald man sie anfasst.** Nach der Migration wird die alte REST-Route entfernt. Bis dahin bleiben sie bestehen, es werden aber **keine neuen hinzugefügt**.
- **JSON wird auf RS-JSON vereinheitlicht**: Lesen mit `from_str_secure` / `parse_secure` (Grenzen für Größe, Tiefe und doppelte Schlüssel), Schreiben mit `rust_json::fast`. `prost` kommt nicht in Browser-WASM (die Datei würde größer).
- **Ausnahmen (Formate, die ein fremder Standard vorgibt, nicht unsere eigene API)**: ACME, OAuth/OIDC-Callbacks, Webhooks von Dritten (z. B. GitHub), Health-Checks, statische Dateien, OTLP usw. Diese nehmen schlichtes HTTP entgegen, wie es der Standard verlangt.
- Referenzimplementierungen: `RPoem` (`open_runo.admin.v1.Admin`) und `open-easy-web` (`server/src/connect.rs`, `proto/open_easy_web/admin/v1/admin.proto`).
- Hinweis: Ob der Code eines Repositorys diese Richtlinie schon befolgt, hängt vom jeweiligen Repository ab. Dieser Abschnitt hält die Richtlinie fest; er bedeutet nicht, dass der Code bereits geändert wurde.

### Русский (машинный перевод, носителями языка ещё не проверен)
- **REST полностью отменяется. Новые API не создаются как REST.** API, вызываемые из браузера или через `curl`, используют **унарные вызовы Connect** (`POST /<пакет>.<Service>/<Method>`, `content-type: application/json`, тело — JSON); между сервисами — **gRPC / gRPC-web** (так же, как административный API RPoem, `open_runo.admin.v1.Admin`). Потоковая передача — только когда она действительно нужна.
- **Существующие REST-эндпоинты переносятся по одному, когда до них доходят руки.** После переноса старый REST-маршрут удаляется. До завершения переноса они остаются, но **новые не добавляются**.
- **JSON унифицируется на RS-JSON**: чтение через `from_str_secure` / `parse_secure` (ограничения на размер, глубину и повторяющиеся ключи), запись через `rust_json::fast`. `prost` не добавляется в браузерный WASM (размер вырос бы).
- **Исключения (форматы, заданные чужим стандартом, а не наш собственный API)**: ACME, обратные вызовы OAuth/OIDC, вебхуки сторонних сервисов (например, GitHub), проверки работоспособности, раздача статических файлов, OTLP и т. п. Они принимают обычный HTTP, как требует стандарт.
- Эталонные реализации: `RPoem` (`open_runo.admin.v1.Admin`) и `open-easy-web` (`server/src/connect.rs`, `proto/open_easy_web/admin/v1/admin.proto`).
- Примечание: соответствует ли код конкретного репозитория этой политике, зависит от самого репозитория. Этот раздел фиксирует политику и не означает, что код уже изменён.
