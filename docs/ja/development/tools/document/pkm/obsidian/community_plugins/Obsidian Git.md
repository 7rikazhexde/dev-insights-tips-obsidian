---
title: Obsidian Git
tags:
  - Obsidian
  - Obsidian-Git
description: Obsidian Gitr関連のtipsをまとめてます。
---
# Obsidian Gitとは

### リポジトリ

https://github.com/Vinzent03/obsidian-git

### 設定

- 基本的に設定はデフォルトのままで良い。
- 変更ファイルは右下の設定メニューの
`Open Git control view`から確認できる。
- コミットメッセージは`Open Git control view`で指定できるが、obsidian gitのプラグインの設定から設定可能。
- git configはobsidian gitのプラグインで設定できる。usernameとauthor name,author emailは設定しないと認証エラーが出る場合がある。リポジトリのコンテンツ権限を有効にしたPATも必要になる場合もある。ただ、入力しても設定できているか確認できないので不明。
- PAT設定
1. GitHub → Settings → Developer settings → Personal access tokens → Fine-grained tokens
2. "Generate new token"
3. 設定内容：
   - Token name: 任意（例：obsidian-git-iphone）
   - Expiration: 任意の期限
   - Repository access: "Only select repositories"から対象リポジトリを選択
   - Repository permissions:
     - Contents: Read and write（gitの読み書きに必要）
     - Metadata: Read-only（自動で設定）

### 操作

- ⬆️:stage&push
- `+`: stage
- `-`: upstage
- `↑`: push
- `↓`: pull
- 📁:フォルダレベルで差分を表示
- `三`:ファイルレベルで差分を表示
- 🔄:ローカルファイルの更新？

![IMG_5304.png](./images/IMG_5304.png)

![IMG_5306.png](./images/IMG_5306.png)
