# CONFIG.md

### TeamSpeak 6 Server: Frequently Used Parameters & Environment Variables

This document provides a detailed list of frequently used command-line parameters and their corresponding environment variables for configuring the TeamSpeak 6 server. You can configure the server using:

- Command-line flags
- Environment variables
- A `tsserver.yaml` configuration file

---

#### General & Core Server Configuration

| Parameter                         | Environment Variable           | Description                                                                 |
|----------------------------------|--------------------------------|-----------------------------------------------------------------------------|
| `--config-file <file>`           | `TSSERVER_CONFIG_FILE`         | Specifies the configuration file to load (e.g., `tsserver.yaml`).          |
| `--license-path <path>`          | `TSSERVER_LICENSE_PATH`        | Directory path where the server will look for the `licensekey.dat` file.   |
| `--accept-license [0\|1]`        | `TSSERVER_LICENSE_ACCEPTED`    | Set to `1` or `accept` to agree to the server license.                     |
| `--default-voice-port <port>`    | `TSSERVER_DEFAULT_PORT`        | Sets the default UDP voice port for the first virtual server (Default: 9987). |

---

#### File Transfer

| Parameter                         | Environment Variable           | Description                                                                 |
|----------------------------------|--------------------------------|-----------------------------------------------------------------------------|
| `--filetransfer-port <port>`     | `TSSERVER_FILE_TRANSFER_PORT`  | The TCP port used for file transfers (Default: 30033).                      |         |

---

#### Database

| Parameter                         | Environment Variable           | Description                                                                 |
|----------------------------------|--------------------------------|-----------------------------------------------------------------------------|
| `--db-plugin <plugin>`           | `TSSERVER_DATABASE_PLUGIN`     | Specifies the database plugin to use (e.g., `sqlite3`, `mariadb`).         |
| `--db-host <host>`               | `TSSERVER_DATABASE_HOST`       | The hostname or IP address of your database server.                        |
| `--db-port <port>`               | `TSSERVER_DATABASE_PORT`       | The port used to connect to your database server.                          |
| `--db-name <name>`               | `TSSERVER_DATABASE_NAME`       | The name of the database to use.                                           |
| `--db-username <user>`           | `TSSERVER_DATABASE_USERNAME`   | The username for database authentication.                                  |
| `--db-password <pass>`           | `TSSERVER_DATABASE_PASSWORD`   | The password for database authentication.                                  |
| `--db-connections <num>`         | `TSSERVER_DATABASE_CONNECTIONS`| Number of connections to establish to the database (Default: 10).          |

---

#### Server Query

| Parameter                         | Environment Variable             | Description                                                             |
|----------------------------------|----------------------------------|-------------------------------------------------------------------------|
| `--query-http-enable [0\|1]`     | `TSSERVER_QUERY_HTTP_ENABLED`    | Enables the HTTP query interface.                                      |
| `--query-http-port <port>`       | `TSSERVER_QUERY_HTTP_PORT`       | The port for the HTTP query interface (Default: 10080).                |
| `--query-ssh-enable [0\|1]`      | `TSSERVER_QUERY_SSH_ENABLED`     | Enables the SSH query interface.                                       |
| `--query-ssh-port <port>`        | `TSSERVER_QUERY_SSH_PORT`        | The port for the SSH query interface (Default: 10022).                 |
| `--query-admin-password <pass>`  | `TSSERVER_QUERY_ADMIN_PASSWORD`  | Sets a password for the serveradmin query account, overriding the database. |
| `--query-ip-allow-list <file>`   | `TSSERVER_QUERY_ALLOW_LIST`      | Path to a file listing IPs exempt from query flood protection.         |
| `--query-ip-block-list <file>`   | `TSSERVER_QUERY_DENY_LIST`       | Path to a file listing IPs that are blocked from the query interface.  |

### Example `tsserver.yaml` Configuration

```yaml
server:
  license-path: .
  default-voice-port: 9987
  voice-ip:
    - 0.0.0.0
    - "::"
  machine-id: ""
  threads-voice-udp: 5
  log-path: logs
  log-append: 0
  no-default-virtual-server: 0
  filetransfer-port: 30033
  filetransfer-ip:
    - 0.0.0.0
    - "::"
  clear-database: 0
  no-permission-update: 0
  http-proxy: ""
  accept-license: accept
  crashdump-path: crashdumps

  database:
    plugin: sqlite3
    sql-path: /opt/tsserver/sql/
    sql-create-path: /opt/tsserver/sql/create_sqlite/
    client-keep-days: 30
    config:
      skip-integrity-check: 0
      host: 127.0.0.1
      port: 5432
      socket: ""
      timeout: 10
      name: teamspeak
      username: ""
      password: ""
      connections: 10
      log-queries: 0

  query:
    pool-size: 2
    log-timing: 3600
    ip-allow-list: query_ip_allowlist.txt
    ip-block-list: query_ip_denylist.txt
    admin-password: ""
    log-commands: 0
    skip-brute-force-check: 0
    buffer-mb: 20
    documentation-path: serverquerydocs
    timeout: 300

    ssh:
      enable: 0
      port: 10022
      ip:
        - 0.0.0.0
        - "::"
      rsa-key: ssh_host_rsa_key

    http:
      enable: 0
      port: 10080
      ip:
        - 0.0.0.0
        - "::"

    https:
      enable: 0
      port: 10443
      ip:
        - 0.0.0.0
        - "::"
      certificate: ""
      private-key: ""
```

### Further Options

#### General Options

- **`-h, --help`**
    - Prints the help message and exits.

- **`--write-config-file`**
    - Writes the current configuration to the config file.

- **`--config-file [tsserver.yaml]`**
    - Configuration file to load.
    - **Environment Variable:** `TSSERVER_CONFIG_FILE`

- **`-v, --version`**
    - Displays program version information and exits.

- **`--license-path [.]`**
    - Path where the server will look for the server license.
    - **Environment Variable:** `TSSERVER_LICENSE_PATH`

#### Server Configuration

- **`--default-voice-port :INT in [1 - 65535] [9987]`**
    - Sets the default voice port for the first virtual server.
    - **Environment Variable:** `TSSERVER_DEFAULT_PORT`

- **`--voice-ip [[0.0.0.0,::]]`**
    - IP address for the server to bind the voice port to.
    - **Environment Variable:** `TSSERVER_VOICE_IP`

- **`--machine-id`**
    - A string to distinguish this instance from other instances using the same database.
    - **Environment Variable:** `TSSERVER_MACHINE_ID`

- **`--threads-voice-udp :UINT in [1 - 16] [5]`**
    - Number of threads to use for voice processing.
    - **Environment Variable:** `TSSERVER_VOICE_UDP_THREADS`

#### Logging

- **`--log-path [logs]`**
    - Path to the directory where log files are stored.
    - **Environment Variable:** `TSSERVER_LOG_PATH`

- **`--log-append [0]`**
    - Write one ever-growing log file per virtual server.
    - **Environment Variable:** `TSSERVER_APPEND_LOGS`

#### Virtual Server Management

- **`--no-default-virtual-server [0]`**
    - If specified, no default server is created even if no server exists in the database.
    - **Environment Variable:** `TSSERVER_NO_DEFAULT_SERVER`

#### File Transfer

- **`--filetransfer-port :INT in [1 - 65535] [30033]`**
    - Port on which to listen for file transfer connections.
    - **Environment Variable:** `TSSERVER_FILE_TRANSFER_PORT`

- **`--filetransfer-ip [[0.0.0.0,::]]`**
    - Address on which to listen for file transfer connections.
    - **Environment Variable:** `TSSERVER_FILE_TRANSFER_IP`

#### Database Configuration

- **`--clear-database [0]`**
    - **WARNING:** If present, the database is reset to default, and all servers, clients, etc., are lost!

- **`--no-permission-update [0]`**
    - Do not apply permission changes with a server update.
    - **Environment Variable:** `TSSERVER_SKIP_PERMISSION_UPDATE`

- **`--db-plugin [sqlite3]`**
    - Specifies which database plugin to use.
    - **Environment Variable:** `TSSERVER_DATABASE_PLUGIN`

- **`--db-sql-path [sql]`**
    - Path to the SQL folder containing the SQL queries executed by the server.
    - **Environment Variable:** `TSSERVER_DATABASE_SQL_PATH`

- **`--db-sql-create-path [create_sqlite]`**
    - Path relative to the `--sql-path` where to load the database creation scripts from.
    - **Environment Variable:** `TSSERVER_DATABASE_SQL_CREATE_PATH`

- **`--db-client-keep-days [30]`**
    - Number of days to keep clients in the database.
    - **Environment Variable:** `TSSERVER_DATABASE_CLIENT_KEEP_DAYS`

#### Advanced Database Configuration

- **`--db-skip-integrity-check [0]`**
    - SQLite only: Skip the database integrity check when opening the database.
    - **Environment Variable:** `TSSERVER_DATABASE_SKIP_INTEGRITY_CHECK`

- **`--db-host [127.0.0.1]`**
    - Hostname/IP address to use when connecting to the database server.
    - **Environment Variable:** `TSSERVER_DATABASE_HOST`

- **`--db-port :INT in [1 - 65535] [5432]`**
    - Port to use when connecting to the database server.
    - **Environment Variable:** `TSSERVER_DATABASE_PORT`

- **`--db-socket`**
    - Socket to use for the database connection.
    - **Environment Variable:** `TSSERVER_DATABASE_SOCKET`

- **`--db-timeout :INT in [1 - 432000] [10]`**
    - Timeout in seconds for the database connection.
    - **Environment Variable:** `TSSERVER_DATABASE_TIMEOUT`

- **`--db-name [teamspeak]`**
    - The database name to use.
    - **Environment Variable:** `TSSERVER_DATABASE_NAME`

- **`--db-username`**
    - The username to use for database authentication.
    - **Environment Variable:** `TSSERVER_DATABASE_USERNAME`

- **`--db-password`**
    - The password to use for database authentication.
    - **Environment Variable:** `TSSERVER_DATABASE_PASSWORD`

- **`--db-connections :INT in [2 - 100] [10]`**
    - Number of connections to establish to the database.
    - **Environment Variable:** `TSSERVER_DATABASE_CONNECTIONS`

- **`--db-log-queries [0]`**
    - Whether to log database queries.
    - **Environment Variable:** `TSSERVER_DATABASE_LOG_QUERIES`

#### Query Options

- **`--query-pool-size :UINT in [2 - 32] [2]`**
    - Number of threads to use for query command processing.
    - **Environment Variable:** `TSSERVER_QUERY_POOL_SIZE`

- **`--query-log-timing :INT in [10 - 31556952] [3600]`**
    - Interval in seconds after which to log query statistics.
    - **Environment Variable:** `TSSERVER_QUERY_LOG_TIMING`

- **`--query-ip-allow-list [query_ip_allowlist.txt]`**
    - File path to a file which lists the addresses that are exempt from query flood protection.
    - **Environment Variable:** `TSSERVER_QUERY_ALLOW_LIST`

- **`--query-ip-block-list [query_ip_denylist.txt]`**
    - File path to a file which lists the addresses that are blocked from the query interface.
    - **Environment Variable:** `TSSERVER_QUERY_DENY_LIST`

- **`--query-admin-password`**
    - Override the query password for the built-in serveradmin account.
    - **Environment Variable:** `TSSERVER_QUERY_ADMIN_PASSWORD`

- **`--query-log-commands [0]`**
    - If specified, every command received on the query interface will be logged.
    - **Environment Variable:** `TSSERVER_QUERY_LOG_COMMANDS`

- **`--query-skip-brute-force-check [0]`**
    - Skips brute force checking when clients connect to the query interface.
    - **Environment Variable:** `TSSERVER_QUERY_SKIP_BRUTE_FORCE_CHECK`

- **`--query-buffer-mb :INT in [20 - 100] [20]`**
    - Memory allocation for query connection buffering.
    - **Environment Variable:** `TSSERVER_QUERY_BUFFER_MB`

- **`--query-documentation-path [serverquerydocs]`**
    - Path to the query documentation files.
    - **Environment Variable:** `TSSERVER_QUERY_DOCUMENTATION_PATH`

- **`--query-timeout [300]`**
    - Specifies the timeout for query connections.
    - **Environment Variable:** `TSSERVER_QUERY_TIMEOUT`

#### Query SSH Options

- **`--query-ssh-enable [0]`**
    - Enable the SSH query interface.
    - **Environment Variable:** `TSSERVER_QUERY_SSH_ENABLED`

- **`--query-ssh-port :INT in [1 - 65535] [10022]`**
    - Port on which to listen for SSH query connections.
    - **Environment Variable:** `TSSERVER_QUERY_SSH_PORT`

- **`--query-ssh-ip [[0.0.0.0,::]]`**
    - Address on which to listen for SSH query connections.
    - **Environment Variable:** `TSSERVER_QUERY_SSH_IP`

- **`--query-ssh-rsa-key [ssh_host_rsa_key]`**
    - The SSH RSA host key to use.
    - **Environment Variable:** `TSSERVER_QUERY_SSH_RSA_KEY`

#### Query HTTP Options

- **`--query-http-enable [0]`**
    - Enable the HTTP query interface.
    - **Environment Variable:** `TSSERVER_QUERY_HTTP_ENABLED`

- **`--query-http-port :INT in [1 - 65535] [10080]`**
    - Port on which to listen for Web Query connections.
    - **Environment Variable:** `TSSERVER_QUERY_HTTP_PORT`

- **`--query-http-ip [[0.0.0.0,::]]`**
    - Address on which to listen for Web Query connections.
    - **Environment Variable:** `TSSERVER_QUERY_HTTP_IP`

#### Query HTTPS Options

- **`--query-https-enable [0]`**
    - Enable the HTTPS query interface.
    - **Environment Variable:** `TSSERVER_QUERY_HTTPS_ENABLED`

- **`--query-https-port :INT in [1 - 65535] [10443]`**
    - Port on which to listen for secure Web Query connections.
    - **Environment Variable:** `TSSERVER_QUERY_HTTPS_PORT`

- **`--query-https-ip [[0.0.0.0,::]]`**
    - Address on which to listen for secure Web Query connections.
    - **Environment Variable:** `TSSERVER_QUERY_HTTPS_IP`

- **`--query-https-certificate`**
    - File containing the certificate for secure Web Query connections.
    - **Environment Variable:** `TSSERVER_QUERY_HTTPS_CERT`

- **`--query-https-private-key`**
    - File containing the private key for the certificate.
    - **Environment Variable:** `TSSERVER_QUERY_HTTPS_PRIVATE_KEY`

#### Additional Options

- **`--http-proxy`**
    - Proxy server for connecting to external services.
    - **Environment Variable:** `TSSERVER_HTTP_PROXY`

- **`--accept-license [0]`**
    - Confirms you have read and accepted the server license agreement.
    - **Environment Variable:** `TSSERVER_LICENSE_ACCEPTED`

- **`--crashdump-path [crashdumps]`**
    - Path to store crash dumps.
    - **Environment Variable:** `TSSERVER_CRASHDUMP_PATH`

- **`--daemon [0]`**
    - Run the server in the background.
    - **Environment Variable:** `TSSERVER_DAEMON`

- **`--pid-file`**
    - File to store the process ID of the server.
    - **Environment Variable:** `TSSERVER_PID_FILE`

- **`--hints-enabled [0]`**
    - Enable permission hints (may affect performance on larger servers).
    - **Environment Variable:** `TSSERVER_HINTS_ENABLED`

- **`--maxmind-db-path`**
    - Path to the MaxMind IP database.
    - **Environment Variable:** `TSSERVER_MAXMIND_DB_PATH`

- **`--administrative-domain`**
    - Administrative domain.
    - **Environment Variable:** `TSSERVER_ADMINISTRATIVE_DOMAIN`
