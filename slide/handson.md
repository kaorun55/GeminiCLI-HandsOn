---
marp: true
theme: uncover
paginate: true
---

# Gemini CLI ハンズオン
## AIに指示してToDoアプリを開発しよう

---

## 1. このハンズオンのゴール

- **Gemini CLI** の基本的な使い方を体験する。
- AIに指示を出し、**コード生成**や**コマンド実行**をさせながらアプリケーションを開発するプロセスを理解する。
- AIが自律的にツールを使う仕組み **MCP (Model-as-a-Controller)** の裏側を覗いてみる。

---

## 2. セットアップ：Gemini CLIのインストール

**Gemini CLI** は、Google AIのモデルと対話しながら、ローカル環境で直接コマンドを実行したり、ファイルを操作したりできる強力なツールです。

**▼ インストール**
以下のコマンドを実行して、Gemini CLIをインストールします。
（※環境によっては `pip` の代わりに `pip3` を使用します）

```bash
pip install google-generativeai
```

---

## 3. ハンズオン：AIにToDoアプリを作���せよう

ここからは、実際にGemini CLIに指示を出して、コマンドラインで動作するToDoアプリを開発していきます。

**私（Gemini）が、あなたに代わって開発作業を行います。**

---

### Step 1: プロジェクトの準備

まず、開発の土台を準備します。

**▼ あなたが出す指示**
「`_demo` という名前で作業フォルダを作って、その中に `src` フォルダも作って」

**▼ 私（Gemini）の動き**
1. 指示を理解し、`mkdir` コマンドを実行する必要があると判断します。
2. `run_shell_command` ツールを呼び出し、`mkdir _demo` と `mkdir _demo/src` を実行します。

---

### Step 2: ToDo追加機能の実装

次に、アプリの心臓部である「タスク追加機能」のコードを生成させます。

**▼ あなたが出す指示**
「PythonでToDoアプリのコードを書いて。`add` コマンドでタスクを追加できるようにして。ファイル名は `_demo/src/todo.py` にしてね」

**▼ 私（Gemini）の動き**
1. 指示に基づき、Pythonのコードを生成します。
2. `write_file` ツールを呼び出し、生成したコードを指定されたパスに保存します。

---

### Step 3: 動作確認

正しく動くか、すぐに確認します。

**▼ あなたが出す指示**
「今作ったアプリで『牛乳を買う』というタスクを追加してみて」

**▼ 私（Gemini）の動き**
1. `run_shell_command` ツールを呼び出します。
2. `python _demo/src/todo.py add "牛乳を買う"` というコマンドを組み立てて実行します。
3. `_demo` フォルダに `tasks.json` が作成され、タスクが保存されます。

---
<!-- _footer: "以降、`list`, `done`, `delete` 機能も同様に、指示を出すだけで実装が進んでいきます。" -->

## 4. 【発展】GUI化とリアルタイム更新

CUIだけでは物足りない？
GUIを追加し、さらにMCPからの操作をリアルタイムで反映させてみましょう。

---

### Step 4: GUIの追加

**▼ あなたが出す指示**
「これまでのアプリに、TkinterでGUIを追加して」

**▼ 私（Gemini）の動き**
1. Tkinterを使ったGUIアプリのコードを生成���ます。
2. `write_file` で `_demo/src/todo_gui.py` を作成します。
3. `run_shell_command` で `python _demo/src/todo_gui.py` を実行し、デスクトップにウィンドウを表示させます。

![GUIイメージ](https://i.imgur.com/eJc0h2E.png) <!-- 画像はダミーです -->

---

### Step 5: リアルタイム更新

**GUIアプリを開いたまま**、CLIからタスクを追加するとどうなるでしょう？
`watchdog` ライブラリを導入し、ファイル変更を監視させます。

**▼ あなたが出す指示**
「GUIを開いたまま、CLIで『MCPからのテスト』タスクを追加して」

**▼ 私（Gemini）の動き**
1. `run_shell_command` で `python _demo/src/todo.py add ...` を実行。
2. `tasks.json` が変更される。
3. `watchdog` が変更を検知し、GUIアプリの表示が**自動で更新される！**

---

## 5. 仕組みの解説：MCPとツール定義

なぜ、AIは `mkdir` や `write_file` を使えるのでしょうか？
それは、AIが**「ツール定義」**という取扱説明書を持っているからです。

```json
{
  "name": "run_shell_command",
  "description": "このツールは、指定されたシェルコマンドを実行します...",
  "parameters": {
    "type": "object",
    "properties": {
      "command": {
        "type": "string",
        "description": "実行する正確なbashコマンド。"
      },
      // ...
    }
  }
}
```
AIは、あなたの指示とこの `description` を照らし合わせ、最適なツールと引数を判断しているのです。

---

## まとめ

- Gemini CLIを使えば、**自然言語で指示するだけ**でアプリケーション開発が可能。
- **コード生成** (`write_file`) と**コマンド実行** (`run_shell_command`) が強力な武器になる。
- AIがツールを使う裏側には、**MCP (Model-as-a-Controller)** と **ツール定義JSON** が存在する。

**あなたもAIをコントローラーにして、開発を加速させましょう！**
