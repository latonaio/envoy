## 概要
Envoyは、クラウドネイティブアプリケーション向けに設計されたオープンソースおよびサービスプロキシです。  
元々Lyftで構築されたEnvoyは、単一のサービスとアプリケーション向けに設計された高性能C++分散プロキシであり、  
大規模なマイクロサービス「サービスメッシュ」アーキテクチャ用に設計された通信バスと「ユニバーサルデータプレーン」です。  

AIONは、エッジコンピューティング上のマイクロサービスアーキテクチャにおいて、ロードバランサー、ネットワーク制御プロキシとして、Envoyをaion-coreならびにKubernetes環境上で採用しています。  
`envoyproxy/envoy:v1.16-latest`

## Envoyのバージョン設定
[aion-core](https://github.com/latonaio/aion-core)とaion-coreの周辺環境を整備する[aion-core-manifests](aion-core-manifests)で起動するEnvoyのバージョンを設定することができます。

変更が必要なファイルの箇所は以下の通りです。grep等のツールで該当箇所を変更してください。



```

aion-core
- pkg/k8s/pod.go

aion-core-manifest
- Makefile
- template/bases/status-kanban/deployment.yml
- template/bases/kanban-replicator/deployment.yml
- template/bases/send-anything/deployment.yml
- template/bases/service-broker/deployment.yml
- generated/default.yml
- generated/prj.yml
```

