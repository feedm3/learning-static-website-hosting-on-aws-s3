# Learning static website hosting on AWS S3

[![Build Status](https://img.shields.io/travis/feedm3/learning-static-website-hosting-on-aws-s3.svg?style=flat-square)](https://travis-ci.org/feedm3/learning-static-website-hosting-on-aws-s3)

This repo is used to learn how to deploy a static page to AWS.

## How to

### Setup AWS

1. Go to S3 in your AWS account.
2. Create a new bucket on S3
    - To enable static website hosting open the properties tab, click on _Static Website Hosting_ and check _Enable Website 	Hosting_. Fill in the index and error page.
    - You also have to set the `read` access to public, otherwise no one could access the bucket although static hosting is 	enabled. To set the correct access click on _Permissions_ and _Edit bucket policy_ and fill in the following police but
    replace YOUR_BUCKET_NAME with your bucket name
   
```
{
	"Version": "2012-10-17",
	"Id": "Policy1478014846282",
	"Statement": [
		{
			"Sid": "Stmt1478014844834",
			"Effect": "Allow",
			"Principal": "*",
			"Action": "s3:GetObject",
			"Resource": "arn:aws:s3:::YOUR_BUCKET_NAME/*"
		}
	]
}
``` 

### Setup Repo

### Setup Travis

You have to set your AWS Credentials to travis either by using the `.travis.yml` file
with encrypted values or on the travis website (repository settings).

`AWS_ACCESS_KEY_ID`

`AWS_SECRET_ACCESS_KEY` 

