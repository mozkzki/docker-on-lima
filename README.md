# Docker on lima

limaでDocker Engineを動かす。

- 参考
  - [Docker Desktop 無しで Docker を使う with lima on Mac - cangoxina](https://korosuke613.hatenablog.com/entry/2021/09/18/docker-on-lima#DOCKER_HOST-%E7%92%B0%E5%A2%83%E5%A4%89%E6%95%B0%E3%82%92%E8%A8%AD%E5%AE%9A%E3%81%99%E3%82%8B--sshconfig-%E3%82%92%E8%A8%AD%E5%AE%9A%E3%81%99%E3%82%8B20211022-%E8%BF%BD%E8%A8%98)
- 下記をベースにlima VMを作る
  - [examples/docker.yaml](https://github.com/lima-vm/lima/blob/master/examples/docker.yaml)

## 使い方

### Docker入り lime VM作成

```sh
brew install lima
limactl --version
limactl start ./docker.yaml
# hostの設定(DOCKER_HOSTの指定等)が済んでいれば、
# 下記でDocker client, serverのバージョンが表示されるはず
docker version
```

### `docker.yaml`編集した場合

lima VM作り直し。

```sh
limactl stop docker
limactl rm docker
limactl start ./docker.yaml
```

### lima VMへの操作関連

lima VMに入る時 (`docker`はlima VMのインスタンス名)

```sh
limactl shell docker
```

なお、`lima`コマンドは下記の通りdefaultインスタンスへのエイリアス

> lima is an alias for "limactl shell default".

なので、別のインスタンスへのコマンド実行は下記のように行う

```sh
limactl shell docker uname -a
# defaultインスタンスへの実行なら下記が可能
lima uname -a
```

## メモ

- `~/src`と`~/ghq`を書き込み可能でmountしている
