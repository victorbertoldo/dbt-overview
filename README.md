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

Clone the Jaffle Shop repository [here](https://github.com/dbt-labs/jaffle_shop)