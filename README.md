# static-blog

Blog site created using the [mkdocs](http://www.mkdocs.org) static site generator.

## setup

added the following to the git pre-push hook so that it builds abd syncs the generated site to a S3 bucket

```
mkdocs build
export AWS_PROFILE=WHATEVER_YOUR_AWS_PROFILE_IS
aws s3 sync site/ s3://BUCKET_NAME
```
