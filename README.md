# hum

環境別の設定などより柔軟に docker-compose を扱うための docker-compose のラッパースクリプト  
プロジェクトルートに`compose.d`ディレクトリを自動で作成し  
`compose.d`ディレクトリ内の`base.yml`, `<OPERATION_MODE>.yml`(.env の環境変数 OPERATION_MODE に指定),`gitignored.yml`の 3 種類の yaml ファイルによって docker-compose を制御します。
3 種類の yaml の特性については[compose.d 内の 3 種類の docker-compose.yml について](https://github.com/asweed888/hum#composed-%E5%86%85%E3%81%AE-3-%E7%A8%AE%E9%A1%9E%E3%81%AE-docker-composeyml-%E3%81%AB%E3%81%A4%E3%81%84%E3%81%A6)の項を確認してください。

## 前提

- docker インストール済み
- docker-compose インストール済み

## インストール

```
git clone https://github.com/asweed888/hum.git ~/.hum
sudo ln -s $HOME/.hum/hum /usr/local/bin/hum
```

## 使用法

#### 初期化

初期化は非常に簡単です。
自分のプロジェクトルートにて
`hum`を実行するだけです。
初期構成を構築するか問われるので`y`を入力します。
するとその後、hum を実行するための初期構成が構築され
docker-compose の help が表示されます。

```
$ cd /path/to/your_project
$ hum
Initial configuration not found, do you want to build hum initial configuration? (y/N): y
.env is not found. Create .env.
compose.d is not found. Prepare initial configuration.
Define and run multi-container applications with Docker.

Usage:
  docker-compose [-f <arg>...] [--profile <name>...] [options] [--] [COMMAND] [ARGS...]
  docker-compose -h|--help

Options:
  -f, --file FILE             Specify an alternate compose file
                              (default: docker-compose.yml)
  -p, --project-name NAME     Specify an alternate project name
                              (default: directory name)
  --profile NAME              Specify a profile to enable
  -c, --context NAME          Specify a context name
  --verbose                   Show more output
  --log-level LEVEL           Set log level (DEBUG, INFO, WARNING, ERROR, CRITICAL)
  --ansi (never|always|auto)  Control when to print ANSI control characters
  --no-ansi                   Do not print ANSI control characters (DEPRECATED)
  -v, --version               Print version and exit
  -H, --host HOST             Daemon socket to connect to

  --tls                       Use TLS; implied by --tlsverify
  --tlscacert CA_PATH         Trust certs signed only by this CA
  --tlscert CLIENT_CERT_PATH  Path to TLS certificate file
  --tlskey TLS_KEY_PATH       Path to TLS key file
  --tlsverify                 Use TLS and verify the remote
  --skip-hostname-check       Don't check the daemon's hostname against the
                              name specified in the client certificate
  --project-directory PATH    Specify an alternate working directory
                              (default: the path of the Compose file)
  --compatibility             If set, Compose will attempt to convert keys
                              in v3 files to their non-Swarm equivalent (DEPRECATED)
  --env-file PATH             Specify an alternate environment file

Commands:
  build              Build or rebuild services
  config             Validate and view the Compose file
  create             Create services
  down               Stop and remove resources
  events             Receive real time events from containers
  exec               Execute a command in a running container
  help               Get help on a command
  images             List images
  kill               Kill containers
  logs               View output from containers
  pause              Pause services
  port               Print the public port for a port binding
  ps                 List containers
  pull               Pull service images
  push               Push service images
  restart            Restart services
  rm                 Remove stopped containers
  run                Run a one-off command
  scale              Set number of containers for a service
  start              Start services
  stop               Stop services
  top                Display the running processes
  unpause            Unpause services
  up                 Create and start containers
  version            Show version information and quit

Docker Compose is now in the Docker CLI, try `docker compose`
```

## compose.d 内の 3 種類の docker-compose.yml について

### base.yml

全ての環境に共通した。最も汎用的な設定を記載します。

### <OPERATION_MODE>.yml

`hum`を実行すると.env の`OPERATION_MODE`の値に応じた yaml ファイルが`compose.d/<OPERATION_MODE>.yml`として自動的に作成されます。  
このファイルは development や production など環境毎に必要な値を設定します。

### gitignored.yml

このファイルは最も固有な設定をしたい場合に利用します。  
あるいは git の歴史に載せたくない設定値を指定する場合にも利用できます。  
また、`ports`などの設定値も docker-compose の配列型設定値の上書きできないという特性上このファイルに記載することが推奨される場合があります。  
このファイルはその名の通り gitignore されており git で commit される事はありません。  
`gitignored.yml`以外にも`gitignored*.yml`も gitignore される為作業リポジトリでブランチを切り替えて作業する際も  
`gitignored.BAK.yml`などにリネームすれば、以前設定した`gitignored.yml`の内容を退避させることもできます。
