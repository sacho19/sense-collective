# CLAUDE.md — sense-collective

This file provides guidance for AI assistants (Claude and others) working in this repository.

---

## プロジェクト概要

感性タイプ診断サイト。15問の質問からE/N/Iの3軸スコアを計算し、8タイプのうち1つに分類する。
診断は無料。有料レポート（PDF自動生成・メール配信）でマネタイズする。

**SNS:** [@sense_profiling](https://www.instagram.com/sense_profiling/)（Instagram）

---

## 技術スタック

| 要素 | 内容 |
|------|------|
| フレームワーク | React 18（CDN + Babel） / Next.js移行検討中 |
| スタイリング | Tailwind CSS（CDN） |
| DB | Supabase（PostgreSQL） |
| 決済 | Stripe |
| AI生成 | Claude API（anthropic） |
| ホスティング | 現状: 静的HTML / 将来: Vercel想定 |

---

## Supabaseテーブル構成

```sql
-- 診断結果
diagnostic_results (
  id           uuid PRIMARY KEY,
  user_id      uuid REFERENCES users(id),
  browser_id   text,
  type_key     text,   -- 例: "CID", "ENI"
  type_name    text,   -- 例: "ダークウッド"
  score_e      numeric,
  score_n      numeric,
  score_i      numeric,
  result_json  jsonb,
  answers      jsonb,
  session_id   text,
  created_at   timestamptz DEFAULT now()
)

-- ユーザー（メール任意）
users (
  id        uuid PRIMARY KEY,
  email     text UNIQUE,
  created_at timestamptz DEFAULT now()
)

-- ver.2で追加予定
purchases (
  id           uuid PRIMARY KEY,
  result_id    uuid REFERENCES diagnostic_results(id),
  user_id      uuid REFERENCES users(id),
  stripe_session_id text,
  amount       integer,
  status       text,  -- "pending" | "completed" | "failed"
  pdf_sent_at  timestamptz,
  created_at   timestamptz DEFAULT now()
)
```

---

## タイプシステム

### スコア軸

- **E軸**（活動性）: 穏やか ←→ 活動的
- **N軸**（感受性）: 大胆 ←→ 繊細
- **I軸**（判断軸）: 思慮深い ←→ 直感的

### タイプキーとタイプ名のマッピング

```js
{
  "CID": "ダークウッド",
  "CNI": "シルバーミスト",
  "CII": "コットンフラワー",
  "CND": "ホワイトセージ",
  "ENI": "ブラックローズ",
  "EII": "ゴールデンハニー",
  "EID": "パステルピンク",
  "END": "フレッシュミント"
}
```

### 相性スコアの計算方法

ユークリッド距離（E/N/Iスコア）で全タイプと比較。距離が近いほど相性が良い。

```js
function calcCompatibility(myScores, targetScores) {
  const dist = Math.sqrt(
    Math.pow(myScores.E - targetScores.E, 2) +
    Math.pow(myScores.N - targetScores.N, 2) +
    Math.pow(myScores.I - targetScores.I, 2)
  );
  return Math.max(0, 100 - dist * 10); // 0〜100のスコアに変換
}
```

---

## Repository Overview

**Project:** sense-collective
**Status:** Freshly initialized — project structure is being established.

This file will grow as the codebase evolves. Update it whenever you add new tooling, conventions, or architectural decisions.

---

## Development Workflow

### Branching Strategy

- All AI-assisted work happens on a dedicated `claude/<task-id>` branch.
- Never push directly to `main` without explicit permission.
- Use `git push -u origin <branch-name>` when pushing a new branch.

### Commit Messages

Use clear, imperative commit messages:
```
Add user authentication module
Fix off-by-one error in pagination logic
Refactor data fetching to use async/await
```

Avoid vague messages like "update" or "fix stuff".

### Pull Requests

- Keep PRs focused on a single concern.
- Include a summary of *what* changed and *why*.
- Reference the relevant issue number when applicable.

---

## Code Conventions

> Update this section as the project stack is decided and code is written.

### General

- Prefer clarity over cleverness.
- Avoid over-engineering — implement only what the current task requires.
- Do not add comments for self-evident code; only comment non-obvious logic.
- Do not add error handling for scenarios that cannot occur.

### Security

- Never hardcode secrets or credentials. Use environment variables.
- Validate all data at system boundaries (user input, external APIs).
- Avoid common vulnerabilities: SQL injection, XSS, command injection, SSRF.

---

## Project Structure

> To be filled in once the project structure is established.

```
sense-collective/
├── CLAUDE.md          # This file
├── README.md          # (to be created) Human-facing project overview
└── ...                # Source code, tests, config (to be added)
```

---

## Environment & Configuration

- Copy `.env.example` to `.env` for local development (add this file when secrets are needed).
- Never commit `.env` files.

---

## Testing

> To be filled in once a test framework is chosen.

- Run all tests before committing.
- Aim for meaningful tests on business logic, not trivial coverage.

---

## Dependency Management

> To be filled in once a language/runtime is chosen (npm, pip, cargo, etc.).

---

## AI Assistant Guidelines

1. **Read before editing.** Always read the relevant file(s) before making changes.
2. **Stay in scope.** Only make changes directly requested or clearly necessary.
3. **No file bloat.** Prefer editing existing files over creating new ones.
4. **No backwards-compat hacks.** Remove unused code rather than leaving stubs.
5. **Update this file** when you introduce new tooling, structure, or conventions.
6. **Ask before destructive actions.** Deleting files, force-pushing, dropping data — confirm first.
7. **Commit and push.** When a task is complete, commit with a clear message and push to the designated branch.
