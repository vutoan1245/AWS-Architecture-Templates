
# Static Website with S3 and CloudFront

This CloudFormation template sets up a static website hosted on S3 with CloudFront as the CDN.

## Resources Created

1. **S3 Bucket**: Configured to host a static website with public read access.
2. **Bucket Policy**: Allows public read access to the objects in the S3 bucket.
3. **CloudFront Origin Access Identity (OAI)**: Restricts direct access to the S3 bucket.
4. **CloudFront Distribution**: Distributes the content from the S3 bucket globally.

## Template

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Resources:
  S3Bucket:
    Type: 'AWS::S3::Bucket'
    Properties:
      BucketName: !Sub '${AWS::StackName}-bucket'
      WebsiteConfiguration:
        IndexDocument: index.html
        ErrorDocument: error.html
      AccessControl: PublicRead

  S3BucketPolicy:
    Type: 'AWS::S3::BucketPolicy'
    Properties:
      Bucket: !Ref S3Bucket
      PolicyDocument:
        Version: '2012-10-17'
        Statement:
          - Effect: Allow
            Principal: '*'
            Action: 's3:GetObject'
            Resource: !Sub 'arn:aws:s3:::${S3Bucket}/*'

  CloudFrontOriginAccessIdentity:
    Type: 'AWS::CloudFront::CloudFrontOriginAccessIdentity'
    Properties:
      CloudFrontOriginAccessIdentityConfig:
        Comment: !Sub '${AWS::StackName} Origin Access Identity'

  CloudFrontDistribution:
    Type: 'AWS::CloudFront::Distribution'
    Properties:
      DistributionConfig:
        Enabled: true
        Origins:
          - Id: !Sub '${S3Bucket}-origin'
            DomainName: !GetAtt S3Bucket.DomainName
            S3OriginConfig:
              OriginAccessIdentity: !Sub 'origin-access-identity/cloudfront/${CloudFrontOriginAccessIdentity}'
        DefaultCacheBehavior:
          TargetOriginId: !Sub '${S3Bucket}-origin'
          ViewerProtocolPolicy: 'redirect-to-https'
          AllowedMethods:
            - GET
            - HEAD
          CachedMethods:
            - GET
            - HEAD
          ForwardedValues:
            QueryString: false
            Cookies:
              Forward: 'none'
        ViewerCertificate:
          CloudFrontDefaultCertificate: true
        DefaultRootObject: index.html

Outputs:
  WebsiteURL:
    Description: 'URL for static website'
    Value: !Sub 'http://${S3Bucket}.s3-website-${AWS::Region}.amazonaws.com'
  CloudFrontURL:
    Description: 'CloudFront URL'
    Value: !GetAtt CloudFrontDistribution.DomainName
```

## Deployment

You can deploy this template using the AWS Management Console or the AWS CLI. To deploy using the AWS CLI, save the template as `template.yaml` and run the following command:

```sh
aws cloudformation create-stack --stack-name my-static-website --template-body file://template.yaml --capabilities CAPABILITY_NAMED_IAM
```

Replace `my-static-website` with your desired stack name. This will create the necessary AWS resources to host a static website using S3 and CloudFront.

## License

This project is licensed under the MIT License.
