# CloudFront Origin Access Identities

A CloudFront OAI (Origin Access Identity) is used to grant access from a
CloudFront distribution to an S3 bucket.

This is useful if you want to host a static website in an S3 bucket that users
will access via a CloudFront distribution.

## Purpose

An Origin Access Identity allows a CloudFront distribution to access objects in
an S3 bucket. This means that you can disable access to the S3 bucket directly
and only allow users to access the content via the CloudFront distribution.

#### AWS Documentation

This process is described in the AWS documentation [here](
https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-restricting-access-to-s3.html).

### Create an Origin Access Identity

There are two ways to create an Origin Access Identity:

* Manually, via the Origin Access Identity dashboard
* Automatically, via the CloudFront distribution dashboard

### Create an OAI Manually

To create an OAI manually, visit the Origin Access Identity dashboard
[here](https://console.aws.amazon.com/cloudfront/home?region=us-east-1#oai:).

* Click the "Create OAI" button.
* Enter a comment
* Click Create

#### Create an OAI Automatically

See below in "Associate OAI with Distribution" for a note on where to create
the OAI automatically. _(It's a radio button during the association step.)_

### Associate OAI with Distribution

Visit the CloudFront distribution dashboard
[here](https://console.aws.amazon.com/cloudfront/home?#distributions:).

* Select a distribution (check the box next to it)
* Click **Distribution Settings**
* Select the **Origins and Origin Groups** tab
* Check the box next to the origin and click **Edit**
* Set **Restrict Bucket Access** to **Yes**
* Automatic:
  * Set **Origin Access Identity** to **Create a New Identity**
  * Enter a comment to describe the identity
* Manual:
  * Set **Origin Access Identity** to **Use an Existing Identity**
  * Under **Your Identities** select the OAI to use
* Set **Grant Read Permissions on Bucket** to **No, I Will Update Permissions**
* Click **Yes, Edit**

### Update S3 Bucket Permissions

Next we edit bucket permissions to grant the new OAI access to the bucket and
to remove existing access permissions so that only the CloudFront distribution
has access.

First, get the ID of the OAI by visiting the dashboard and copying from the
**ID** column. This is used later on when adding permissions to the S3 policy.
Access the OAI dashboard [here](
https://console.aws.amazon.com/cloudfront/home?region=us-east-1#oai:).

Visit the S3 dashboard [here](https://s3.console.aws.amazon.com/s3/home)

* Click the name of the bucket to edit
* Select the **Permissions** tab
* Scroll down to **Bucket Policy** and click **Edit**
* Paste the following example policy, but change **OAIID** and **bucket_name**
  * **OAIID** should be replaced with the OADI ID copied from earlier
  * **bucket_name** is the name of the bucket
```
{
    "Version": "2012-10-17",
    "Id": "PolicyForCloudFrontPrivateAccess",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
              "AWS": "arn:aws:iam::cloudfront:user/CloudFront Origin Access Identity OAIID"
            },
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::bucket_name/*"
        }
    ]
}
```

This policy grants the OAI permission to call s3:GetObject on the bucket, which
is all it needs to expose the bucket content to the CloudFront distribution.
