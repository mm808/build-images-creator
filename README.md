## What is this repository for? ##
This project provides a way to create and deploy custom builder images that can be used in CodeBuild (or other CI tools) projects. The idea is to have this project build Docker containers that are specific to a CI project and have the pre-reqs installed already so there is little or no pre-build setup in the CI project.        
This should decrease build times and eliminate build time errors due to tool chain install issues such as deprecated items or unavailable versions.   
The goal is to have all images get pushed to one account's ECR as the central place all images are stored. The caveat is that the image repository needs to allow push/pull rights to the resources that need the image in any account across the organization. So carefully modify _each image repo_ security policy to allow other accounts access. Here is a sample JSON to allow CodeBuild service in the Dev account permission to pull from Prod:   

{
  "Version": "2008-10-17",   
  "Statement": [   
    {   
      "Sid": "DevECSImageRetrievalPolicy",   
      "Effect": "Allow",   
      "Principal": {   
        "Service": "ecs.amazonaws.com"   
      },   
      "Action": [   
        "ecr:BatchGetImage",   
        "ecr:DeleteRepositoryPolicy",   
        "ecr:GetDownloadUrlForLayer",   
        "ecr:GetRepositoryPolicy",   
        "ecr:SetRepositoryPolicy"   
      ],   
      "Condition": {   
        "StringLike": {   
          "aws:sourceArn":"arn:aws:codebuild:us-east-1:1234567890:*"   
        }   
      }   
    }    
  ]   
}   

## Use ##
- Make sure the ECR repo exists for any new image you are creating! This project assumes the repo exists already.   
- Create a new directory with the name of the image you want to create in the Images directory. Put a Dockerfile for your image in the new directory.
- Use the Build with Parameters link in Jenkins    
COMMIT=the commit hash   
IMAGE_NAME=image name matching the directory name for the item to build   
ENVIRONMENT=the environment associated with where the build should go
