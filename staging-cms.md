This page was created when a staging site was needed for upgrading the CMS.

## Steps

## Backup the Database
Backup the database by taking a snapshot of the instance. 
### Initiate a staging RDS
Take a snapshot of the current production database and restore it to a staging instance.
### Create a staging Image

builds the image with staging
`docker build -f docker/cms/Dockerfile.staging -t deako/cms-staging:latest .` 

tag the image with was region and repo
`docker tag deako/cms-staging:latest 941585028052.dkr.ecr.us-east-1.amazonaws.com/deako/cms-staging:latest tag the image

refresh authorization token
`aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 941585028052.dkr.ecr.us-east-1.amazonaws.com` 

push the image
`docker push 941585028052.dkr.ecr.us-east-1.amazonaws.com/deako/cms-staging:latest` 

`deako/cms:1.10` build with 4.25.17 latest strapi

run the image
`docker run --env-file apps/cms/staging.env -p 1337:1337 deako/cms-staging:latest`






## notes:

### production

name: `deako/cms` 

build cmd: `docker build -f docker/cms/Dockerfile -t deako/cms:latest .` 

### staging

name: `deako/cms-staging`

build cmd: `docker build -f docker/cms/Dockerfile -t deako/cms-staging:latest .` 

tag image: `docker tag deako/cms-staging:latest 941585028052.dkr.ecr.us-east-1.amazonaws.com/deako/cms-staging:latest`

push image: `docker push 941585028052.dkr.ecr.us-east-1.amazonaws.com/deako/cms-staging:latest`


## security groups

`s3` iam role that allows `ecs` to access `s3` bucket
`rds` sg that allows `ecs` sg to access `rds` port 3306

has `sg-0cb6c0ebe875cdebf - dev-cms-ecs-sg`: which allows inbound: `sg-0cb6c0ebe875cdebf - dev-cms-ecs-sg` (access to 3306)


`ecs` sg that allows `1337` from `alb` sg

`alb` sg that allows `80` from `cloudfront` sg
`cloudfront` sg that allows `80` and `443` from all