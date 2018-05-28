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
