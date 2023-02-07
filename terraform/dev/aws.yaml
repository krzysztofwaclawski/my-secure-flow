provider "aws" {
  region = "us-west-2"
}

module "vpc" {
  source = "terraform-aws-modules/vpc/aws"
  version = "3.0.0"

  name = "k8s-vpc"
  cidr = "10.0.0.0/16"
  subnets = [
    {
      cidr = "10.0.0.0/24"
      azs  = ["us-west-2a"]
      type = "private"
    },
    {
      cidr = "10.0.1.0/24"
      azs  = ["us-west-2b"]
      type = "private"
    },
    {
      cidr = "10.0.2.0/24"
      azs  = ["us-west-2c"]
      type = "private"
    },
  ]
}

module "eks" {
  source = "terraform-aws-modules/eks/aws"
  version = "15.0.0"

  cluster_name = "k8s-cluster"
  subnets = module.vpc.private_subnets
  vpc_id = module.vpc.vpc_id
  vpc_security_group_ids = [module.vpc.default_security_group_id]
  tags = {
    Terraform   = "true"
    Environment = "dev"
  }
}

output "kubeconfig" {
  value = module.eks.kubeconfig
}
