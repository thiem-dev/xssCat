## AWS

### AWS buckets
- testing guide: https://blog.intigriti.com/hacking-tools/hacking-misconfigured-aws-s3-buckets-a-complete-guide 
- regex to find buckets `\.s3\.amazonaws\.com\/?`
  - or look for: `x-amz-bucket-region` `x-amz-request-id` `x-amz-id-2`
  - google dork: `site:.s3.amazonaws.com "company"`
  - Automated tools such as S3enum and cloud_enum can help you enumerate AWS S3 buckets
- bad configued bucket check cmds
  - `aws s3 ls s3://{BUCKET_NAME} --no-sign-request`

### cognito create new user
- https://security.theodo.com/en/blog/aws-cognito-pentest
  - the self-sign up shouldn't be misconfigured and the --no-sign-request should not be allowed either 
- if ever given aws credentials like these. 
  ```js 
    USER_POOL_ID: 'us-east-1_[redacted]',
    APP_CLIENT_ID: '[redacted]',
    IDENTITY_POOL_ID: 'us-east-1:[redacted]',
    IDP: '[org-redacted]-idm-saml-impl',

    //note this doesn't use aws-configure settings because all your commands override those
    //then use: 
    aws cognito-idp sign-up --client-id [redacted app-client-id] --username pentest12345@yopmail.com --password Password1234! --region us-east-1 --no-sign-request --user-attributes '[ {"Name":"email", "Value": "pentest12345@yopmail.com"}]'
  ```