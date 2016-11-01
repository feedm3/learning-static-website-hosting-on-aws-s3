# Learning static website hosting on AWS S3

This repo gets automatically deployed to S3 via CodePipeline.

## How to

1. Create a repository with your website.
2. Create a new bucket on S3
    - On the properties tab, click on _Static Website Hosting_ and check _Enable Website Hosting_.
    Fill in the index and error page.
    - On the properties tab, click on _Permissions_ and _Edit bucket policy_. 
    Fill in the following police but replace YOUR_BUCKET_NAME with your bucket name
   
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
3. Create a new pipeline on CodePipeline
