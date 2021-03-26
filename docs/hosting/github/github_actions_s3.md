# GitHub Actions - Deploy to S3

A GitHub Actions workflow can be used to deploy resources to an S3 bucket.
This is particularly useful for a static site such as this one that is hosted
in S3 and deployed when content changes are committed to the site's GitHub
repository.

### Credentials

Configuring the crendentials used by AWS can be achieved a few different ways.
One way is to use the configure AWS credentials action offered [here](https://github.com/marketplace/actions/configure-aws-credentials-action-for-github-actions).

### Example Workflow

The following workflow checks out the content of the repository and then
deploys the contents of a "deploy/" folder to a bucket named my-bucket.

The credentials are set on step 2: "Configure AWS Credentials"

```
name: CI

on:
  push:
    branches: [main]

jobs:
  deploy:
    name: Deploy
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v2

      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v1
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: us-west-2

      - name: Deploy to S3
        run: aws s3 sync deploy/ s3://my-bucket
```
