# `bosh_multi_runner`

This tool will the specified list of commands, sending stderr and stdout to our own. It will restart any failing commands with roughly exponential backoff.

## Why does this exist at all?

`bosh` doesn't allow us to run more than one job as the same type (ie same name) on the same box at once.

This lets us workaround that.

## Usage

1. Create a `config.yml`:

    ```yaml
    commands:
    - command: httpd
    args:
    - arg1
    - arg2
    files:
      /etc/httpd/blah.conf: |
        some text to put in that file
      /tmp/foo: |
        some other text
    - command: httpd
    args:
    - arg1
    - arg2
    files:
      /etc/httpd/blah.conf: |
        some text to put in that file
      /tmp/foo: |
        some other text
    ```

2. Run us:

    ```bash
    bosh_multi_runner.go -config /path/to/config.yml
    ```

## Create release

```bash
VERSION=1.0.1

mkdir -p release/bosh_multi_runner-${VERSION}.linux-amd64
GOOS=linux GOARCH=amd64 go build -o release/bosh_multi_runner-${VERSION}.linux-amd64/bosh_multi_runner bosh_multi_runner.go
tar -C release -zcf release/bosh_multi_runner-${VERSION}.linux-amd64.tar.gz bosh_multi_runner-${VERSION}.linux-amd64
```
