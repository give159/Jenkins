# 【5分でできる】Jenkins 初めてのジョブ作成 - 現在時刻を表示する

Jenkinsサーバーが起動している状態から、**dateコマンドで現在時刻を表示するジョブ**を作ります！

---

## 🎯 これから作るもの

```
ジョブ名: show-current-time
実行内容: サーバーの現在時刻を表示
出力例: 
  2025年 10月 23日 木曜日 14:30:15 JST
  PC起動時刻: 2025-10-23 09:00:00
```

---

## 📋 前提条件

✅ Jenkinsサーバーが起動している（http://localhost:8080 にアクセスできる）
✅ Jenkinsにログインできる

---

## 🚀 手順1: 新しいジョブを作成

### ステップ1-1: New Item をクリック

```
┌─────────────────────────────────────┐
│ Jenkins                              │
├─────────────────────────────────────┤
│ 🏠 Dashboard                        │
│ 🆕 New Item          ← ここをクリック │
│ 👥 People                           │
│ 📊 Build History                    │
│ ⚙️  Manage Jenkins                  │
└─────────────────────────────────────┘
```

---

### ステップ1-2: ジョブ名を入力

```
┌─────────────────────────────────────┐
│ Enter an item name                   │
│ ┌─────────────────────────────────┐ │
│ │ show-current-time               │ │
│ └─────────────────────────────────┘ │
└─────────────────────────────────────┘

💡 ジョブ名のルール:
  ✅ 英数字、ハイフン、アンダースコア
  ❌ スペース、日本語、特殊文字
```

---

### ステップ1-3: Freestyle project を選択

```
┌─────────────────────────────────────┐
│ ● Freestyle project  ← これを選択    │
│ ○ Pipeline                          │
│ ○ Multi-configuration project      │
│ ○ Folder                            │
│ ○ Multibranch Pipeline              │
│ ○ Organization Folder               │
└─────────────────────────────────────┘

下にスクロールして「OK」ボタンをクリック
```

---

## 🔧 手順2: ビルドステップを追加

設定画面が開きます。下にスクロールして「**Build**」セクションを探します。

### ステップ2-1: Add build step をクリック

```
┌─────────────────────────────────────┐
│ Build                                │
│ ┌─────────────────────────────────┐ │
│ │ Add build step ▼                │ │ ← クリック
│ └─────────────────────────────────┘ │
└─────────────────────────────────────┘
```

---

### ステップ2-2: Execute shell を選択

```
メニューが表示されます:
┌─────────────────────────────────────┐
│ Execute shell           ← Linuxの場合 │
│ Execute Windows batch command       │
│ Invoke Ant                          │
│ Invoke Gradle script                │
│ Invoke top-level Maven targets      │
└─────────────────────────────────────┘

💡 環境に応じて選択:
  Linux/Mac → Execute shell
  Windows   → Execute Windows batch command
```

---

### ステップ2-3: コマンドを入力

**Linux/Macの場合:**

```bash
#!/bin/bash

echo "=== 現在時刻を表示 ==="
date

echo ""
echo "=== 詳細な日時情報 ==="
date '+%Y年 %m月 %d日 %A %H:%M:%S %Z'

echo ""
echo "=== システム起動時刻 ==="
uptime -s

echo ""
echo "=== タイムゾーン ==="
timedatectl | grep "Time zone"
```

---

**Windowsの場合:**

```cmd
@echo off
echo === 現在時刻を表示 ===
echo %date% %time%

echo.
echo === システム情報 ===
systeminfo | findstr /C:"System Boot Time"

echo.
echo === タイムゾーン ===
tzutil /g
```

---

### ステップ2-4: 入力画面

```
┌─────────────────────────────────────┐
│ Build                                │
│ Execute shell                        │
│ ┌─────────────────────────────────┐ │
│ │ #!/bin/bash                     │ │
│ │                                 │ │
│ │ echo "=== 現在時刻を表示 ===" │ │
│ │ date                            │ │
│ │                                 │ │
│ │ (コマンドをここに貼り付け)    │ │
│ └─────────────────────────────────┘ │
└─────────────────────────────────────┘
```

---

## 💾 手順3: 保存

画面下部の「**Save**」ボタンをクリック

```
┌─────────────────────────────────────┐
│ [Save]  [Apply]  [Delete Project]   │
│  ↑                                  │
│  ここをクリック                       │
└─────────────────────────────────────┘
```

---

## ▶️ 手順4: ジョブを実行

### ステップ4-1: Build Now をクリック

ジョブの画面に移動したら、左サイドバーの「**Build Now**」をクリック

```
┌─────────────────────────────────────┐
│ show-current-time                    │
├─────────────────────────────────────┤
│ ← Back to Dashboard                 │
│                                     │
│ 🔨 Build Now         ← ここをクリック │
│ 🔧 Configure                        │
│ 📊 Build History                    │
│ 🗑️  Delete Project                 │
└─────────────────────────────────────┘
```

---

### ステップ4-2: ビルド履歴を確認

左下の「**Build History**」に新しいビルドが表示されます

```
┌─────────────────────────────────────┐
│ Build History                        │
├─────────────────────────────────────┤
│ #1  ⏱️  数秒前  ⚙️ 実行中...       │
│  ↓                                  │
│ #1  ✅ 1分前   SUCCESS              │
└─────────────────────────────────────┘
```

---

### ステップ4-3: Console Output を確認

ビルド番号（#1）をクリックして、「**Console Output**」を選択

```
┌─────────────────────────────────────┐
│ Build #1                             │
├─────────────────────────────────────┤
│ 📋 Console Output    ← ここをクリック │
│ 📊 Status                           │
│ 🔄 Changes                          │
└─────────────────────────────────────┘
```

---

## 📺 出力結果の見方

### Linux/Macの場合

```
Started by user admin
Running as SYSTEM
Building in workspace /var/lib/jenkins/workspace/show-current-time
[show-current-time] $ /bin/bash /tmp/jenkins1234567890.sh

=== 現在時刻を表示 ===
Thu Oct 23 14:30:15 JST 2025

=== 詳細な日時情報 ===
2025年 10月 23日 Thursday 14:30:15 JST

=== システム起動時刻 ===
2025-10-23 09:00:00

=== タイムゾーン ===
       Time zone: Asia/Tokyo (JST, +0900)

Finished: SUCCESS
```

---

### Windowsの場合

```
Started by user admin
Running as SYSTEM
Building in workspace C:\jenkins\workspace\show-current-time
[show-current-time] $ cmd /c call C:\TEMP\jenkins1234567890.bat

=== 現在時刻を表示 ===
2025/10/23 14:30:15.45

=== システム情報 ===
System Boot Time: 2025/10/23, 9:00:00

=== タイムゾーン ===
Tokyo Standard Time

Finished: SUCCESS
```

---

## 🎨 さらに便利にする設定

### 1. タイムスタンプを追加

ログに時刻を表示して、処理時間を把握しやすくします。

**設定方法:**
```
1. ジョブの設定画面を開く（Configure）
2. Build Environment セクション
3. ✅ Add timestamps to the Console Output にチェック
4. Save
```

**結果:**
```
[2025-10-23 14:30:15] Started by user admin
[2025-10-23 14:30:16] Running as SYSTEM
[2025-10-23 14:30:16] Building in workspace...
[2025-10-23 14:30:17] === 現在時刻を表示 ===
[2025-10-23 14:30:17] Thu Oct 23 14:30:17 JST 2025
[2025-10-23 14:30:18] Finished: SUCCESS
```

必要なプラグイン: **Timestamper Plugin**（標準でインストール済みの場合が多い）

---

### 2. 定期的に実行

毎朝9時に自動実行する設定

**設定方法:**
```
1. ジョブの設定画面を開く
2. Build Triggers セクション
3. ✅ Build periodically にチェック
4. Schedule に以下を入力:
   0 9 * * *
5. Save
```

**Schedule の書き方:**
```
0 9 * * *
│ │ │ │ └── 曜日 (0-7, 0と7は日曜)
│ │ │ └──── 月 (1-12)
│ │ └────── 日 (1-31)
│ └──────── 時 (0-23)
└────────── 分 (0-59)

例:
0 9 * * *      → 毎日9時
0 9 * * 1-5    → 平日の9時
*/10 * * * *   → 10分ごと
0 */2 * * *    → 2時間ごと
```

---

### 3. ビルド履歴を管理

古いビルドを自動削除してディスク容量を節約

**設定方法:**
```
1. ジョブの設定画面を開く
2. General セクション
3. ✅ Discard old builds にチェック
4. 設定:
   Strategy: Log Rotation
   Days to keep builds: 7
   Max # of builds to keep: 10
5. Save
```

**結果:**
- 7日より古いビルドは自動削除
- 最大10個までビルド履歴を保持

---

### 4. Slack通知を追加

ビルド結果をSlackに通知

**設定方法:**
```
1. ジョブの設定画面を開く
2. Post-build Actions セクション
3. Add post-build action → Slack Notifications
4. 設定:
   ✅ Notify Success
   Channel: #jenkins
5. Save
```

必要なプラグイン: **Slack Notification Plugin**

---

## 🎯 応用編：もっと詳しい情報を表示

### より詳細なシステム情報

**Linux/Mac:**
```bash
#!/bin/bash

echo "╔═══════════════════════════════════════╗"
echo "║   Jenkins - システム情報レポート       ║"
echo "╚═══════════════════════════════════════╝"

echo ""
echo "📅 現在日時"
echo "----------------------------------------"
date '+%Y年 %m月 %d日 (%A) %H:%M:%S %Z'

echo ""
echo "🖥️  ホスト名"
echo "----------------------------------------"
hostname

echo ""
echo "🔄 システム起動時刻"
echo "----------------------------------------"
uptime -s

echo ""
echo "⏱️  稼働時間"
echo "----------------------------------------"
uptime

echo ""
echo "👤 現在のユーザー"
echo "----------------------------------------"
whoami

echo ""
echo "📂 作業ディレクトリ"
echo "----------------------------------------"
pwd

echo ""
echo "💾 ディスク使用状況"
echo "----------------------------------------"
df -h / | tail -1

echo ""
echo "🧠 メモリ使用状況"
echo "----------------------------------------"
free -h | grep Mem

echo ""
echo "🌍 タイムゾーン情報"
echo "----------------------------------------"
timedatectl | grep "Time zone"

echo ""
echo "✅ レポート完了"
echo "----------------------------------------"
echo "ビルド番号: #${BUILD_NUMBER}"
echo "ジョブ名: ${JOB_NAME}"
```

---

**Windows:**
```cmd
@echo off
echo ╔═══════════════════════════════════════╗
echo ║   Jenkins - システム情報レポート       ║
echo ╚═══════════════════════════════════════╝

echo.
echo 📅 現在日時
echo ----------------------------------------
echo %date% %time%

echo.
echo 🖥️  コンピューター名
echo ----------------------------------------
echo %COMPUTERNAME%

echo.
echo 👤 現在のユーザー
echo ----------------------------------------
echo %USERNAME%

echo.
echo 📂 作業ディレクトリ
echo ----------------------------------------
cd

echo.
echo 🔄 システム起動時刻
echo ----------------------------------------
systeminfo | findstr /C:"System Boot Time"

echo.
echo 💾 ディスク使用状況
echo ----------------------------------------
wmic logicaldisk get caption,size,freespace

echo.
echo ✅ レポート完了
echo ----------------------------------------
echo ビルド番号: %BUILD_NUMBER%
echo ジョブ名: %JOB_NAME%
```

---

### 出力結果（Linux）

```
╔═══════════════════════════════════════╗
║   Jenkins - システム情報レポート       ║
╚═══════════════════════════════════════╝

📅 現在日時
----------------------------------------
2025年 10月 23日 (Thursday) 14:30:15 JST

🖥️  ホスト名
----------------------------------------
jenkins-server

🔄 システム起動時刻
----------------------------------------
2025-10-23 09:00:00

⏱️  稼働時間
----------------------------------------
14:30:15 up  5:30,  1 user,  load average: 0.15, 0.10, 0.08

👤 現在のユーザー
----------------------------------------
jenkins

📂 作業ディレクトリ
----------------------------------------
/var/lib/jenkins/workspace/show-current-time

💾 ディスク使用状況
----------------------------------------
/dev/sda1       50G   25G   23G  53% /

🧠 メモリ使用状況
----------------------------------------
Mem:           8.0Gi       3.2Gi       1.5Gi       0.5Gi       3.3Gi       4.3Gi

🌍 タイムゾーン情報
----------------------------------------
       Time zone: Asia/Tokyo (JST, +0900)

✅ レポート完了
----------------------------------------
ビルド番号: #5
ジョブ名: show-current-time
```

---

## 📊 Jenkins環境変数を使う

Jenkinsは便利な環境変数を提供しています。

### 使用可能な環境変数

| 変数名 | 説明 | 例 |
|--------|------|-----|
| `BUILD_NUMBER` | ビルド番号 | 42 |
| `JOB_NAME` | ジョブ名 | show-current-time |
| `BUILD_ID` | ビルドID | 2025-10-23_14-30-15 |
| `BUILD_URL` | ビルドURL | http://jenkins:8080/job/show-current-time/42/ |
| `WORKSPACE` | ワークスペースパス | /var/lib/jenkins/workspace/show-current-time |
| `JENKINS_HOME` | Jenkins ホームディレクトリ | /var/lib/jenkins |
| `NODE_NAME` | ノード名 | master |

---

### 環境変数を表示するコマンド

**Linux/Mac:**
```bash
#!/bin/bash

echo "=== Jenkins 環境変数 ==="
echo "ビルド番号: ${BUILD_NUMBER}"
echo "ジョブ名: ${JOB_NAME}"
echo "ビルドID: ${BUILD_ID}"
echo "ビルドURL: ${BUILD_URL}"
echo "ワークスペース: ${WORKSPACE}"
echo "Jenkinsホーム: ${JENKINS_HOME}"
echo "ノード名: ${NODE_NAME}"

echo ""
echo "=== すべての環境変数 ==="
env | sort
```

**Windows:**
```cmd
@echo off
echo === Jenkins 環境変数 ===
echo ビルド番号: %BUILD_NUMBER%
echo ジョブ名: %JOB_NAME%
echo ビルドID: %BUILD_ID%
echo ビルドURL: %BUILD_URL%
echo ワークスペース: %WORKSPACE%
echo Jenkinsホーム: %JENKINS_HOME%
echo ノード名: %NODE_NAME%

echo.
echo === すべての環境変数 ===
set
```

---

## 🎨 カラフルな出力（ANSI Color）

ログを色付けして見やすくします。

### プラグインのインストール

```
1. Manage Jenkins → Manage Plugins
2. Available タブで「AnsiColor」を検索
3. チェック → Install
4. Jenkins を再起動
```

---

### ジョブの設定

```
1. ジョブの設定画面を開く
2. Build Environment セクション
3. ✅ Color ANSI Console Output にチェック
4. Save
```

---

### カラーコードを使ったスクリプト

**Linux/Mac:**
```bash
#!/bin/bash

# カラーコード
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
BLUE='\033[0;34m'
PURPLE='\033[0;35m'
CYAN='\033[0;36m'
NC='\033[0m' # No Color

echo -e "${CYAN}╔═══════════════════════════════════════╗${NC}"
echo -e "${CYAN}║${NC}   ${PURPLE}Jenkins - システム情報${NC}           ${CYAN}║${NC}"
echo -e "${CYAN}╚═══════════════════════════════════════╝${NC}"

echo ""
echo -e "${GREEN}✅ 現在日時${NC}"
echo "----------------------------------------"
date '+%Y年 %m月 %d日 %H:%M:%S'

echo ""
echo -e "${BLUE}ℹ️  システム情報${NC}"
echo "----------------------------------------"
echo -e "ホスト名: ${YELLOW}$(hostname)${NC}"
echo -e "ユーザー: ${YELLOW}$(whoami)${NC}"

echo ""
echo -e "${RED}⚠️  リソース使用状況${NC}"
echo "----------------------------------------"
uptime

echo ""
echo -e "${GREEN}✅ 完了${NC}"
```

---

## 🔍 トラブルシューティング

### 問題1: dateコマンドが見つからない

**エラー:**
```
/bin/bash: date: command not found
```

**解決方法:**
```bash
# フルパスを指定
/bin/date

# または which で場所を確認
which date
```

---

### 問題2: タイムゾーンが正しくない

**症状:**
```
出力時刻がUTCになっている
```

**解決方法（Linux）:**
```bash
# タイムゾーンを設定
export TZ='Asia/Tokyo'
date

# または
TZ='Asia/Tokyo' date
```

---

### 問題3: 日本語が文字化けする

**解決方法:**
```bash
# ロケールを設定
export LANG=ja_JP.UTF-8
export LC_ALL=ja_JP.UTF-8

date '+%Y年 %m月 %d日 %A'
```

---

## ✅ チェックリスト

- [ ] Jenkinsサーバーにアクセスできる
- [ ] 新しいジョブを作成できた
- [ ] ビルドステップを追加できた
- [ ] ジョブを実行できた
- [ ] Console Outputを確認できた
- [ ] 現在時刻が表示された
- [ ] タイムスタンプを追加できた（オプション）
- [ ] 定期実行を設定できた（オプション）

---

## 🎓 次のステップ

このジョブができたら、次は以下に挑戦してみましょう：

1. **Gitリポジトリと連携**
   - GitHubからコードを取得
   - コミット後に自動ビルド

2. **プログラムのビルド**
   - Node.js、Python、Javaなど
   - テストの実行

3. **Dockerイメージのビルド**
   - Dockerfileからイメージ作成
   - レジストリにプッシュ

4. **Pipelineに移行**
   - Jenkinsfileでコード化
   - より複雑なワークフロー

---

## 📚 参考リンク

- [Jenkins公式ドキュメント](https://www.jenkins.io/doc/)
- [Jenkinsチュートリアル](https://www.jenkins.io/doc/tutorials/)
- [Jenkins環境変数一覧](https://wiki.jenkins.io/display/JENKINS/Building+a+software+project)

---

💬 うまくいきましたか？コメントで教えてください！

#Jenkins #初心者 #CI/CD #ジョブ #date #チュートリアル #新人エンジニア