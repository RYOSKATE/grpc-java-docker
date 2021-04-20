# grpc-java-docker

grpc-javaのクライアント―サーバ通信をDockerコンテナ間で動作確認するためのサンプル

```bash
git clone --recursive https://github.com/RYOSKATE/grpc-java-docker.git
cd grpc-java-docker
```

## build

イメージをビルドします。

```bash
docker build -t grpc-java-server:1.0 -f Dockerfile.server .
docker build -t grpc-java-client:1.0 -f Dockerfile.client .
```

- `-t`・・・イメージのタグ名とバージョン
- `-f`・・・Dockerfile指定

## network

コンテナ間通信用のネットワークを作っておきます。

```bash
docker network create grpc-java-network
```

## run

イメージを元にコンテナを作り実行します。

```bash
ryoskate:~/grpc-java-docker$ docker run --name grpc-java-server --network grpc-java-network grpc-java-server:1.0
Apr 20, 2021 12:14:05 PM io.grpc.examples.helloworld.HelloWorldServer start
INFO: Server started, listening on 50051
```

```bash
ryoskate:~/grpc-java-docker$ run --name grpc-java-client --network grpc-java-network grpc-java-client:1.0
Apr 20, 2021 12:20:36 PM io.grpc.examples.helloworld.HelloWorldClient greet
INFO: Will try to greet world ...
Apr 20, 2021 12:20:37 PM io.grpc.examples.helloworld.HelloWorldClient greet
INFO: Greeting: Hello world
```

- `--name`・・・コンテナ名指定
- `--network`・・・接続するネットワーク指定
- `-d`・・・バックグラウンドで実効

## start stop

一度runしたコンテナを起動したり停止したりします。

```bash
docker start -a grpc-java-server
docker start -a grpc-java-client

docker stop grpc-java-server
docker stop grpc-java-client
```

- `--a`・・・標準出力を表示

### Docker化せずに動作確認

#### Windwos

```bat
cd grpc-java/examples
gradlew.bat installDist -PskipAndroid=true -PskipCodegen=true
build\install\examples\bin\hello-world-server.bat
build\install\examples\bin\hello-world-client.bat
```

#### Linux

```bash
cd grpc-java/examples
gradlew installDist -PskipAndroid=true -PskipCodegen=true
build/install/examples/bin/hello-world-server
build/install/examples/bin/hello-world-client
```
