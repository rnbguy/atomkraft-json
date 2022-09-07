# Atomkraft JSON

## Just show me some JSON

```sh
git clone https://github.com/rnbguy/atomkraft-json
cat atomkraft-json/reports/*/log.txt  | grep -v "INFO" | jq -s
```

I saved one output [here](atomkraft_tx.json).

## Install Atomkraft

Requires `pip` (from Python 3.10 and with latest `setuptools`), `git` and `java`.

Check pip's python version

```sh
pip -V # should print ... (python 3.10)
```

I modified a method to have access to the prepared transaction. So use my special branch.

```sh
pip install -U git+https://github.com/informalsystems/atomkraft@rano/tx-json
```

Verify

```sh
atomkraft version
```

Run following to setup Apalache

```sh
atomkraft model apalache get
```

## Setting up

Clone this project. It comes with a simple model for bank token transfer.

```sh
git clone https://github.com/rnbguy/atomkraft-json
cd atomkraft-json
```

Build `simd` from cosmos-sdk source. Requires `make`, `go`, `gcc`.

```sh
git submodule update --init --recursive
(cd cosmos-sdk; make build)
```

The [_reactor_](reactors/reactor.py) is responsible to execute each ITF step on a SUT.

I modified it to print signed valid transactions in pretty JSON format. You can use these JSON objects in `simd tx broadcast ...`.

Feel free to edit this file as you like.

## Executing

```sh
atomkraft test model --model models/transfer.tla --reactor reactors/reactor.py --keypath action.tag
```

- It will generate traces in `traces/` directory.
- Print `INFO` logs to the console.
- Also, the `INFO` logs are stored at `reports/<LAST_RUN>/log.txt`

For now, the JSON dump is very hacky. Please, update the code as you like.

For the current setup, you may run this to produce a valid JSON list of executed transactions. Requires `jq` (or `yq`)

```sh
cat reports/<LAST_RUN>/log.txt  | grep -v "INFO" | jq -s
```
