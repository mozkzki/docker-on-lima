# Docker on lima

limaでDocker Engineを動かす。

- 参考
  - [Docker Desktop 無しで Docker を使う with lima on Mac - cangoxina](https://korosuke613.hatenablog.com/entry/2021/09/18/docker-on-lima#DOCKER_HOST-%E7%92%B0%E5%A2%83%E5%A4%89%E6%95%B0%E3%82%92%E8%A8%AD%E5%AE%9A%E3%81%99%E3%82%8B--sshconfig-%E3%82%92%E8%A8%AD%E5%AE%9A%E3%81%99%E3%82%8B20211022-%E8%BF%BD%E8%A8%98)
- 下記をベースにlima VMを作る
  - [examples/docker.yaml](https://github.com/lima-vm/lima/blob/master/examples/docker.yaml)

## Docker入り lime VM作成

```sh
brew install lima
limactl --version
limactl start ./docker.yaml
limactl shell docker
```

## `docker.yaml`編集した場合

lima VM作り直し。

```sh
limactl stop docker
limactl rm docker
limactl start ./docker.yaml
```
