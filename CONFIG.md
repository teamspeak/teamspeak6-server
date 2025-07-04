# CONFIG.md

### TeamSpeak 6 Server: Frequently Used Parameters & Environment Variables

This document outlines the most commonly used command-line parameters and their equivalent environment variables for configuring the TeamSpeak 6 server. You can configure the server using either of the following methods:
- Command-line flags
- Environment variables
- A `tsserver.yaml` configuration file

---

#### General & Core Server Configuration

| Parameter                     | Environment Variable           | Description                                                                                                                                           |
|-------------------------------|--------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|
| `--config-file <file>`        | `TSSERVER_CONFIG_FILE`         | Specifies the configuration file to load (e.g., `tsserver.yaml`).                                                                                     |
| `--license-path <path>`       | `TSSERVER_LICENSE_PATH`        | Directory path where the server will look for the `licensekey.dat` file.                                                                              |
| `--accept-license`            | `TSSERVER_LICENSE_ACCEPTED`    | Use the --accept-license flag or set the environment variable to 1 or accept to confirm that you have read and accepted the server license agreement. |
| `--default-voice-port <port>` | `TSSERVER_DEFAULT_PORT`        | Permanently sets the default UDP voice port for the first virtual server created (Default: 9987).                                                     |

---

#### File Transfer

| Parameter                         | Environment Variable           | Description                                            |
|-----------------------------------|--------------------------------|--------------------------------------------------------|
| `--filetransfer-port <port>`      | `TSSERVER_FILE_TRANSFER_PORT`  | The TCP port used for file transfers (Default: 30033). |

---

#### Database

| Parameter                         | Environment Variable              | Description                                                                           |
|----------------------------------|------------------------------------|---------------------------------------------------------------------------------------|
| `--db-plugin <plugin>`           | `TSSERVER_DATABASE_PLUGIN`         | Specifies the database plugin to use (e.g., `sqlite3`, `mariadb`).                    |
| `--db-host <host>`               | `TSSERVER_DATABASE_HOST`           | The hostname or IP address of your database server.                                   |
| `--db-port <port>`               | `TSSERVER_DATABASE_PORT`           | The port used to connect to your database server.                                     |
| `--db-name <name>`               | `TSSERVER_DATABASE_NAME`           | The name of the database to use.                                                      |
| `--db-username <user>`           | `TSSERVER_DATABASE_USERNAME`       | The username for database authentication.                                             |
| `--db-password <pass>`           | `TSSERVER_DATABASE_PASSWORD`       | The password for database authentication.                                             |
| `--db-connections <num>`         | `TSSERVER_DATABASE_CONNECTIONS`    | Number of connections to establish to the database (Default: 10).                     |
| `--db-sql-create-path <path>`    | `TSSERVER_DATABASE_SQL_CREATE_PATH`| The path for the database creation scripts (e.g., `create_mariadb`, `create_sqlite`). |

---

#### Server Query

| Parameter                       | Environment Variable             | Description                                                                 |
|---------------------------------|----------------------------------|-----------------------------------------------------------------------------|
| `--query-http-enable`           | `TSSERVER_QUERY_HTTP_ENABLED`    | Enables the HTTP query interface.                                           |
| `--query-http-port <port>`      | `TSSERVER_QUERY_HTTP_PORT`       | The port for the HTTP query interface (Default: 10080).                     |
| `--query-ssh-enable`            | `TSSERVER_QUERY_SSH_ENABLED`     | Enables the SSH query interface.                                            |
| `--query-ssh-port <port>`       | `TSSERVER_QUERY_SSH_PORT`        | The port for the SSH query interface (Default: 10022).                      |
| `--query-admin-password <pass>` | `TSSERVER_QUERY_ADMIN_PASSWORD`  | Sets a password for the serveradmin query account, overriding the database. |
| `--query-ip-allow-list <file>`  | `TSSERVER_QUERY_ALLOW_LIST`      | Path to a file listing IPs exempt from query flood protection.              |
| `--query-ip-block-list <file>`  | `TSSERVER_QUERY_DENY_LIST`       | Path to a file listing IPs that are blocked from the query interface.       |

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

### Further Parameters and Environment Variables

#### General Options

* **`-h, --help`**

  * Prints the help message and exits.
  * **Type:** `BOOLEAN FLAG`

* **`--accept-license`**

  * Confirms you have read and accepted the server license agreement.
  * **Type:** `BOOLEAN FLAG`
  * **Environment Variable:** `TSSERVER_LICENSE_ACCEPTED`

* **`--write-config-file`**

  * Writes the current configuration to the config file.
  * **Type:** `BOOLEAN FLAG`

* **`--config-file [tsserver.yaml]`**

  * Configuration file to load.
  * **Type:** `STRING`
  * **Environment Variable:** `TSSERVER_CONFIG_FILE`

* **`-v, --version`**

  * Displays program version information and exits.
  * **Type:** `BOOLEAN FLAG`

* **`--license-path [.]`**

  * Path where the server will look for the server license.
  * **Type:** `STRING`
  * **Environment Variable:** `TSSERVER_LICENSE_PATH`

---

#### Server Configuration

* **`--default-voice-port [9987]`**

  * Permanently sets the default voice port for the first virtual server created.
  * **Type:** `INTEGER in [1 - 65535]`
  * **Environment Variable:** `TSSERVER_DEFAULT_PORT`

* **`--voice-ip [[0.0.0.0,::]]`**

  * IP address for the server to bind the voice port to.
  * **Type:** `STRING`
  * **Environment Variable:** `TSSERVER_VOICE_IP`

* **`--machine-id`**

  * A string to distinguish this instance from other instances using the same database.
  * **Type:** `STRING`
  * **Environment Variable:** `TSSERVER_MACHINE_ID`

* **`--threads-voice-udp [5]`**

  * Number of threads to use for voice processing.
  * **Type:** `INTEGER in [1 - 16]`
  * **Environment Variable:** `TSSERVER_VOICE_UDP_THREADS`

---

#### Logging

* **`--log-path [logs]`**

  * Path to the directory where log files are stored.
  * **Type:** `STRING`
  * **Environment Variable:** `TSSERVER_LOG_PATH`

* **`--log-append`**

  * Write one ever-growing log file per virtual server.
  * **Type:** `BOOLEAN FLAG`
  * **Environment Variable:** `TSSERVER_APPEND_LOGS`

---

#### Virtual Server Management

* **`--no-default-virtual-server`**

  * If specified, no default server is created even if no server exists in the database.
  * **Type:** `BOOLEAN FLAG`
  * **Environment Variable:** `TSSERVER_NO_DEFAULT_SERVER`

---

#### File Transfer

* **`--filetransfer-port [30033]`**

  * Port on which to listen and advertise for file transfer connections.
  * **Type:** `INTEGER in [1 - 65535]`
  * **Environment Variable:** `TSSERVER_FILE_TRANSFER_PORT`
  * **Docker Note:** When running inside a Docker container, ensure that the **internal container port matches the external host port**. File transfers rely on clients connecting directly to this port, mismatches between the container's listening port and the published port will cause connectivity issues.

* `--filetransfer-ip [[0.0.0.0,::]]`

  * The address on which to listen for file transfer connections
  * **Type:** `STRING`
  * **Environment Variable:** `TSSERVER_FILE_TRANSFER_IP`

---

#### Database Configuration

* **`--clear-database`**

  * ⚠️ WARNING: Resets the database completely (deletes all servers, clients, etc.).
  * **Type:** `BOOLEAN FLAG`

* **`--no-permission-update`**

  * Do not apply permission changes during server updates.
  * **Type:** `BOOLEAN FLAG`
  * **Environment Variable:** `TSSERVER_SKIP_PERMISSION_UPDATE`

* **`--db-plugin [sqlite3]`**

  * Specifies the database plugin to use.
  * **Type:** `STRING`
  * **Environment Variable:** `TSSERVER_DATABASE_PLUGIN`

* **`--db-sql-path [sql]`**

  * Path to folder containing SQL queries used by the server.
  * **Type:** `STRING`
  * **Environment Variable:** `TSSERVER_DATABASE_SQL_PATH`

* **`--db-sql-create-path [create_sqlite]`**

  * Subdirectory in SQL path to use for DB creation scripts.
  * **Type:** `STRING`
  * **Environment Variable:** `TSSERVER_DATABASE_SQL_CREATE_PATH`

* **`--db-client-keep-days [30]`**

  * Number of days to keep clients in the database.
  * **Type:** `INTEGER`
  * **Environment Variable:** `TSSERVER_DATABASE_CLIENT_KEEP_DAYS`

* **`--db-skip-integrity-check`**

  * SQLite only: skip DB integrity check at startup.
  * **Type:** `BOOLEAN FLAG`
  * **Environment Variable:** `TSSERVER_DATABASE_SKIP_INTEGRITY_CHECK`

* **`--db-log-queries`**

  * Enable logging of SQL queries.
  * **Type:** `BOOLEAN FLAG`
  * **Environment Variable:** `TSSERVER_DATABASE_LOG_QUERIES`

#### Database Connection Options
* **`--db-host [127.0.0.1]`**

  * Database hostname or IP.
  * **Type:** `STRING`
  * **Environment Variable:** `TSSERVER_DATABASE_HOST`

* **`--db-port [5432]`**

  * Database port.
  * **Type:** `INTEGER in [1 - 65535]`
  * **Environment Variable:** `TSSERVER_DATABASE_PORT`

* **`--db-socket`**

  * Socket file to use for database connection.
  * **Type:** `STRING`
  * **Environment Variable:** `TSSERVER_DATABASE_SOCKET`

* **`--db-timeout [10]`**

  * Timeout in seconds when connecting to DB.
  * **Type:** `INTEGER in [1 - 432000]`
  * **Environment Variable:** `TSSERVER_DATABASE_TIMEOUT`

* **`--db-name [teamspeak]`**

  * Name of the database to use.
  * **Type:** `STRING`
  * **Environment Variable:** `TSSERVER_DATABASE_NAME`

* **`--db-username`**

  * The username to use for database authentication.
  * **Type:** `STRING`
  * **Environment Variable:** `TSSERVER_DATABASE_USERNAME`

* **`--db-password`**

  * The password to use for database authentication.
  * **Type:** `STRING`
  * **Environment Variable:** `TSSERVER_DATABASE_PASSWORD`

* **`--db-connections [10]`**

  * Number of connections to establish to the database.
  * **Type:** `INTEGER in [2 - 100]`
  * **Environment Variable:** `TSSERVER_DATABASE_CONNECTIONS`

---

#### Query Options

* **`--query-pool-size [2]`**

  * How many threads to use for query command processing.
  * **Type:** `INTEGER in [2 - 32]`
  * **Environment Variable:** `TSSERVER_QUERY_POOL_SIZE`

* **`--query-log-timing [3600]`**

  * Interval in seconds after which to log query statistics.
  * **Type:** `INTEGER in [10 - 31556952]`
  * **Environment Variable:** `TSSERVER_QUERY_LOG_TIMING`

* **`--query-ip-allow-list [query_ip_allowlist.txt]`**

  * File path listing IPs exempt from query flood protection.
  * **Type:** `STRING`
  * **Environment Variable:** `TSSERVER_QUERY_ALLOW_LIST`

* **`--query-ip-block-list [query_ip_denylist.txt]`**

  * File path listing IPs blocked from the query interface.
  * **Type:** `STRING`
  * **Environment Variable:** `TSSERVER_QUERY_DENY_LIST`

* **`--query-admin-password`**

  * Override the query password for the built-in serveradmin account.
  * **Type:** `STRING`
  * **Environment Variable:** `TSSERVER_QUERY_ADMIN_PASSWORD`

* **`--query-log-commands`**

  * Log every command received on the query interface.
  * **Type:** `BOOLEAN FLAG`
  * **Environment Variable:** `TSSERVER_QUERY_LOG_COMMANDS`

* **`--query-skip-brute-force-check`**

  * Skip brute force checking on query interface connections.
  * **Type:** `BOOLEAN FLAG`
  * **Environment Variable:** `TSSERVER_QUERY_SKIP_BRUTE_FORCE_CHECK`

* **`--query-buffer-mb [20]`**

  * How much memory (in MB) to allocate for query connection buffering.
  * **Type:** `INTEGER in [20 - 100]`
  * **Environment Variable:** `TSSERVER_QUERY_BUFFER_MB`

* **`--query-documentation-path [serverquerydocs]`**

  * Path to the query documentation files.
  * **Type:** `STRING`
  * **Environment Variable:** `TSSERVER_QUERY_DOCUMENTATION_PATH`

* **`--query-timeout [300]`**

  * Timeout in seconds before query connections expire.
  * **Type:** `INTEGER`
  * **Environment Variable:** `TSSERVER_QUERY_TIMEOUT`

---

#### Query SSH Options

* **`--query-ssh-enable`**

  * Enable the SSH query interface.
  * **Type:** `BOOLEAN FLAG`
  * **Environment Variable:** `TSSERVER_QUERY_SSH_ENABLED`

* **`--query-ssh-port [10022]`**

  * Port on which to listen for SSH query connections.
  * **Type:** `INTEGER in [1 - 65535]`
  * **Environment Variable:** `TSSERVER_QUERY_SSH_PORT`

* **`--query-ssh-ip [[0.0.0.0,::]]`**

  * Address to listen on for SSH query connections.
  * **Type:** `STRING`
  * **Environment Variable:** `TSSERVER_QUERY_SSH_IP`

* **`--query-ssh-rsa-key [ssh_host_rsa_key]`**

  * Path to the SSH RSA host key file.
  * **Type:** `STRING`
  * **Environment Variable:** `TSSERVER_QUERY_SSH_RSA_KEY`

---

#### Query HTTP Options

* **`--query-http-enable`**

  * Enable the HTTP query interface.
  * **Type:** `BOOLEAN FLAG`
  * **Environment Variable:** `TSSERVER_QUERY_HTTP_ENABLED`

* **`--query-http-port [10080]`**

  * Port on which to listen for Web Query connections.
  * **Type:** `INTEGER in [1 - 65535]`
  * **Environment Variable:** `TSSERVER_QUERY_HTTP_PORT`

* **`--query-http-ip [[0.0.0.0,::]]`**

  * Address to listen on for Web Query connections.
  * **Type:** `STRING`
  * **Environment Variable:** `TSSERVER_QUERY_HTTP_IP`

---

#### Query HTTPS Options

* **`--query-https-enable`**

  * Enable the HTTPS query interface.
  * **Type:** `BOOLEAN FLAG`
  * **Environment Variable:** `TSSERVER_QUERY_HTTPS_ENABLED`

* **`--query-https-port [10443]`**

  * Port on which to listen for secure Web Query connections.
  * **Type:** `INTEGER in [1 - 65535]`
  * **Environment Variable:** `TSSERVER_QUERY_HTTPS_PORT`

* **`--query-https-ip [[0.0.0.0,::]]`**

  * Address to listen on for secure Web Query connections.
  * **Type:** `STRING`
  * **Environment Variable:** `TSSERVER_QUERY_HTTPS_IP`

* **`--query-https-certificate`**

  * Path to certificate file for HTTPS Web Query.
  * **Type:** `STRING`
  * **Environment Variable:** `TSSERVER_QUERY_HTTPS_CERT`

* **`--query-https-private-key`**

  * Path to private key file used with the certificate.
  * **Type:** `STRING`
  * **Environment Variable:** `TSSERVER_QUERY_HTTPS_PRIVATE_KEY`

---

#### Additional Options

* **`--http-proxy`**

  * Proxy server used for external connections.
  * **Type:** `STRING`
  * **Environment Variable:** `TSSERVER_HTTP_PROXY`

* **`--crashdump-path [crashdumps]`**

  * Path where crash dumps should be written.
  * **Type:** `STRING`
  * **Environment Variable:** `TSSERVER_CRASHDUMP_PATH`

* **`--daemon`**

  * Run the server in the background (daemon mode).
  * **Type:** `BOOLEAN FLAG`
  * **Environment Variable:** `TSSERVER_DAEMON`

* **`--pid-file`**

  * File to write the process ID to.
  * **Type:** `STRING`
  * **Environment Variable:** `TSSERVER_PID_FILE`

* **`--hints-enabled`**

  * Enable permission hints (can impact performance on large servers).
  * **Type:** `BOOLEAN FLAG`
  * **Environment Variable:** `TSSERVER_HINTS_ENABLED`

* **`--maxmind-db-path`**

  * Path to the MaxMind GeoIP database.
  * **Type:** `STRING`
  * **Environment Variable:** `TSSERVER_MAXMIND_DB_PATH`

* **`--administrative-domain`**

  * Administrative domain for internal or regulatory identification.
  * **Type:** `STRING`
  * **Environment Variable:** `TSSERVER_ADMINISTRATIVE_DOMAIN`
