---
title: "チャットで楽々検索！ Azure AI Search と OpenAI を組み合わせた独自ドキュメント検索サンプルのご紹介"
emoji: "🐨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Azure", "OpenAI", "RAG","Python"]
published: true
publication_name: "microsoft"
---

# はじめに
皆さん、こんにちは！
弊社では、OpenAI を活用して様々なシナリオに対応するサンプルを作成・公開しています。
私のチームのプロジェクトで独自のデータを検索することのできる[サンプル](https://github.com/Azure-Samples/jp-azureopenai-samples/tree/main/5.internal-document-search)の作成に携わっておりますので、その内容をご紹介いたします。


# 概要
## どんなサンプルか？
![image](https://github.com/marumaru1019/github-image/assets/70362624/54a75c5d-00a2-4c91-ac9c-9feff02f316e "文章検索サンプルのアーキテクチャ")
文章検索サンプルのアーキテクチャ

本サンプルは、Azure AI Search と OpenAI を組み合わせて、独自のドキュメントを検索することができます！
独自のドキュメントを Azure AI Search のインデックスに登録しておき、OpenAI を介して質問をすることで、ドキュメントの内容にも基づいた回答を得ることができます。 
このサンプルは、企業の情報管理を効率化し、社内の知識共有を容易にすることを目的としており、以下のようなシナリオで活用することができます。

1. **人事文書の問い合わせ**
   - 従業員が自然言語で社内の人事ポリシーやガイドラインを検索。

2. **市場調査の分析**
   - マーケティングチームが市場調査レポートを迅速に調べ、関連データやトレンドを把握。

3. **財務データの取得**
   - 財務部門が多数のレポートから特定の財務情報を抽出。

4. **プロジェクト管理**
   - プロジェクトマネージャーがプロジェクト文書や過去の分析から特定の詳細を見つける。

5. **カスタマーサポートの向上**
   - カスタマーサポートチームが過去の事例研究やサポート文書を調べ、現在の顧客問題の解決策を見つける。

これら以外にも Azure OpenAI を使ってアプリケーションを開発したいな～ってなった際に、どういう風にカスタマイズされているのかを参考にすることができます。

ChatGPT のような対話型のインターフェースのようになっており、本家の ChatGPT と同様に、チャット形式で質問をすることができます。


## どんなことができるのか？
本サンプルの画面は 2 つで、 **企業向け Chat** と **社内文章検索**です。

### 企業向け Chat
![image](https://github.com/marumaru1019/github-image/assets/70362624/1732ad96-707e-4206-a44c-ae911182f229)
チャット画面

![image](https://github.com/marumaru1019/github-image/assets/70362624/266e9d7b-23f7-4b0c-9cd7-dfa0aad652e0)
チャット画面のパラメータ設定

この画面では、ChatGPT のように、単純に OpenAI がもともと学習しているデータに基づいて回答を得ることができます。

右上の歯車マークの設定をクリックすることで、パラメータの値を変更することができます。

#### GPT Model
OpenAI の学習済みモデルを選択することができます。
選択できるモデルは、以下の通りです。(事前にデプロイされている必要がある点に注意です)
- gpt-35-turbo
- gpt-35-turbo-16k
- gpt-4
- gpt-4-32k

#### System Prompt
OpenAIのモデルを使用する際に、モデルに与える最初の指示または質問のことです。
これは、モデルが応答を生成するためのコンテキストや指針を提供し、ユーザーのニーズや意図に合わせた回答を生成するために使用されます。
<!-- TODO: system prompt の具体例とふるまいの違いを示す画像を追加する or 別記事で紹介する -->

#### Temperature
生成されるテキストのランダム性を制御します。
低い温度（0に近い）は予測可能で保守的な応答を、高い温度（1に近い）はランダム性と創造性が高い応答を生み出します。

### 社内文章検索
![image](https://github.com/marumaru1019/github-image/assets/70362624/7aad72a9-9956-4988-bee2-435b1b8a8947)
社内文章検索画面

![image](https://github.com/marumaru1019/github-image/assets/70362624/205c81f1-aaf4-4ba8-9805-8ba307c57be9)
社内文章検索のパラメータ設定


![image](https://github.com/marumaru1019/github-image/assets/70362624/16126cc0-47c9-4a5d-864e-1ab350ff4ae1)
ドキュメントの引用

この画面では、質問をすると、事前に登録したドキュメントの中から、質問に関連するドキュメントを検索してその内容をもとに回答を得ることができます。
サンプルデータには厚生労働省のモデル就業規則を使用しています。
引用元から、ドキュメントの内容を確認することもできます。

右上の歯車マークの設定をクリックすることで、企業向け Chat と同様にパラメータの値を変更することができます。
Exclude Category、Use semantic ranker for retrieval、Use query-contextual summaries instead of whole documents に関しては、挙動が安定していないため紹介していません。

#### GPT Model
OpenAI の学習済みモデルを選択することができます。
選択できるモデルは、以下の通りです。(事前にデプロイされている必要がある点に注意です)
- gpt-35-turbo
- gpt-35-turbo-16k
- gpt-4
- gpt-4-32k

#### Temperature
生成されるテキストのランダム性を制御します。
低い温度（0に近い）は予測可能で保守的な応答を、高い温度（1に近い）はランダム性と創造性が高い応答を生み出します。

#### Retrieve this many documents from search
回答を生成する際に参考にするドキュメントの数を指定します。
この値が大きければ、より多くのドキュメントを参照して回答を生成することができますが、一方で余計なドキュメントを参照してしまう可能性もあります。


# 使い方
ここでは、本サンプルの使い方をご紹介します。

## 事前準備
以下のコマンドを使用して、本サンプルのリポジトリをクローンしてください。

```pwsh
git clone https://github.com/Azure-Samples/jp-azureopenai-samples.git
```
本サンプルは、ローカル及び事前に用意してある devcontainer の環境で実行することができます。
どちらのオプションを選択するかで、事前準備が異なりますので、以下の手順に従って準備を行ってください。

※ 本記事では、環境差異をなくすために devcontainer (for pwsh) を使用しています。

### ローカルで実行する場合
以下のツールをインストールしてください。

| ツール名            | バージョン | ダウンロードリンク |
|-------------------|------------|------------------|
| Azure Developer CLI | 1.0.2以降   | [ダウンロード](https://learn.microsoft.com/en-us/azure/developer/azure-developer-cli/install-azd?tabs=winget-windows%2Cbrew-mac%2Cscript-linux&pivots=os-windows) |
| Azure CLI | 2.50.0以降   | [ダウンロード](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) |
| Python | 3.11以降   | [ダウンロード](https://www.python.org/downloads/) |
| Node.js | 14.18以降   | [ダウンロード](https://nodejs.org/en/download/) |
| Git |   | [ダウンロード](https://git-scm.com/downloads) |
| Powershell 7+ (pwsh) ※ Windows 環境の場合 | 7.1.5以降   | [ダウンロード](https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell?view=powershell-7.1) |

### devcontainer で実行する場合
以下のツールをインストールしてください。
| ツール名            | バージョン | ダウンロードリンク |
|-------------------|------------|------------------|
| Visual Studio Code |    | [ダウンロード](https://code.visualstudio.com/Download) |
| Docker |    | [ダウンロード](https://docs.docker.com/desktop/install/windows-install) |

続いて、以下の手順で devcontainer を起動してください。

1. サンプルアプリのリポジトリまで移動してください。
    ```pwsh
    cd path/to/jp-azureopenai-samples/5.internal-document-search
    ```

2. Visual Studio Code を起動してください。
    ```pwsh
    code .
    ```

3. Extensions から **Dev Containers** をインストールしてください。
    ![image](https://github.com/marumaru1019/github-image/assets/70362624/8af4133b-05fd-4258-81d9-9956e8c0e667)

4. Visual Studio Code の左下にある **><** みたいになっている部分をクリックしてください。
    ![image](https://github.com/marumaru1019/github-image/assets/70362624/5a136a8a-8697-4fbc-a167-a429ff4d64ca)

5. Reopen in Container をクリックしてください。
    ![image](https://github.com/marumaru1019/github-image/assets/70362624/2b7de313-7f75-4075-ab1a-cbc3b92cb6ef)

6. devcontainer のビルドが始まります。10 分程度待つとビルドが完了して、以下のような画面になります。
    ![image](https://github.com/marumaru1019/github-image/assets/70362624/a5f21728-318a-427e-af4f-e6b0edbb5d8d)


::::details dev container とは？
:::message
dev container とは、VS Code で開発する際に、開発環境をコンテナとして提供する機能です。
コードベースでの作業に必要なツール、ライブラリ、ランタイムを含みフルアクセスが可能になっており、ローカル環境にインストールする必要がないため、環境構築の手間を省くことができます。
異なる環境で開発する際にも、コンテナを共有することで、環境の差異による問題を回避することができるので、開発環境の構築に時間をかけたくない方におすすめです。
:::
::::


## デプロイ方法
1. 以下のコマンドで、Azure へのログインを行ってください。
    ```pwsh
    azd auth login
    ```
    コマンドを打つと、ブラウザが開き、Azure へのログインを求められます。
    使用するアカウントでログインしてください。

    ログインが完了すると、以下のようなメッセージが表示されます。
    ```pwsh
    Logged in to Azure.
    ```


2. 以下のコマンドで、Azure へのデプロイを行ってください。
    ```pwsh
    azd up
    ```

    1. `? Enter a new environment name: [? for help]` と表示されたら、任意の環境名を入力してください。これがもとにリソースグループが `rg-環境名` のように作成されます。
    2. `? Select an Azure Subscription to use:  [Use arrows to move, type to filter]` と表示されたら、使用する Azure サブスクリプションを選択してください。
    3. `? Select an Azure location to use:  [Use arrows to move, type to filter]` と表示されたら、使用する Azure リージョンを選択してください。ここで選択したリージョンにリソースが作成されます。
    4. `? Enter a value for the 'vmLoginPassword' infrastructure parameter:` と表示されたら、空のまま `Enter` を入力してください。この設定は閉域網でサンプルアプリを実行する際に必要になりますが、今回はパブリックアクセスを許可するため、設定する必要はありません。\
    閉域網でのデプロイは、後日に別記事にて紹介いたします。
    5. `? Save the value in the environment for future use (y/N)` と表示されたら、`Enter` を入力してください。
    6. デプロイが完了すると、以下のようなメッセージが表示されます。
    メッセージ内に、アプリケーションの URL が記載されていますので、そちらのリンクをクリックしてブラウザで開いてください。
        ```pwsh
        Deploying services (azd deploy)

        (✓) Done: Deploying service backend
        - Endpoint: <アプリケーションの URL>


        SUCCESS: Your application was provisioned and deployed to Azure in 33 minutes 1 second.
        ....
        ```
    7. ブラウザでアプリケーションが開けて、何かしらチャットが機能していればデプロイ成功です！

## ローカルでの起動方法
ローカルでの実行は以下手手順に従って実行してください。
1. Azure Portal から、作成されたリソースグループ内にアクセスしてください。
作成されたリソースから以下の値を控えておいてください。

| リソース名            | 値 |
|-------------------|------------|
| Application Insights | 接続文字列 |
| Cosmos DB | アクセスキー |
2. `.azure/環境名/.env` ファイルの末尾に以下を追加してください。
    ```.env
    APPLICATIONINSIGHTS_CONNECTION_STRING=<Application Insights の接続文字列>
    COSMOSDB_KEY=<Cosmos DB のアクセスキー>
    ```
3. `src/backend/approachs/chatlogging.py` の 19 行目を以下のように書き換えてください。
    ```diff.js:src/backend/approachs/chatlogging.py
    - credential = DefaultAzureCredential()
    + credential = key
    ```
4. 以下のコマンドで、Azure へのログインを行ってください。
    ```pwsh
    az login
    ```
5. `src` ディレクトリ内に、ローカル実行するためのスクリプトが用意されていますので、こちらを実行してください。
    ```pwsh
    cd src
    ./start.ps1
    ```
6. ブラウザで `localhost:5000` にアクセスして、アプリケーションが開けて、何かしらチャットが機能していればローカルでの起動成功です！

# まとめ
本記事では、弊社で提供している Azure AI Search と OpenAI を組み合わせて、独自のドキュメントを検索することができるサンプルについてご紹介しました。

本サンプルをぜひデプロイしてみて、独自データを活用した OpenAI のアプリケーション作成の参考にしてください！

## お問い合わせ
本サンプルを使用するうえで不具合や改善点等フィードバックありましたら、issue にてお気軽にご連絡ください！

https://github.com/Azure-Samples/jp-azureopenai-samples



