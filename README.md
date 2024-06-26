Task:
Automate EKS cluster setup with Karpenter and Graviton on AWS

The automation is done using terraform which will:
- create EKS cluster
- setup Karpenter using helm provider
- configure Karpenter nodepool to support both amd64 and arm64(Graviton) instances


Prerequisites:
 Existing VPC
 An AWS user with admin privileges is created and credentials are configured

Steps:
1.  Add tag "karpenter.sh/discovery = <cluster_name>" to private subnets and security group which will be utilized by Karpenter
2.  Edit terraform.tfvars with correct values
3.  Install aws cli
    https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html
4.  Install kubectl
    https://kubernetes.io/docs/tasks/tools/
5.  Run terraform commands inside eks-karpenter-infra folder:
    - terraform init
    - terraform plan
    - run terraform apply
5.  Get kubeconfig
    aws sts get-caller-identity
    aws eks update-kubeconfig --name <cluster_name>

After these steps cluster is ready for use.


Usage:
    From a developer's perspective, using EKS with Karpenter setup is same using EKS with autoscaling setup. To run both amd64 and arm64 images developers need to create the deployment file, define the image (either arm64 or amd64) and run the deployment. Karpenter will automatically recognize the architecture of the image and provision the appropriate instance to run the container.
    This is demonstrated through two examples(examples folder) where Nginx is deployed one using an arm64 image and the other using an amd64 image. As shown, the only difference is the specified image.
    Node autoscaling mechanism should be consider while configuration of pod autscaling.
    

NOTE: The project is based on the documentation and examples provided by the official Terraform Karpenter module.
https://registry.terraform.io/modules/terraform-aws-modules/eks/aws/latest/submodules/karpenter
https://registry.terraform.io/modules/terraform-aws-modules/eks/aws/latest/examples/karpenter
