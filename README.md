# -0g-da-client-guide

![0G-Labs](https://github.com/user-attachments/assets/454988f9-1c59-4770-9a94-76870f084449)


### DA Client Setup Guide

This document provides a step-by-step guide for setting up and running your own DA client.

### Recommended Hardware Specifications

For optimal performance, it is recommended that your system meets the following specifications:

- **RAM:** 8 GB
- **CPU:** 2 cores
- **Bandwidth:** 100 Mbps for both download and upload

### Installation

#### Installing Dependencies

##### For Linux

1. Update the package manager:
   ```bash
   sudo apt-get update
   ```
2. Install cmake:
   ```bash
   sudo apt-get install cmake
   ```

##### For macOS

1. Install cmake using Homebrew:
   ```bash
   brew install cmake
   ```

#### Installing Go

##### For Linux

1. Download the Go installer:
   ```bash
   wget https://go.dev/dl/go1.22.0.linux-amd64.tar.gz
   ```
2. Extract the archive:
   ```bash
   rm -rf /usr/local/go && tar -C /usr/local -xzf go1.22.0.linux-amd64.tar.gz
   ```
3. Add Go to your PATH environment variable by adding the following line to your `~/.profile`:
   ```bash
   export PATH=$PATH:/usr/local/go/bin
   ```

##### For macOS

1. Download the Go installer from [go.dev](https://go.dev/dl/go1.22.0.darwin-amd64.pkg).
2. Open the downloaded package file and follow the instructions to install Go.

### Downloading the Source Code

Clone the repository containing the source code:
```bash
git clone -b v1.0.0-testnet https://github.com/0glabs/0g-da-client.git
```

### Configuration

Below is a table describing the main configuration parameters:

| Field                                 | Description                                                       |
|---------------------------------------|-------------------------------------------------------------------|
| `--chain.rpc`                         | JSON RPC node endpoint for the blockchain.                         |
| `--chain.private-key`                 | Hex-encoded private key for signing transactions.                  |
| `--chain.receipt-wait-rounds`         | Maximum number of retries for waiting for a transaction receipt.   |
| `--chain.receipt-wait-interval`       | Interval between retries when waiting for a transaction receipt.   |
| `--chain.gas-limit`                   | Gas limit for transactions.                                        |
| `--combined-server.use-memory-db`     | Whether to use memory for blob storage.                            |
| `--combined-server.storage.kv-db-path`| Path to the level DB.                                              |
| `--combined-server.storage.time-to-expire` | Time to expire blobs in the level DB.                           |
| `--combined-server.log.level-file`    | File log level.                                                    |
| `--combined-server.log.level-std`     | Standard output log level.                                         |
| `--combined-server.log.path`          | Log file path.                                                     |
| `--disperser-server.grpc-port`        | Server listening port.                                             |
| `--disperser-server.retriever-address`| GRPC host for the retriever.                                        |
| `--batcher.da-entrance-contract`      | Hex-encoded DA entrance contract address.                          |
| `--batcher.da-signers-contract`       | Hex-encoded DA signers contract address.                           |
| `--batcher.finalizer-interval`        | Interval for finalizing operations.                                |
| `--batcher.finalized-block-count`     | Number of blocks between the finalized and the latest block.       |
| `--batcher.confirmer-num`             | Number of confirmer threads.                                       |
| `--batcher.max-num-retries-for-sign`  | Maximum number of retries before a signing attempt fails.          |
| `--batcher.batch-size-limit`          | Maximum batch size in MiB.                                         |
| `--batcher.encoding-request-queue-size`| Size of the encoding request queue.                                |
| `--batcher.encoding-interval`         | Interval between blob encoding requests.                           |
| `--batcher.pull-interval`             | Interval for pulling from the encoded queue.                       |
| `--batcher.signing-interval`          | Interval between slice signing requests.                           |
| `--batcher.signed-pull-interval`      | Interval for pulling from the signed queue.                        |
| `--encoder-socket`                    | GRPC host of the encoder.                                          |
| `--encoding-timeout`                  | Total time to wait for a response from the encoder.                |
| `--signing-timeout`                   | Total time to wait for a response from the signer.                 |

### Running the DA Client

#### Building the Combined Server

To build the combined server, run the following command:
```bash
make build
```

#### Starting the Combined Server

To start the combined server, update the command below with the appropriate configuration values for your environment:

```bash
./bin/combined \
    --chain.rpc https://rpc-testnet.0g.ai \
    --chain.private-key 0x00 \
    --chain.receipt-wait-rounds 180 \
    --chain.receipt-wait-interval 1s \
    --chain.gas-limit 2000000 \
    --combined-server.use-memory-db \
    --combined-server.storage.kv-db-path ./../run/ \
    --combined-server.storage.time-to-expire 300 \
    --disperser-server.grpc-port 51001 \
    --batcher.da-entrance-contract 0xDFC8B84e3C98e8b550c7FEF00BCB2d8742d80a69 \
    --batcher.da-signers-contract 0x0000000000000000000000000000000000001000 \
    --batcher.finalizer-interval 20s \
    --batcher.confirmer-num 3 \
    --batcher.max-num-retries-for-sign 3 \
    --batcher.finalized-block-count 50 \
    --batcher.batch-size-limit 500 \
    --batcher.encoding-interval 3s \
    --batcher.encoding-request-queue-size 1 \
    --batcher.pull-interval 10s \
    --batcher.signing-interval 3s \
    --batcher.signed-pull-interval 20s \
    --encoder-socket 52.198.175.144:34000 \
    --encoding-timeout 600s \
    --signing-timeout 600s \
    --chain-read-timeout 12s \
    --chain-write-timeout 13s \
    --combined-server.log.level-file trace \
    --combined-server.log.level-std  trace \
    --combined-server.log.path ./../run/run.log
```

By following these steps, you will be able to successfully install and configure the DA client for your infrastructure.
