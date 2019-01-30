# 第5章 Workloadsリソース

## 5.2.2 Podの作成

```bash
# Podの作成
kubectl apply -f sample-pod.yaml
```

```bash
# Podの一覧表示
kubectl get pods
```

## 5.2.3 2つのコンテナを内用したPodの作成

2つのコンテナを内用したPodの作成

```bash
kubectl apply -f sample-2pod.yaml
```

2つのコンテナを内包したPodの確認

```bash
kubectl get pods
```

ポートを衝突させたPodの作成

```bash
kubectl apply -f sample-2pod-fail.yaml
```

Podのステータスがエラーになっている

```bash
kubectl get pods
```

Podのログを確認(複数のコンテナを内包する場合は特定のコンテナを指定可能)

```bash
kubectl logs sample-2pod-fail -c nginx-container-113
```

## 5.2.4 コンテナへのログインとコマンドの実行

コンテナ上で/bin/bashを起動

```bash
kubectl exec -it sample-pod /bin/bash
# 確認に使用するパッケージのインストール
apt-get update && apt-get -y install iproute procps
```

コンテナ内からのIPアドレスの確認

```bash
ip a | grep "inet"
```

コンテナ内からのbind(listen)しているポートの確認

```bash
ss -napt | grep LISTEN
```

コンテナ内からのプロセスの確認

```bash
ps aux
```

コンテナ内から抜ける

```bash
exit
```

コンテナでlsコマンドを実行

```bash
kubectl exec -it sample-pod ls
```

複数コンテナを内包するPodの場合は特定コンテナを指定可能

```bash
kubectl exec -it sample-2pod -c nginx-container ls
```

オプションを含むコマンドの実行

```bash
kubectl exec -it sample-pod -- ls --all --classify
```

パイプなど特殊な文字列が膨れる場合の実行

```bash
kubectl exec -it sample-pod -- sh -c "ls --all --classify | grep media"