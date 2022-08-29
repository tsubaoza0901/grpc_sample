# grpc_sample

## 使用方法

### 1. Dockerイメージのビルド&コンテナの起動

```
$ docker-compose up -d --build
```

### 2. サーバー起動

1. アプリケーションコンテナ内に移動

```
$ docker exec -it grpc-sample-go bash
```

2. サーバー起動

```
/usr/src/app# go run greeter_server/main.go
```


3. クライアント起動   
別のターミナルに移動して以下を実行

```
/usr/src/app# go run greeter_client/main.go
```

※想定出力結果

```
2022/00/00 00:00:00 Greeting: Hello world
```


# 参考
- gRPC 公式ドキュメント
https://grpc.io/

- gRPC examples
https://github.com/grpc/grpc-go/tree/master/examples/helloworld