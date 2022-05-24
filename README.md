# s3-uploader
A simple github action to send files to AWS S3.

## Usage
This is a super straightforward action that uses the latest aws cli tool to copy a file or a folder to an S3 bucket. The file can come from your code directly or it can be generated by an earlier part of your github actions flow. Check out the examples below to get started.

__Please note that each env var is required.__  
It is recommended to put your AWS credentials in as repository secrets, as well as your bucket name.  

All the parameters and additional arguments are passed to the [`aws s3 cp` command](https://docs.aws.amazon.com/cli/latest/reference/s3/cp.html).

```yaml
# inside .github/workflows/your-action.yml
name: Add a file to a bucket
on: push

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@master

    - name: Upload file to bucket
      uses: a-sync/s3-uploader@master
      with:
        args: --acl public-read
      env:
        AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
        AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        AWS_REGION: 'eu-central-1'
        S3_BUCKET: ${{ secrets.AWS_S3_BUCKET }}
        S3_KEY: ${{ secrets.S3_KEY }}
        FILE: ./lambda.zip
```

```yaml
# inside .github/workflows/your-action.yml
name: Add a folder to a bucket, excluding .log files
on: push

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@master

    - name: Upload folder to bucket
      uses: a-sync/s3-uploader@master
      with:
        args: --recursive --exclude "*.log"
      env:
        AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
        AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        AWS_REGION: 'eu-central-1'
        S3_BUCKET: ${{ secrets.AWS_S3_BUCKET }}
        S3_KEY: ${{ secrets.S3_KEY }}
        FILE: ./build
```
