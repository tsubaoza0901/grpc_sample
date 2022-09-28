# grpc_sample

## 起動方法

### ① Server 側

1. Docker イメージのビルド&コンテナの起動

```
$ cd greeter_server
$ docker-compose up -d --build
```

2. サーバー起動

```
$ docker exec -it grpc-sample-server-go bash
```

```
/usr/src/app# go run main.go
```

### ② Client 側

1. Docker イメージのビルド&コンテナの起動

```
$ cd greeter_client
$ docker-compose up -d --build
```

2. サーバー起動

```
$ docker exec -it grpc-sample-client-go bash
```

```
/usr/src/app# go run main.go
```

※想定出力結果

```
2022/00/00 00:00:00 Greeting: Hello world
2022/00/00 00:00:00 Greeting: Hello again world
```

なお、Client 側で以下のように command-line argument を設定することも可能

```
/usr/src/app# go run main.go --name=Alice
```

※想定出力結果

```
2022/00/00 00:00:00 Greeting: Hello Alice
2022/00/00 00:00:00 Greeting: Hello again Alice
```

## 更新方法

1. `.proto` ファイルの内容更新

ex）helloworld.proto

```
service Greeter {
    ...

  // Sends another greeting
  rpc SayHelloAgain (HelloRequest) returns (HelloReply) {}
}
```

2. gRPC コードの再生成

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

### 注）

Client と Server が分かれているため、片方の.proto に変更を加えた場合は、同様の内容で Client もしくは Server の .proto ファイルを更新し、コードの再生成を行うか、片方で生成した内容をコピーするなどして Client もしくは Server に内容を反映させる必要あり。

なお、grpc_sample 直下にも helloworld ディレクトリがあり、その中にも.proto ファイル等があるが、Client と Server が分かれている現状では使用していない。

# 参考

- gRPC 公式ドキュメント
  https://grpc.io/

- gRPC examples
  https://github.com/grpc/grpc-go/tree/master/examples/helloworld

# 関連

- Docker で protoc を行って開発を楽にする
  https://zenn.dev/mitsugu/articles/0323811005f233
