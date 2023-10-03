# Demonstration of a dbt environment
This demo, was built to run inside of `Gitpod`

So, first things first create your own account

### First install the dependencies

With everything done, open a workspace and install all dependencies in the file `requirements.txt`

### Local AWS init

After that we have to simulate a AWS environment to use a kind of s3 bucket

So run this command to initiate the local AWS:
``` shell
localstack start
```

Now let's create a bucket

``` shell
awslocal s3 mb s3://lake-dbt-demo
```
> Localstack runs a local suite of common AWS services including S3. By default, it uses port 4566, and has test as both the access key ID and secret.

### Now lets get our data sources

Clone the Jaffle Shop repository [here](https://github.com/dbt-labs/jaffle_shop), in some sub-folder.

Configure your ``profiles.yml``

``` yml
jaffle_shop:
  outputs:
    dev:
      type: duckdb
      path: ../../db/jaffle_shop.duckdb
      filesystems:
        - fs: s3
          anon: false
          key: test
          secret: test
          client_kwargs:
            endpoint_url: "http://localhost:4566"
      external_root: "s3://lake-dbt-demo"
  target: dev
```

### Now let's get duckdb

Create a directory called `db`, go to it and run the command bellow

``` shell
wget https://github.com/duckdb/duckdb/releases/download/v0.9.0/duckdb_cli-linux-amd64.zip
```
### DB creation

Run the command bellow inside of ``jaffle_shop`` folder:

```shell
dbt debug --profiles-dir .
```
> This will generate de jaffle_shop's database.

### Ingest data

Still in the `jaffle_shop` folder, run the command bellow:

``` shell
dbt seed
```
> This will get all data files inside of seeds folder and put it inside of database.

### Check source tables

Go to the `db` folder and run the command bellow to enter duckdb:

``` shell
./duckdb
```

Now we are inside of duckdb, but we don't see any databases, try to run `SHOW DATABASES;` to see for your self.

To see the data that we put into duckdb, you'll need to attach the database, when you ran `dbt debug...` dbt generate a file with the name of the database right? So run the command bellow to attach the database:

``` shell
attach '<nameofdb>';

SHOW DATABASES; # see what is the name of the database

use <db>;
```
