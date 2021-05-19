# docker_network_sample

## 概要
コンテナ間で通信するために、docker-composeのnetworksを使うことができます。

コードは[こちら](https://github.com/Qstairs/docker_network_sample)

## テスト準備
```shell
docker-compose build
docker-compose up -d
```

## テスト用docker-compose

```
version: '3.2'
services:
  test_service_one:
    image: ubuntu:latest
    container_name: test_service1
    build: ./
    networks:
      - test_network
    tty: true
    stdin_open: true
    privileged: true
  test_service_two:
    image: ubuntu:latest
    container_name: test_service2
    build: ./
    networks:
      - test_network
    tty: true
    stdin_open: true
    privileged: true
networks:
  test_network:
```


## テスト実行結果
pingを実行すると双方のコンテナ内から通知できていることが分かります。

```
user@wsl:~/develop/dockers/docker_network_sample$ docker exec -it test_service1 ping test_service2 -c 5
PING test_service2 (172.18.0.3) 56(84) bytes of data.
64 bytes from test_service2.docker_network_sample_test_network (172.18.0.3): icmp_seq=1 ttl=64 time=0.157 ms
64 bytes from test_service2.docker_network_sample_test_network (172.18.0.3): icmp_seq=2 ttl=64 time=0.046 ms
64 bytes from test_service2.docker_network_sample_test_network (172.18.0.3): icmp_seq=3 ttl=64 time=0.060 ms
64 bytes from test_service2.docker_network_sample_test_network (172.18.0.3): icmp_seq=4 ttl=64 time=0.097 ms
64 bytes from test_service2.docker_network_sample_test_network (172.18.0.3): icmp_seq=5 ttl=64 time=0.041 ms

--- test_service2 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4130ms
rtt min/avg/max/mdev = 0.041/0.080/0.157/0.043 ms
user@wsl:~/develop/dockers/docker_network_sample$ docker exec -it test_service2 ping test_service1 -c
5
PING test_service1 (172.18.0.2) 56(84) bytes of data.
64 bytes from test_service1.docker_network_sample_test_network (172.18.0.2): icmp_seq=1 ttl=64 time=0.286 ms
64 bytes from test_service1.docker_network_sample_test_network (172.18.0.2): icmp_seq=2 ttl=64 time=0.040 ms
64 bytes from test_service1.docker_network_sample_test_network (172.18.0.2): icmp_seq=3 ttl=64 time=0.042 ms
64 bytes from test_service1.docker_network_sample_test_network (172.18.0.2): icmp_seq=4 ttl=64 time=0.044 ms
64 bytes from test_service1.docker_network_sample_test_network (172.18.0.2): icmp_seq=5 ttl=64 time=0.082 ms

--- test_service1 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4152ms
rtt min/avg/max/mdev = 0.040/0.098/0.286/0.094 ms
```

## 最後に
docker-composeのnetworksを使うことでコンテナ間の通信ができることが分かりますね。

