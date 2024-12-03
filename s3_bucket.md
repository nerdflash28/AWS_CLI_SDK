## To List S3 buckets
```bash

aws s3 ls

```

### To remove a bucket 

```bash
# to get bucket arn we the s3api 
bucketToDelete=$(aws s3api list-buckets --output text --query 'Buckets[?contains(Name,`deletemebucket`) == `true`] | [0].Name')

# to delete bucket we use given below command
aws s3 rb s3://bucketToDelete
```

### To debug any errors while deleting bucket we use
```bash
aws s3 rb s3://bucketToDelete --debug
```

### to attach policy to given role
* we are unable to delete bucket because our given role dont have permission to delete bucket 
* to add permission we use the given command
```bash
# get policy arn
policArn=$(aws iam list-policies --output text --query  'Policies[?PolicyName == `S3-Delete-Bucket-Policy`].Arn')

# print policy document 
aws iam get-policy-version --policy-arn $policyArn --version-id v1

# attaching the policy to given role 
aws iam attach-role-policy --policy-arn $policyArn --role-name notes-application-role

# geting list of policies attached with given role 
aws iam list-attached-role-policies --role-name notes-application-role

# now we can delete the bucket with given command 
aws s3 rb s3://$bucketToDelete

# to confirm we can list the aws s3 buckets
aws s3 ls

```