# CodeIgniter3 から CodeIgniter4 への移行の忘備録です。

## プロジェクト概要
- 既存 CI3をCI4 へ移行
- **現行環境**：
  - CI3
  - Docker 開発環境
  - PHP 8.2.x / nginx 1.23.x / MySQL 8.0.x
- **新環境（移行後）**：
  - CI4
  - Docker 対応（PHP 8.3 対応）
  - `.env` ベースの環境変数管理