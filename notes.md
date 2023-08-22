# Create Stack using Terraform
`tf apply`

Apply takes roughly 20 minutes to perform

## What does this do?
- Creates a VPC
- Creates an EKS using the VPC
    - Creates a node_group
- Creates an ebs csi driver 
    - This allows the EKS to manage Elastic Block Storage lifecycles
    - Additionally creates an irsa-ebs-csi role
    - Additionally creates an aws iam policy for the EBS CLI driver


# Update local kubectl configuration
```
aws eks --region $(terraform output -raw region) update-kubeconfig \
    --name $(terraform output -raw cluster_name)
```

# Update the configmap/aws-auth

1. `kubectl edit -n kube-system configmap/aws-auth`
2. Add the following:
```
mapUsers:
----
- groups:
  - system:master
  userarn: arn:aws:iam::461706357402:user/phillyp.henning
  username: phillyp.henning
```

Sign into AWS console with: https://461706357402.signin.aws.amazon.com/console
Username and password are stored in 1Password

At this point resources should now be viewable in the AWS Console

# Updating Lens to allow view access
Ensure Lens is opened from the terminal after running `prep_personal` function

`open -n /Applications/Lens.app`