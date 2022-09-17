# Docker on Lima

[Lima](https://github.com/lima-vm/lima)でDocker Engineを動かす。M1 Macで動作検証。

- 参考
  - [Docker Desktop 無しで Docker を使う with lima on Mac - cangoxina](https://korosuke613.hatenablog.com/entry/2021/09/18/docker-on-lima#DOCKER_HOST-%E7%92%B0%E5%A2%83%E5%A4%89%E6%95%B0%E3%82%92%E8%A8%AD%E5%AE%9A%E3%81%99%E3%82%8B--sshconfig-%E3%82%92%E8%A8%AD%E5%AE%9A%E3%81%99%E3%82%8B20211022-%E8%BF%BD%E8%A8%98)
  - [Docker with Lima on Mac 2022版](https://zenn.dev/catminusminus/articles/2732083d18d004)
  - [macOSでもWSLみたいなLinux環境を手に入れる - Qiita](https://qiita.com/chibiegg/items/eede37345f7058ce604d)
- 下記をベースにDocker入りの lima VM を作る
  - [examples/docker.yaml](https://github.com/lima-vm/lima/blob/master/examples/docker.yaml)

## 使い方

### 初回に1度

Docker入り lime VMを作成する。

```sh
brew install lima
limactl --version
limactl start ./docker.yaml
```

Docker CLIを入れてlima上のdockerdへの接続を確認。

```sh
# .zshrcに下記追記
export DOCKER_HOST=$(limactl list docker --format 'unix://{{.Dir}}/sock/docker.sock')
```

```sh
brew install docker
# 下記はなくても動くようだ
limactl show-ssh --format=config docker >> ~/.ssh/config
# 下記でDocker client, serverのバージョンが表示されればOK
docker version
```

### 通常時

VMを立ち上げたら、VSCode Remote Containers等で開発が可能。

```sh
# VM状態確認
limactl list
# VM起動 ("docker"はlima VMのインスタンス名)
limactl start docker
# VM終了
limactl stop docker
```

### `docker.yaml`を修正した場合

lima VMを作り直す。

```sh
limactl stop docker
limactl rm docker
limactl start ./docker.yaml
```

### その他の操作

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

`.zshrc`等に下記設定すれば`lima`で特定インスタンスへ接続可能

```sh
export LIMA_INSTANCE=docker
```

## メモ

- `~/src`と`~/ghq`を書き込み可能でmountしている
