# grpc_sample

## 起動方法

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

### 3. クライアント起動   

1. 新たなターミナルタブの立ち上げ

2. アプリケーションコンテナ内に移動

```
$ docker exec -it grpc-sample-go bash
```

3. クライアント起動

```
/usr/src/app# go run greeter_client/main.go
```

※想定出力結果

```
2022/00/00 00:00:00 Greeting: Hello world
2022/00/00 00:00:00 Greeting: Hello again world
```

なお、以下のようにcommand-line argumentを設定することも可能

```
/usr/src/app# go run greeter_client/main.go --name=Alice
```

※想定出力結果

```
2022/00/00 00:00:00 Greeting: Hello Alice
2022/00/00 00:00:00 Greeting: Hello again Alice
```

## 更新方法

1. `.proto` ファイルの内容更新
2. gRPCコードの再生成

```
/usr/src/app# protoc --go_out=. --go_opt=paths=source_relative \
    --go-grpc_out=. --go-grpc_opt=paths=source_relative \
    helloworld/helloworld.proto
```

3. 実装コードの更新

ex）greeter_server.go

```go
func (s *server) SayHelloAgain(ctx context.Context, in *pb.HelloRequest) (*pb.HelloReply, error) {
        return &pb.HelloReply{Message: "Hello again " + in.GetName()}, nil
}
```

ex）greeter_client.go

```go
r, err = c.SayHelloAgain(ctx, &pb.HelloRequest{Name: *name})
if err != nil {
        log.Fatalf("could not greet: %v", err)
}
log.Printf("Greeting: %s", r.GetMessage())
```



# 参考
- gRPC 公式ドキュメント
https://grpc.io/

- gRPC examples
https://github.com/grpc/grpc-go/tree/master/examples/helloworld

# 関連
- Dockerでprotocを行って開発を楽にする
https://zenn.dev/mitsugu/articles/0323811005f233