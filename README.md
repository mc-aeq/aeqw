aeqw - aeqd Wallet
=========

aeqw is a daemon handling Æquator wallet functionality for a single user. It's a fork from [decred/dcrwallet] and acts as both an RPC client to aeqd and an RPC server for wallet clients and legacy RPC applications.

Public and private keys are derived using the hierarchical deterministic format described by [BIP0032](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki).
Unencrypted private keys are not supported and are never written to disk. aeqw uses the
`m/44'/<coin type>'/<account>'/<branch>/<address index>`
HD path for all derived addresses, as described by [BIP0044](https://github.com/bitcoin/bips/blob/master/bip-0044.mediawiki).

Due to the sensitive nature of public data in a BIP0032 wallet, aeqw provides the option of encrypting not just private keys, but public data as well.  This is intended to thwart privacy risks where a wallet file is compromised without exposing all current and future addresses (public keys) managed by the wallet. While access to this
information would not allow an attacker to spend or steal coins, it does mean they could track all transactions involving your addresses and therefore know your exact balance.  In a future release, public data encryption will extend to transactions as well.

aeqw is not an SPV client and requires connecting to a local or remote aeqd instance for asynchronous blockchain queries and notifications over websockets.  Full dcrd installation instructions can be found [here](https://github.com/mc-aeq/aeqd).  An alternative SPV mode that is compatible with aeqd is planned for a future release.

Wallet clients can use one of two RPC servers:

  1. A legacy JSON-RPC server inspired by the Bitcoin Core rpc server

     The JSON-RPC server exists to ease the migration of wallet applications from Core, but complete compatibility is not guaranteed.  Some portions of the API (and especially accounts) have to work differently due to other design decisions (mostly due to BIP0044).  However, if you find a compatibility issue and feel that it could be reasonably supported, please report an issue.  This server is enabled by default as long as a username and password are provided.

  2. A gRPC server

     The gRPC server uses a new API built for aeqw, but the API is not stabilized. This server is enabled by default and may be disabled with the config option `--nogrpc`.  If you don't mind applications breaking due to API changes, don't want to deal with issues of the legacy API, or need notifications for changes to the wallet, this is the RPC server to use. The gRPC server is documented [here](./rpc/documentation/README.md).

## Installation and updating

### Windows - MSIs Available

Install the latest MSIs as soon as they are released by our team.

### Windows/Linux/BSD/POSIX - Build from source

Building or updating from source requires the following build dependencies:

- **Go 1.9 or 1.10**

  Installation instructions can be found here: http://golang.org/doc/install.  It is recommended to add `$GOPATH/bin` to your `PATH` at this point.

- **Dep**

  Dep is used to manage project dependencies and provide reproducible builds.  It is recommended to use the latest Dep release, unless a bug prevents doing  so.  The latest releases (for both binary and source) can be found  [here](https://github.com/golang/dep/releases).

Unfortunately, the use of `dep` prevents a handy tool such as `go get` from automatically downloading, building, and installing the source in a single command.  Instead, the latest project and dependency sources must be first obtained manually with `git` and `dep`, and then `go` is used to build and install the project.

**Getting the source**:

For a first time installation, the project and dependency sources can be obtained manually with `git` and `dep` (create directories as needed):

```
git clone https://github.com/mc-aeq/aeqw $GOPATH/src/github.com/mc-aeq/aeqw
cd $GOPATH/src/github.com/mc-aeq/aewq
dep ensure
```

To update an existing source tree, pull the latest changes and install the matching dependencies:

```
cd $GOPATH/src/github.com/mc-aeq/aeqw
git pull
dep ensure
```

**Building/Installing**:

The `go` tool is used to build or install (to `GOPATH`) the project.  Some example build instructions are provided below (all must run from the `aeqw`
project directory).

To build and install `aeqw` and all helper commands (in the `cmd` directory) to `$GOPATH/bin/`, as well as installing all compiled packages to `$GOPATH/pkg/` (**use this if you are unsure which command to run**):

```
go install . ./cmd/...
```

To build a `aeqw` executable and install it to `$GOPATH/bin/`:

```
go install
```

To build a `dcrwallet` executable and place it in the current directory:

```
go build
```

## Docker

All tests and linters may be run in a docker container using the script `run_tests.sh`.  This script defaults to using the current supported version of go.  You can run it with the major version of go you would like to use as the only arguement to test a previous on a previous version of go (generally decred supports the current version of go and the previous one).

```
./run_tests.sh 1.9
```

To run the tests locally without docker:

```
./run_tests.sh local
```

## Getting Started

The following instructions detail how to get started with aeqw connecting to a localhost aeqd.  Commands should be run in `cmd.exe` or PowerShell on Windows, or any terminal emulator on *nix.

- Run the following command to start aeqd:

```
aeqd -u rpcuser -P rpcpass
```

- Run the following command to create a wallet:

```
aeqw -u rpcuser -P rpcpass --create
```

- Run the following command to start aeqw:

```
aeqw -u rpcuser -P rpcpass
```

If everything appears to be working, it is recommended at this point to copy the sample aeqd and aeqw configurations and update with your RPC username and password.

Linux/BSD/POSIX (Installed from source):
```bash
$ cp $GOPATH/src/github.com/mc-aeq/aeqd/sample-aeqd.conf ~/.aeqd/aeqd.conf
$ cp $GOPATH/src/github.com/mc-aeq/aeqw/sample-aeqw.conf ~/.aeqw/aeqw.conf
$ $EDITOR ~/.aeqd/aeqd.conf
$ $EDITOR ~/.aeqw/aeqw.conf
```

## Issue Tracker

We're using a private JIRA bugtracking system. Please foward us any issues and we'll assign them internally. If you want to join our bug tracking team, contact us and you'll get direct access to our JIRA instance.

## License

dcrwallet is licensed under the liberal ISC License.
aeqw is a fork from dcrwallet and also under the ISC License.

[//]: # (These are reference links used in the body of this note and get stripped out when the markdown processor does its job. There is no need to format nicely because it shouldn't be seen. Thanks SO - http://stackoverflow.com/questions/4823468/store-comments-in-markdown-syntax)

   [decred/dcrwallet]: <https://github.com/decred/dcrwallet> 
