### Hi there 👋

I'm fritz. I am a technical data engineer / passionate technologist who loves tackling interesting problems with code :grinning:. I enjoy building useful data-oriented tools that unlock value, increase revenue and/or save cost. Some of the projects I am most proud of are: 

<details><summary><strong>dbNET (https://github.com/dbnet-io/dbnet)</strong></summary>
  <p align="center">
    <img src="https://user-images.githubusercontent.com/7671010/209962426-a849b819-480d-4863-9676-d13a195cc19d.png" height="50">
  </p>
dbNet is a web-based SQL IDE using Go as a backend, and your browser (or electron) as front-end. I built it because I was unsatisfied with the database clients out there. Alot of them are top-heavy, unituitive, slow or expensive. dbNet aims to be smart and useful especially for analysis and simply querying any SQL database.
  
<p/>
  
The goal is to make it a great SQL IDE which gives useful context as you hover table and column names for example. It should allow you to ingest files with ease, imagine drag-dropping a CSV file into a schema where dbNet auto-creates the table with proper column types. The other nifty part is that it can run from a shell/terminal on any machine and lets users access the UI from the browser (with `dbnet serve`). 
  
<img width="1241" alt="image" src="https://user-images.githubusercontent.com/7671010/209964766-5c694ee0-ea56-4d0e-8af6-317b070d5dc4.png">


dbNet is in active developement and will be open-sourced soon. Here are some of the databases it connects to:
* Clickhouse
* Google BigQuery
* Google BigTable
* MySQL
* Oracle
* Redshift
* PostgreSQL
* SQLite
* SQL Server
* Snowflake
* DuckDB (coming soon)
* ScyllaDB (coming soon)
* Firebolt (coming soon)
* Databricks (coming soon)
</details>

<details><summary><strong>dbREST (https://github.com/flarco/dbREST)</strong></summary>
  <p align="center">
    <img src="https://user-images.githubusercontent.com/7671010/209962006-fa72b231-fb12-4e78-8c72-eb7906874650.png" height="50">
  </p>

dbREST is basically an API backend that you can put in front of your database. Ever wanted to spin up an API service in front of your Snowflake, MySQL or even SQLite database? Well, dbREST allows that! Running `dbrest serve` will launch an API process which allow you to:

  
<details><summary>Select a table's data</summary>
  
```http
GET /snowflake_db/my_schema/docker_logs?fields=container_name,timestamp&limit=100
```
  
```json
[
  { "container_name": "vector", "timestamp": "2022-04-22T23:54:06.644268688Z" },
  { "container_name": "postgres", "timestamp": "2022-04-22T23:54:06.644315426Z" },
  { "container_name": "api", "timestamp": "2022-04-22T23:54:06.654821046Z" },
]
```
</details>
  
<details><summary>Insert into a table</summary>
  
```http
POST /snowflake_db/my_schema/docker_logs

[
  {"container_name":"vector","host":"vector","image":"timberio/vector:0.21.1-debian","message":"2022-04-22T23:54:06.644214Z  INFO vector::sources::docker_logs: Capturing logs from now on. now=2022-04-22T23:54:06.644150817+00:00","stream":"stderr","timestamp":"2022-04-22T23:54:06.644268688Z"}
]
```
</details>
  
<details><summary>Update a table</summary>
  
```http
PATCH /snowflake_db/my_schema/my_table?key=col1

[
  { "col1": "123", "timestamp": "2022-04-22T23:54:06.644268688Z" },
  { "col1": "124", "timestamp": "2022-04-22T23:54:06.644315426Z" },
  { "col1": "125", "timestamp": "2022-04-22T23:54:06.654821046Z" }
]
```
</details>
  
<details><summary>Upsert into a table</summary>
  
```http
POST /snowflake_db/my_schema/my_table?strategy=upsert&key=col1

[
  { "col1": "123", "timestamp": "2022-04-22T23:54:06.644268688Z" },
  { "col1": "124", "timestamp": "2022-04-22T23:54:06.644315426Z" },
  { "col1": "125", "timestamp": "2022-04-22T23:54:06.654821046Z" }
]
```
</details>
  
<details><summary>Submit a Custom SQL query</summary>
  
```http
POST /snowflake_db/.sql

select * from my_schema.docker_logs where timestamp is not null
```
  
```json
[
  { "container_name": "vector", "timestamp": "2022-04-22T23:54:06.644268688Z" },
  { "container_name": "postgres", "timestamp": "2022-04-22T23:54:06.644315426Z" },
  { "container_name": "api", "timestamp": "2022-04-22T23:54:06.654821046Z" },
]
```
</details>
  
<details><summary>List all columns in a table</summary>
  
```http
GET /snowflake_db/my_schema/docker_logs/.columns
```
  
```json
[
  {"column_id":1,"column_name":"timestamp", "column_type":"String", "database_name":"default", "schema_name":"my_schema", "table_name":"docker_logs", "table_type":"table"},
  {"column_id":2,"column_name":"container_name", "column_type":"String", "database_name":"default", "schema_name":"my_schema", "table_name":"docker_logs", "table_type":"table"},
  {"column_id":3,"column_name":"host", "column_type":"String", "database_name":"default", "schema_name":"my_schema", "table_name":"docker_logs", "table_type":"table"},{"column_id":4,"column_name":"image", "column_type":"String", "database_name":"default", "schema_name":"my_schema", "table_name":"docker_logs", "table_type":"table"},
]
```
</details>
  
<details><summary>List all tables in a schema</summary>
  
```http
GET /snowflake_db/my_schema/.tables
```
  
```json
[
  {"database_name":"default", "is_view":"table", "schema_name":"my_schema", "table_name":"docker_logs"},
  {"database_name":"default", "is_view":"table", "schema_name":"my_schema", "table_name":"example"},
  {"database_name":"default", "is_view":"view", "schema_name":"my_schema", "table_name":"place_vw"}
]
```
</details>
  
  
<details><summary>List all columns, in all tables in a schema</summary>
  
```http
GET /snowflake_db/my_schema/.columns
```
  
```json
[
  {"column_id":1,"column_name":"timestamp", "column_type":"String", "database_name":"default", "schema_name":"my_schema", "table_name":"docker_logs", "table_type":"table"},
  {"column_id":2,"column_name":"container_name", "column_type":"String", "database_name":"default", "schema_name":"my_schema", "table_name":"docker_logs", "table_type":"table"},
  {"column_id":3,"column_name":"host", "column_type":"String", "database_name":"default", "schema_name":"my_schema", "table_name":"docker_logs", "table_type":"table"},{"column_id":4,"column_name":"image", "column_type":"String", "database_name":"default", "schema_name":"my_schema", "table_name":"docker_logs", "table_type":"table"},
]
```
</details>

Of course there must be an authentication / authorization logic. It is based on tokens being issued with the `dbrest token` sub-command which are tied to roles defined in a YAML config file:

```yaml
roles:
  reader:
    snowflake_db:
      allow_read:
        - schema1.*
        - schema2.table1
      allow_sql: 'disable'

    my_pg:
      allow_read:
        - '*'
      allow_sql: 'disable' 

  writer:
    snowflake_db:
      allow_read:
        - schema1.*
        - schema2.table1
      allow_write:
        - schema2.table3
      allow_sql: 'disable'

    my_pg:
      allow_read:
        - '*'
      allow_write:
        - '*'
      allow_sql: 'any' 
```

We can now issue tokens with `dbrest token issue <token_name> --roles reader,writer`.
  
It is built in Go. And as you might have guessed, it also powers alot of `dbNet` :).

dbREST is in active developement. Here are some of the databases it connects to:
* Clickhouse
* Google BigQuery
* Google BigTable
* MySQL
* Oracle
* Redshift
* PostgreSQL
* SQLite
* SQL Server
* Snowflake
* DuckDB (coming soon)
* ScyllaDB (coming soon)
* Firebolt (coming soon)
* Databricks (coming soon)

</details>

<details><summary><strong>Sling (https://docs.slingdata.io/)</strong></summary>
  <p align="center">
    <img src="https://user-images.githubusercontent.com/7671010/209962606-ae2792a6-712e-4689-b5e9-cf4283749d8b.png" height="50">
  </p>
Sling is a passion project turned into a free CLI & SaaS Product which offers an easy solution to create and maintain high volume data pipelines using the Extract & Load (EL) approach. It focuses on data movement between:

* Database to Database
* File System to Database
* Database to File System

Ever wanted to quickly pipe in a CSV or JSON file into your database? Use sling to do so:

```bash
cat my_file.csv | sling run --tgt-conn MYDB --tgt-object my_schema.my_table
```
  
Or want to copy data between two databases? Do it with sling:
```bash
sling run --src-conn PG_DB --src-stream public.transactions --tgt-conn MYSQL_DB --tgt-object mysql.bank_transactions --mode full-refresh
```

We can also easily manage our local connections with the `sling conns` command:

```bash
$ sling conns set MY_PG url='postgresql://postgres:myPassword@pghost:5432/postgres'

$ sling conns list
+--------------------------+-----------------+-------------------+
| CONN NAME                | CONN TYPE       | SOURCE            |
+--------------------------+-----------------+-------------------+
| AWS_S3                   | FileSys - S3    | sling env yaml    |
| FINANCE_BQ               | DB - BigQuery   | sling env yaml    |
| DO_SPACES                | FileSys - S3    | sling env yaml    |
| LOCALHOST_DEV            | DB - PostgreSQL | dbt profiles yaml |
| MSSQL                    | DB - SQLServer  | sling env yaml    |
| MYSQL                    | DB - MySQL      | sling env yaml    |
| ORACLE_DB                | DB - Oracle     | env variable      |
| MY_PG                    | DB - PostgreSQL | sling env yaml    |
+--------------------------+-----------------+-------------------+

$ sling conns discover LOCALHOST_DEV
9:05AM INF Found 344 streams:
 - "public"."accounts"
 - "public"."bills"
 - "public"."connections"
 ...
```

Also check out [Sling Cloud](https://slingdata.io/) if you're interested in a GUI instead of the CLI!
  
![image](https://user-images.githubusercontent.com/7671010/209959200-925a815f-1cd6-4ca2-95e1-d8dc095fa8e8.png)


</details>
  
[![Linkedin](https://img.shields.io/badge/-LinkedIn-blue?style=flat-square&logo=Linkedin&logoColor=white&link=https://www.linkedin.com/in/fritzlarco/)](https://www.linkedin.com/in/fritzlarco/)
  
<!--
**flarco/flarco** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->
