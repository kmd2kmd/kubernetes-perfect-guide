# GKEクラスターの作成

## 前提

- GCPアカウントを所有している
  - クレジットカードを登録している

## Cloud Shellを起動

GCPにログインして上部メニュー右側のShellのアイコンをクリックします｡

## チュートリアルの読み込み

このチュートリアルを読み込みます｡下記のコマンドをCloud Shellで実行してください｡

```bash
git clone https://github.com/kmd2kmd/kubernetes-perfect-guide.git
cd kubernetes-perfect-guide
cloudshell launch-tutorial -d tutorial-deploy-gke.md
```

## コードエディタの起動

Cloud Shellの灰色のメニューバーの鉛筆のアイコンをクリックします｡

新しいタブでコードエディタとCloud Shellが起動します｡
Cloud Shellが新しいタブで開いているので､×ボタンを押して閉じてください｡

## プロジェクトの作成

GCPのプロジェクトを作成します｡プロジェクトのIDは一意である必要がある為､ランダム数列をサフィックスにしています｡

```bash
PROJECT_ID=k8s-pg-$RANDOM && echo $PROJECT_ID
gcloud projects create $PROJECT_ID
gcloud config set project $PROJECT_ID
```

既存のプロジェクトを利用したい場合は､```PROJECT_ID```を設定した後に下記のコマンドを使用してください｡

```bash
gcloud config set project $PROJECT_ID
```

## 必要なAPIの有効化

必要になるAPIを有効化します。この処理には約5分かかります｡

```bash
gcloud services enable \
  cloudapis.googleapis.com \
  container.googleapis.com \
  containerregistry.googleapis.com \
  cloudbuild.googleapis.com
```

## GKE クラスターの作成

GKEのクラスターを作成します。

```bash
gcloud container clusters create k8s-pg-cluster --enable-ip-alias --num-nodes=1 --zone=us-west1-b --cluster-version=1.11.6-gke.3 --async
```

### クラスターの確認

作成されてたクラスターを確認します。

```bash
gcloud container clusters list
```

[GKEのコンソール](https://console.cloud.google.com/kubernetes/list)でも確認することができます。

しばらくすると、STATUSがRUNNINGになります。

## GKE クラスターに接続

```bash
gcloud container clusters get-credentials k8s-pg-cluster --zone us-west1-b
```

## GKE クラスターの削除

```bash
gcloud container clusters delete k8s-pg-cluster --zone=us-west1-b --async
```