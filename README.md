## 概要
Envoyは、クラウドネイティブアプリケーション向けに設計されたオープンソースおよびサービスプロキシです。  
元々Lyftで構築されたEnvoyは、単一のサービスとアプリケーション向けに設計された高性能C++分散プロキシであり、  
大規模なマイクロサービス「サービスメッシュ」アーキテクチャ用に設計された通信バスと「ユニバーサルデータプレーン」です。  

AIONは、エッジコンピューティング上のマイクロサービスアーキテクチャにおいて、ロードバランサー、ネットワーク制御プロキシとして、Envoyをaion-coreならびにKubernetes環境上で採用しています。  
`envoyproxy/envoy:v1.16-latest`

## Envoyの設定ファイル
[aion-core](https://github.com/latonaio/aion-core)とaion-coreの周辺環境を整備する[aion-core-manifests](aion-core-manifests)で起動するEnvoyの設定を行います。

変更が必要なファイルの箇所は以下の通りです。必要に応じて該当箇所を変更してください。


```

aion-core
- pkg/k8s/pod.go
- template/envoy.yaml

aion-core-manifest
- Makefile
- generated/default.yml
- generated/prj.yml
- template/bases/status-kanban/deployment.yml
- template/bases/kanban-replicator/deployment.yml
- template/bases/send-anything/deployment.yml
- template/bases/service-broker/deployment.yml

```

例えば、generated/default.ymlには、初期値で下記のように記載されています。   
```
---
apiVersion: v1
kind: Service
metadata:
  labels:
    run: aion-sendanything
  name: aion-sendanything
  namespace: default
spec:
  ports:
  - name: envoy-grpc
    nodePort: 30100
    port: 10000
    protocol: TCP
    targetPort: 10000
  - name: envoy-admin
    port: 10001
    protocol: TCP
    targetPort: 10001
  selector:
    run: aion-sendanything
  type: NodePort
---
apiVersion: v1
kind: Service
metadata:
  labels:
    run: aion-servicebroker
  name: aion-servicebroker
  namespace: default
spec:
  ports:
  - name: envoy-admin
    port: 10001
    protocol: TCP
    targetPort: 10001
  - name: envoy-grpc
    nodePort: 31000
    port: 10000
    protocol: TCP
    targetPort: 10000
  selector:
    run: aion-servicebroker
  type: NodePort
---
apiVersion: v1
kind: Service
metadata:
  labels:
    run: aion-statuskanban
  name: aion-statuskanban
  namespace: default
spec:
  ports:
  - name: grpc
    port: 11010
    protocol: TCP
    targetPort: 11010
  - name: envoy-grpc
    port: 10000
    protocol: TCP
    targetPort: 10000
  - name: envoy-admin
    port: 10001
    protocol: TCP
    targetPort: 10001
  selector:
    run: aion-statuskanban
  type: ClusterIP
---
```