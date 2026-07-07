### **To monitor PostreSQL server you need :**

1. Install psycopg2 module.
2. Create PostreSQL user with privileges `pg_monitor`.
3. Configure Agent with User/Pass/Host, for connecting to Postgres server.

### **Install PostreSQL Python client**

**For Debian or Ubuntu**

```bash
apt-get install python3-psycopg2
```

**For RHEL / Rocky / AlmaLinux**

```bash
sudo dnf install python3-psycopg2
```

PostreSQL drivers can be installed via PIP as well.

```bash
pip3  install psycopg2
```

###**Create PostreSQL User**

Login to your PostreSQL server as superuser

```shell
sudo -u postgres psql
```

and execute following statement :

```sql
CREATE
USER monitor WITH PASSWORD 'your_secure_password';
GRANT pg_monitor TO monitor;
```

### **Configure Agent**

Copy or symlink `checks_available/check_postgres.py` to `checks_enabled`

```bash
cd ${AGENT_HOME/checks_enabled}
ln -s ../checks_available/check_postgres.py ./ 
```

Edit conf`conf/sql_cache.ini` and put right parameters at **PostreSQL** section:

```ini
[Postgres]
host : 127.0.0.1
user : monitor
pass : your_secure_password
```

### **Provides**

| Name                     | Description                                             | Type    | Unit      |
|--------------------------|---------------------------------------------------------|---------|-----------|
| psql_xact_commit         | Committed PostgreSQL transactions                       | rate    | TPS       |
| psql_xact_rollback       | Rolled back PostgreSQL transactions                     | rate    | TPS       |
| psql_rollback_ratio      | Ratio of rolled back transactions to total transactions | current | Ratio     |
| psql_numbackends         | Number of active client connections to the database     | current | None      |
| psql_blks_read           | Disk blocks read from storage                           | rate    | Blocks    |
| psql_blks_hit            | Shared buffer cache hits                                | rate    | Blocks    |
| psql_cache_hits_ratio    | PostgreSQL shared buffer cache hit ratio                | current | Ratio     |
| psql_tup_returned        | Rows returned by sequential and index scans             | rate    | Rows/s    |
| psql_tup_fetched         | Rows fetched by index scans                             | rate    | Rows/s    |
| psql_tup_inserted        | Rows inserted                                           | rate    | Rows/s    |
| psql_tup_updated         | Rows updated                                            | rate    | Rows/s    |
| psql_tup_deleted         | Rows deleted                                            | rate    | Rows/s    |
| psql_write_amplification | Total modified rows (inserted + updated + deleted)      | rate    | Rows/s    |
| psql_temp_files          | Temporary files created by queries                      | rate    | Files/s   |
| psql_temp_bytes          | Temporary data written to disk                          | rate    | Bytes     |
| psql_deadlocks           | Deadlocks detected                                      | rate    | Events/s  |
| psql_conflicts           | Query conflicts on standby servers                      | rate    | Events/s  |
| psql_checksum_failures   | Data checksum failures detected                         | rate    | Events/s  |
| psql_stats_reset         | Timestamp when PostgreSQL statistics were last reset    | current | Timestamp |
