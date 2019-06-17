# osquery command execution extension

> **Disclaimer:** This extension has been maid with educational purposes in mind. Do *NOT* run this in a production environment as it allows remote command execution on your device.

## Prerequisites

- Go toolchain (`1.12` or higher)
- `osquery`

## Installation and setup

Clone this repository and pull the dependencies before building the extension.
```bash
git clone https://github.com/hippwn/osquery-exec
cd osquery-exec
go get
go build -o exec.ext exec.go
```

> **Note:** On windows, the file extension is used to define how the file is understood by the system. You may want to change the filename to `exec.exe`.

## Usage

First, retrieve the socket path from osquery:
```bash
osqueryi --nodisable_extensions
osquery> select value from osquery_flags where name = "extensions_socket";
+-------------------+
| value             |
+-------------------+
| \\.\pipe\shell.em |
+-------------------+
```

Then, start the extension in another shell. You should see a log message popping in osquery's window.
```bash
.\exec.exe "\\.\pipe\shell.em"
```

You can now query the `exec` table:
```bash
osquery> .schema exec
CREATE TABLE exec(`cmd` TEXT, `stdout` TEXT, `stderr` TEXT, `code` TEXT);
osquery> SELECT * FROM exec WHERE cmd = "whoami";
+--------+--------------------+--------+------+
| cmd    | stdout             | stderr | code |
+--------+--------------------+--------+------+
| whoami | ad\johndoe         |        | 0    |
+--------+--------------------+--------+------+
```

For more information about osquery and its extensions, see the official documentation on [how to use extensions](https://osquery.readthedocs.io/en/stable/deployment/extensions/) and [how to build them](https://osquery.readthedocs.io/en/stable/development/osquery-sdk/).