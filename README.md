# kafka-shell

Bash-based kafka shell requiring only linux, core-utils, bash & original kafka scripts

## Prerequisites

### Usage in docker

1. Docker
2. Well, that's it

### Standalone usage

1. Bash
2. Local installation of kafka (kafka's shell scripts required)
3. GNU getopt

## Quickstart

If you searched through internet looking for some simple out-of-the-box kafka, I must congrat you! You've
found it! All you need to do is clone this repo:

```bash
git clone https://github.com/saksmt/kafka-shell.git
```

Navigate to `docker` directory:

```bash
cd kafka-shell/docker
```

Type:

```bash
docker-compose up -d
```

And finally get your kafka shell:

```bash
./connect-to-shell
```

### Bonus!

As a bonus you'll get [wonderful zookeeper client](https://github.com/peterservice-rnd/zman) on 8888 port

## Shell usage

There are 3 contexts:

1. `topics`
2. `publish`
3. `consume`

Topics context allows you to `list` existing topics, `create <TOPIC> [OPTIONS]` complex new ones, `create <TOPIC>`
simple (single partition, replication factor = 1) and `describe <TOPIC>` existing

Publish context, suprisingly, allows to publish messages to topics:

 - `echo "my message" | raw topic_name`
 - `echo "my message under key" | with-key key_name topic_name`

Consume context can consume messages, either `all-from <topic>` or only `latest-from <topic>`

## Shell outside of docker

### Installation

You'll need to have original kafka for example in `/opt/kafka`, then you need to tell the shell where kafka is:

```bash
KAFKA_BINDIR=/opt/kafka/bin
```

Place this file in `/usr/local/etc/kafka-shell/config`

If you want your shell to be placed somewhere outside of repository, don't forget to move `.kafka-shell-rcscript`
somewhere too, for example in `/usr/local/lib` and modify config (`/usr/local/etc/kafka-shell/config`)
like following:

```bash
LIB_FILE=/usr/local/lib/.kafka-shell-rcscript
```

#### Default installation

Default installation assumes you have kafka under `/usr/local/opt/kafka` and want to place shell under
`/usr/local/bin`

To quickly use such installation just run `./install`

### Usage

To connect to some kafka you'll need addresses of:
1. Zookeeper
2. Bootstrap kafka server
3. Broker (optional, defaults to bootstrap server)

```bash
ZK='zookeeperHost:2181' BOOTSTRAP_SERVER='kafkaBootstrapServer:9091' BROKER='brokerHost:9091'  kafka-shell connect
```

You can also store your servers under `/usr/local/etc/kafka-shell/servers` directory in following format:

```bash
# filename: localhost

ZK='localhost:2181'
BOOTSTRAP_SERVER='localhost:9091'
```

And then you can quickly connect to your saved kafka:

```bash
kafka-shell connect localhost
```

If you have lots of servers you may find these commands useful:

 - `kafka-shell list` - list all available servers
 - `kafka-shell show <server>` - show server's connect options

# License

All sources are licensed under MIT license.
