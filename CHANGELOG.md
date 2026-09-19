# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [v0.1.0] - 2026-09-19

### Added
- **Core Recording**: 頭痛の発生日時、症状タグ、痛みの強さ（Severity）、服薬タグ、メモを素早く記録する入力インターフェース。
- **Visualization**: Chart.js を利用した期間別の症状・服薬タグ推移の統計グラフ表示機能。
- **Calendar View**: カレンダー上での過去の記録確認および編集・削除機能。
- **Cloud Sync (Supabase)**: Supabase Auth を用いたユーザー認証（登録・ログイン）および PostgreSQL (RLS) による安全なクラウド同期機能。
- **Offline First**: ローカルストレージを活用したキャッシュ機構により、オフラインまたは未ログイン環境でも記録の永続化が可能。
- **Configuration Modal**: アプリケーション画面上から Supabase の Project URL / API Key を直接設定・保存できるUI。
- **Cloud Account Setup**: SupabaseのProject URLとAPI Keyを設定すると、プロジェクト内にユーザーアカウントを新規登録できます。ユーザアカウント毎に記録を保存できます。接続先設定を先に行う手順と、未設定時に認証UIをトーンダウンする表示を明記。
- **Documentation**: システムアーキテクチャ設計書 (`docs/ARCHITECTURE.md`) およびセットアップガイド (`docs/SETUP_GUIDE.md`) の整備。

### Changed
- **Statistics**: 集計期間を本日・直近7日・直近30日・全期間・任意期間から選択できるようにし、状態／回数および日毎／累計の表示を切り替え可能にした。症状・服薬タグ別の推移を表示する。
- **Statistics Visualization**: 1日以内は時間単位、60日以内は日単位、それより長い期間は週単位で集計し、状態／回数のヒートマップと累計の折れ線グラフを表示するようにした。セル選択時の詳細表示にも対応。
- **Record List**: 統計画面の記録一覧に日付・時刻による昇順／降順切り替えを追加。
- **Record Editing**: 日時に対応する保存済み記録をフォームへ自動表示し、内容に変更がある場合だけ更新を有効化するようにした。空の記録は保存できない。
- **Tag Input**: 症状・服薬・メモタグの追加ボタンを入力内容に応じて有効化し、Enterキーでも追加できるようにした。
- **Mobile Navigation**: 記録タブをカレンダーと入力フォームに分離し、画面下部の操作バーとスクロールトップ操作を追加した。
- **Touch Targets**: アプリ内のボタン高さを原則44pxに統一し、集計期間・集計方法の操作性を改善。
- **Theme and Readability**: ライト／ダークテーマの入力・操作部品の視認性を改善し、統計画面のセクションと集計値に区切り線・高コントラストの文字色を適用。

---

## バージョニングポリシー

本プロジェクトではセマンティックバージョニング（`vX.Y.Z`）を採用しています。正式リリース前は `v0.Y.Z` プレビュー版として運用します。

* **マイナー (`v0.Y.0`)**: 新機能の追加、破壊的変更を含む機能刷新
* **パッチ (`v0.Y.Z`)**: バグ修正、スタイルの改善、ドキュメントの更新
* **メジャー (`v1.0.0`)**: 本番運用に十分耐えうる品質に達した正式リリース版

### リリースタグ運用手順

```bash
# 注釈付きタグの作成
git tag -a v0.1.0 -m "Release v0.1.0: Initial preview release"

# タグのプッシュ
git push origin v0.1.0
```

