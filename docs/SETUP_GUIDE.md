# 環境構築・セットアップガイド

本書では、Supabase のプロジェクトセットアップから、本アプリケーションへの接続設定、および GitHub Pages へのデプロイ手順までを解説します。

---

## 1. Supabase のセットアップ

### ステップ 1: プロジェクト作成
1. [Supabase](https://supabase.com/) にアクセスし、アカウントにログインします。
2. **[New project]** をクリックします。
3. 任意のプロジェクト名（例: `headache-logger`）とセキュアなデータベースパスワードを入力します。
4. リージョンは地理的に近い場所（例: `Tokyo / Northeast (Tokyo)`）を選択します。
5. **[Create new project]** を実行します（初期設定の Data API や RLS の項目はすべて有効のままで問題ありません）。

### ステップ 2: テーブル作成およびRLS設定 (SQL実行)
プロジェクトダッシュボードの左サイドバーから **[SQL Editor]** > **[New query]** を開き、以下のSQLスクリプトを実行（Run）します。

```sql
-- 1. ユーザーデータ保存テーブルの作成
create table public.user_headache_data (
  user_id uuid primary key references auth.users(id) on delete cascade,
  entries jsonb not null default '[]'::jsonb,
  tags jsonb not null default '{"symptom":[], "med":[], "memo":[]}'::jsonb,
  updated_at timestamptz not null default now()
);

-- 2. Row Level Security (RLS) の有効化
alter table public.user_headache_data enable row level security;

-- 3. SELECT ポリシー（自身のデータのみ参照可能）
create policy "Users can select their own data"
  on public.user_headache_data for select
  using (auth.uid() = user_id);

-- 4. INSERT ポリシー（自身のデータのみ作成可能）
create policy "Users can insert their own data"
  on public.user_headache_data for insert
  with check (auth.uid() = user_id);

-- 5. UPDATE ポリシー（自身のデータのみ更新可能）
create policy "Users can update their own data"
  on public.user_headache_data for update
  using (auth.uid() = user_id);
```

### ステップ 3: 認証設定（メール確認の省略設定）
個人用途やテストを円滑に行う場合、新規登録時のメール確認ステップを省略できます。
1. 左サイドバーの **[Authentication]** > **[Providers]** > **[Email]** を開きます。
2. **「Confirm email」** をオフに切り替えて保存します。
※これにより、登録完了直後からログイン可能になります。

### ステップ 4: 接続クレデンシャルの取得
1. 左サイドバーの **[Project Settings]** (歯車アイコン) > **[API Keys]** (または **[API]**) を開きます。
2. 以下の2つの値を控えます：
   * **Project URL**: `https://xxxxxxxx.supabase.co` 形式のURL
   * **API Key**: `Publishable key`（または `anon (public)` キー）
   
> **セキュリティ警告**: `service_role` などの管理者用秘密鍵（Secret Key）はフロントエンドに入力しないでください。必ずパブリックアクセス用の Anon / Publishable キーを使用してください。

---

## 2. アプリケーションへの接続設定

1. ブラウザでアプリケーション（`index.html`）を開きます。
2. アプリの **「設定」** タブにある **「クラウド同期設定」** の「未接続」（クラウドステータス）ボタンをクリックしてモーダルを開きます。
3. モーダル上部の **「Supabase接続先設定」** に、前項で控えた **Project URL** と **API Key** を入力して **[接続設定を保存]** をクリックします。SupabaseのProject URLとAPI Keyを設定すると、プロジェクト内にユーザーアカウントを新規登録できます。ユーザアカウント毎に記録を保存できます。
4. 接続先設定を保存すると、**「ユーザーアカウント」** の入力欄とボタンが操作できるようになります。メールアドレスとパスワードを入力し、**[新規登録]**（または既存アカウントで **[ログイン]**）を実行します。
5. メール確認を有効にしている場合は、届いた確認メールのリンクを開いて認証を完了してからログインします。
6. クラウドステータスが「同期済」に切り替われば、クラウド同期の準備は完了です。以後はログインしたユーザーアカウントに紐づいて記録が保存されます。

接続先設定が未保存の間は、誤操作を防ぐためユーザーアカウント欄が薄く表示され、入力・ログイン・新規登録はできません。Project URL と API Key の両方を保存すると利用可能になります。未ログインのままでも、この端末のブラウザ内（localStorage）には記録できます。

### 未ログイン時の保存について

未ログイン時の記録は、ブラウザの `localStorage` に保存されます。これはサイト（オリジン）ごと、ブラウザごと、端末ごとに分かれた保存領域です。そのため、PCとスマートフォン、Chromeと別ブラウザの間では共有されません。通常の記録件数にアプリ側の固定上限はありませんが、ブラウザが定める保存容量やメモの量に依存します。

閲覧履歴だけを削除しても、通常は `localStorage` の記録は削除されません。一方、ブラウザの「Cookie とその他のサイトデータ」やWebサイトデータの削除、サイトデータの個別削除、ブラウザプロファイルの削除などを行うと記録が消える場合があります。シークレット／プライベートウィンドウでの記録も、ウィンドウを閉じると保持されないため、長期保存や端末間共有が必要な場合はSupabaseのユーザーアカウントへ同期してください。

---

## 3. GitHub Pages へのデプロイ

本プロジェクトは静的ファイル（単一の `index.html`）のみで完結しているため、リポジトリにプッシュするだけで即時ホスティングが可能です。

### デプロイ手順

```bash
# 変更をコミット
git add .
git commit -m "Deploy application"

# リモートへプッシュ
git push origin main
```

プッシュ完了後、GitHub のリポジトリページから **Settings** > **Pages** を開き、**Build and deployment** の Source が **Deploy from a branch**、Branch が `main` / `/(root)` に設定されていることを確認してください。
設定後、`https://<username>.github.io/headache-logger/` にてWeb版が公開されます。

