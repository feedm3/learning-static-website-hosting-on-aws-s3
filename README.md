# Learning static website hosting on AWS S3

[![Build Status](https://img.shields.io/travis/feedm3/learning-static-website-hosting-on-aws-s3.svg?style=flat-square)](https://travis-ci.org/feedm3/learning-static-website-hosting-on-aws-s3)

This repo is used to learn how to deploy a static page to AWS.

## How to

### Setup AWS

Our site will be hosted on S3 and served via CloudFront CDN. You will need external access to your AWS resources so
it's best to setup a __new IAM user with `AmazonS3FullAccess` and `CloudFrontFullAccess` policy only__.

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

If you put CloudFront in front of S3 you can save a lot of costs and increase your sites response time. 

> Take in mind that all changes you do on CloudFront take some minutes till they get applied.

1. Go to [CloudFront](https://console.aws.amazon.com/cloudfront/home) in your AWS account
2. Create a new `Web` distribution
3. Configure your distribution:
    - Set your bucket as `Origin domain name`. This will link CloudFront and S3
    - Set the `index.html` file as `Default Root Object`. This will allow the user to access your domain without any path
    and still get the index file.
    - For cheaper pricing only use US, Canada and Europe as `Price Class`
    - Click on `Create Distribution` to create your CloudFront CDN
4. You are now on your distributions dashboard. Select your new distribution
    - Go to `Error Pages` to set up some error pages for specific HTTP status codes
        - Click `Create Custom Error Response` then select `404` as `HTTP Error Code` and click `Yes` on
        `Customize Error Response`. Set `/error.html` as `Response Page Path and `404` as `HTTP Response Code`
5. In you distributions dashboard you can also see your distributions ID. You need this ID in your travis-ci 
environment variables.

### Setup Repo

We will setup our repo in a way that all files which get hosted are placed in the [www](www) folder. For the sake of 
simplicity we place a static `index.html`, `error.html` and `sample.html` file in this folder.

2 files are special: The index and error page. The index page will be send when the user uses your bucket or CloudFront
URL and doesn't provide any path or file. The error page will be send when a 4XX error code occurs. Both pages need to be
set up on your bucket properties.

> To give your user better feedback you should setup extra error pages for specific error codes.

Also place a `.travis.yml` file for travis.ci. We will fill in this file in the next paragraph.

### Setup Travis

#### travis.yml

Explanation of whats happening:

- `install`: The install block will download the `aws-cli` and add it to the `PATH` variable
- `script`: In the script block, we will use the `aws-cli` to first sync all files with S3. The sync command has also
the `--delete` tag to first delete all files on S3 and then upload the new ones. Next we will enable the CloudFront
feature and then invalidate all files. 

The `script` block can also be used to build your project if you have a javascript app or something else.

#### travis.ci settings

Add the following environment variables on travis.ci or as 
[encrypted values](https://docs.travis-ci.com/user/environment-variables/#Defining-encrypted-variables-in-.travis.yml):

- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY`
- `CLOUDFRONT_DISTRIBUTION_ID`

## Further information's

- Blog article on how to setup S3 on AWS: https://renzo.lucioni.xyz/s3-deployment-with-travis/
