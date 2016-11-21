# Learning static website hosting on AWS S3

[![Build Status](https://img.shields.io/travis/feedm3/learning-static-website-hosting-on-aws-s3.svg?style=flat-square)](https://travis-ci.org/feedm3/learning-static-website-hosting-on-aws-s3)

This repo is used to learn how to deploy a static page to AWS.

## How to

### Setup AWS

Our site will be hosted on S3 and served via CloudFront CDN.

#### Hosting on S3

1. Go to [S3](https://console.aws.amazon.com/s3/home) in your AWS account.
2. Create a new bucket on S3
3. By default your bucket is private and no one can access it so we have to configure it to make it publicly available:
    - Open the properties tab, click on _Static Website Hosting_ and check _Enable Website Hosting_. Also fill in the path
     to your index and error page (you can do this later after setting up you repo).
    - You also have to give `read` access to public otherwise no one could access the bucket although static hosting 
    is enabled. To set the access click on _Permissions_ and _Edit bucket policy_ and fill in the following policy but
    replace `YOUR_BUCKET_NAME` with - you wouldn't have guessed it - your bucket name
   
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

#### CloudFront as CDN

1. Go to [CloudFront](https://console.aws.amazon.com/cloudfront/home) in your AWS account
2. Create a new `Web` distribution
3. Configure your distribution:
    - Set your bucket as `Origin domain name`
    - For cheaper pricing only use US, Canada and Europ as `Price Class`
    - Click on `Create Distribution` to create your CloudFront CDN

### Setup Repo

We will setup our repo in a way that all files which get hosted are placed in the [www](www) folder. For the sake of 
simplicity we place a static `index.html`, `error.html` and `sample.html` file in this folder.

2 files are special: The index and error page. The index page will be send when the user uses your bucket URL and
doesn't provide any path or file. The error page will be send when a 4XX error code occures. Both pages need to be
set up on your bucket properties.

TODO: Small React App with yarn as package manager

### Setup Travis

You have to set your AWS Credentials to travis either by using the `.travis.yml` file
with [encrypted values](https://docs.travis-ci.com/user/environment-variables/#Defining-encrypted-variables-in-.travis.yml)
or on the travis website (repository settings). The credentials variables are `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`.

TODO: Explain .yml file
 

