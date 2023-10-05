# AWS/Terraform

## Requirements

1. Create a network with public and private subnets
2. Provision an ECS cluster and load balancer
3. Add an ECS service that serves the base nginx image
4. Set the default route for the ALB that serves the default route of the nginx image
5. Create an S3 bucket and configure the Terraform to allow the nginx task to write to the S3 bucket

## These are the steps I took to complete this portion of the assessment:

I took a low-code approach by leveraging existing open source projects and modifying them just enough to meet the requirements of the assessment.

I am happy to walk through the implementation with the SourceFuse team, if interested.

The final public endpoint showing the Nginx page:

http://nikos-test-ecs-web-app-464196450.us-east-2.elb.amazonaws.com/

1. Researched the open source options available.
2. Selected a repo from Cloud Posse named terraform-aws-ecs-web-app that was similar to the requirements.
3. Forked that repo into a personal Github repo.
4. Updated the Cloud Posse module versions to recent versions.
5. Changed the Docker registry container image name to "nginx".
6. Disabled the CodePipeline resources as they were not necessary.
7. Added my AWS local profile name to the provider.
8. Updated the Terraform variables related to security groups and public IPs, to allow the ECS service task to download the nginx image from the internet.
9. Applied the Terraform to my AWS account.
![aws6_terraform_apply_output](https://github.com/timclark-ai/terraform-aws-ecs-web-app/assets/24612753/9934fc57-ddac-42ad-b64e-a307cfd89d09)

10. Verified that a new VPC was created, with 2 public subnets and 2 private subnets across 2 availability zones. Also an ALB was created having the ECS service task as the target.
![aws1_vpc_subnets](https://github.com/timclark-ai/terraform-aws-ecs-web-app/assets/24612753/333ed723-cb76-4485-a51d-a7b82ab8de7e)
![aws2_alb_dns](https://github.com/timclark-ai/terraform-aws-ecs-web-app/assets/24612753/c7c7c1d7-b9d5-4063-8c2f-f0ea962b9125)
![aws3_alb_rules](https://github.com/timclark-ai/terraform-aws-ecs-web-app/assets/24612753/5f8f5ec2-6a9d-4ee4-97d6-299a4a6e7baa)
![aws4_ecs_task_container](https://github.com/timclark-ai/terraform-aws-ecs-web-app/assets/24612753/141448f0-9464-4c17-a8d1-bb2958b71f78)

11. Verified the public endpoint utilizes the default route of the ALB to serve the default Nginx page.
![aws5_public_endpoint](https://github.com/timclark-ai/terraform-aws-ecs-web-app/assets/24612753/9766dd4a-89ae-4739-878c-c9e18aca0d0b)

12. Verified the ALB access logs are being written to an S3 bucket that was created through Terraform.
![aws7_s3_objects](https://github.com/timclark-ai/terraform-aws-ecs-web-app/assets/24612753/bf7755a5-b743-471a-a289-e89d6257363b)

13. Verified the list of task definitions.
![aws8_task_definitions](https://github.com/timclark-ai/terraform-aws-ecs-web-app/assets/24612753/4230043e-bcb1-464d-b883-bfe6bf3d9d16)
