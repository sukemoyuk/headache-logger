<div align="center">

# 頭痛ろぐ

**サーバーレスかつプライバシー重視の、マルチデバイス対応・頭痛記録Webアプリケーション**

[![Version](https://img.shields.io/badge/version-v0.1.0-22272e.svg?style=flat-square)](CHANGELOG.md)
[![License](https://img.shields.io/badge/license-MIT-22272e.svg?style=flat-square)](LICENSE)
[![Hosting](https://img.shields.io/badge/hosting-GitHub_Pages-22272e.svg?style=flat-square)](https://pages.github.com/)
[![Backend](https://img.shields.io/badge/backend-Supabase-22272e.svg?style=flat-square)](https://supabase.com/)

</div>

---

## 概要

**頭痛ろぐ** は、日々の頭痛の発生状況、服薬内容、痛みの強さを素早く記録し、振り返りや医師への相談に役立てるためのWebアプリケーションです。

GitHub Pages による静的配信と Supabase（BaaS）を連携させることで、サーバー運用コストゼロでマルチデバイス（PC・スマートフォン）間の安全なデータ同期を実現しています。また、オフライン時でもローカルキャッシュにより中断することなく記録を継続できます。

---

## 主な機能

| 機能 | 内容 |
| :--- | :--- |
| **クイック記録** | 発生時刻、痛みの強さ（Severity）、症状タグ、服薬内容、メモをワンタップで登録。 |
| **カレンダー・一覧表示** | 月ごとのカレンダー上で発生頻度を一目で把握し、過去の記録を柔軟に編集・削除。 |
| **統計・グラフ分析** | 本日・直近7日・直近30日・全期間、または任意の日付範囲で集計。状態／回数と日毎／累計を切り替え、症状・服薬タグ別のヒートマップと累計折れ線グラフで可視化。 |
| **記録一覧ソート** | 統計画面の記録一覧を日付・時刻の昇順または降順で切り替え。 |
| **タグ・テーマ管理** | 症状・服薬・メモタグの表示・並び替え、ライト／ダークテーマの切り替えに対応。 |
| **マルチデバイス同期** | Supabase Auth によるユーザー認証と、PostgreSQL への双方向データ同期。 |
| **オフラインファースト** | `localStorage` キャッシュにより、未ログインや通信圏外でも記録が消失しません。 |
| **プライバシー保護** | PostgreSQL の Row Level Security (RLS) により、第三者によるアクセスをDB層で遮断。 |

---

## 技術スタック

* **Frontend**: HTML5, CSS3, Vanilla JavaScript (ES6+), Chart.js
* **Hosting**: GitHub Pages
* **Backend as a Service**: Supabase (PostgreSQL, Supabase Auth, PostgREST)
* **Architecture**: Serverless SPA (Single Page Application)

---

## クイックスタート

### 1. Webブラウザで直接利用する
ローカル環境のビルド作業は不要です。ブラウザで `index.html` を開くだけでオフライン記録ツールとして即座に動作します。

```bash
# リポジトリのクローン
git clone https://github.com/sukemoyuk/headache-logger.git

# アプリケーションを開く
open index.html  # macOS
start index.html # Windows
```

### 2. クラウド同期を利用する
PC・スマホ間でデータを共有したい場合は、Supabase のプロジェクトを作成し、アプリの「設定」タブにある「クラウド同期設定」から Supabase 接続先設定（Project URL / API Key）を保存してください。SupabaseのProject URLとAPI Keyを設定すると、プロジェクト内にユーザーアカウントを新規登録できます。ユーザアカウント毎に記録を保存できます。

接続先設定を保存すると、同じ画面のユーザーアカウント欄からメールアドレスとパスワードで新規登録またはログインできます。未ログインでも記録はこの端末のブラウザ内に保存されますが、クラウド同期や複数端末での共有にはログインが必要です。API Key は Publishable key または Legacy anon key を使い、`service_role` / Secret key は入力しないでください。

未ログイン時の記録はブラウザの `localStorage` に保存されます。保存容量はサイト（オリジン）ごと、ブラウザごと、端末ごとに管理され、通常の記録件数に固定上限はありません。ただし、閲覧履歴だけを削除しても通常は消えませんが、ブラウザの「Cookie とその他のサイトデータ」やWebサイトデータを削除した場合、ブラウザプロファイルを削除した場合などは記録が消えることがあります。重要な記録はSupabaseへ同期してください。

詳細な手順については [セットアップガイド](docs/SETUP_GUIDE.md) を参照してください。

---

## ドキュメント

プロジェクトの設計思想や運用に関する詳細ドキュメントは `docs/` ディレクトリに整備されています。

* [システムアーキテクチャ・データベース設計書](docs/ARCHITECTURE.md)  
  システム構成図（Mermaid）、データベーススキーマ（JSONB構造）、RLSセキュリティ設計。
* [環境構築・セットアップガイド](docs/SETUP_GUIDE.md)  
  Supabase の設定手順（SQLスクリプト含む）、接続手順、GitHub Pages へのデプロイ方法。
* [更新履歴・バージョニング方針](CHANGELOG.md)  
  各バージョンの変更点およびセマンティックバージョニング規約。

---

## ライセンス

本プロジェクトは [MIT License](LICENSE) のもとで公開されています。
