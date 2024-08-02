# Static Website CloudFormation Template

This CloudFormation template provisions a fully functional static website using Amazon S3 with public access.

## Template Details

### AWSTemplateFormatVersion

`2010-09-09`

### Resources

#### S3Bucket

Creates an S3 bucket configured to host a static website with `index.html` as the index document and `error.html` as the error document.

```yaml
S3Bucket:
  Type: "AWS::S3::Bucket"
  Properties:
    PublicAccessBlockConfiguration:
      BlockPublicAcls: false
      BlockPublicPolicy: false
      IgnorePublicAcls: false
      RestrictPublicBuckets: false
    WebsiteConfiguration:
      IndexDocument: index.html
      ErrorDocument: error.html
  DeletionPolicy: Retain
  UpdateReplacePolicy: Retain
```

#### BucketPolicy

Defines a bucket policy to allow public read access to the contents of the S3 bucket.

```yaml
BucketPolicy:
  Type: "AWS::S3::BucketPolicy"
  Properties:
    PolicyDocument:
      Id: MyPolicy
      Version: 2012-10-17
      Statement:
        - Sid: PublicReadForGetBucketObjects
          Effect: Allow
          Principal: "*"
          Action: "s3:GetObject"
          Resource: !Join
            - ""
            - - "arn:aws:s3:::"
              - !Ref S3Bucket
              - /*
    Bucket: !Ref S3Bucket
```

### Outputs

#### WebsiteURL

The URL for the website hosted on S3.

```yaml
WebsiteURL:
  Value: !GetAtt
    - S3Bucket
    - WebsiteURL
  Description: URL for website hosted on S3
```

#### S3BucketSecureURL

The secure URL for the S3 bucket.

```yaml
S3BucketSecureURL:
  Value: !Join
    - ""
    - - "https://"
      - !GetAtt
        - S3Bucket
        - DomainName
  Description: Name of S3 bucket to hold website content
```

## Deployment

To deploy the CloudFormation stack, use the following AWS CLI command:

```sh
aws cloudformation create-stack --stack-name StaticWebsiteStack --template-body file://cloudformation.yaml --capabilities CAPABILITY_NAMED_IAM
```

Replace `cloudformation.yaml` with the path to your CloudFormation template file.
