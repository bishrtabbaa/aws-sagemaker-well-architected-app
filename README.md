# aws-sagemaker-well-architected-app

## :brain: Amazon SageMaker Well-Architected App Overview

[Amazon SageMaker](https://aws.amazon.com/sagemaker/) is a managed service for data science and machine learning (ML) workflows. You can use Amazon SageMaker to simplify the process of building, training, and deploying ML models.

This Git repo contains IAM policy documents, Python code, and CloudFormation templates to help you optimize your SageMaker environment for security, cost, and performance.  It follows the prescriptive guidance found in the following whitepapers and references below:

### References
* [AWS Machine Learning Well-Architected Lens] (https://aws.amazon.com/blogs/architecture/introducing-the-well-architected-framework-for-machine-learning/)
* [Secure ML Platform on AWS] (https://docs.aws.amazon.com/whitepapers/latest/build-secure-enterprise-ml-platform/build-secure-enterprise-ml-platform.html)
* [SageMaker Studio Security Workshop] (https://github.com/aws-samples/amazon-sagemaker-studio-secure-data-science-workshop)
* [SageMaker DevOps Workshop] (https://sagemaker-workshop.com/security_for_sysops.html)

### CLI commands

#### 01 Policies 

The primary purpose of these custom policies is separating Administrator vs. Engineer roles and also limiting the instance types allowed for users so as to minimize costs.  Note the use of the `sagemaker:InstanceTypes` IAM condition key in the `SageMakerDataScienceEngineerPolicy` and `SageMakerDataScienceAdministratorPolicy` documents.

```
aws iam create-policy --policy-name SageMakerComputePolicy --policy-document file://SageMakerComputePolicy.json
aws iam create-policy --policy-name SageMakerStoragePolicy --policy-document file://SageMakerStoragePolicy.json
aws iam create-policy --policy-name SageMakerSecurityPolicy --policy-document file://SageMakerSecurityPolicy.json
aws iam create-policy --policy-name SageMakerDataScienceEngineerPolicy --policy-document file://SageMakerDataScienceEngineerPolicy.json
aws iam create-policy --policy-name SageMakerDataScienceAdministratorPolicy --policy-document file://SageMakerDataScienceAdministratorPolicy.json
```

#### 02 Roles

You MUST grant the `sts:AssumeRole` permission to SageMaker to run on-behalf of the user with these policies.

```
aws iam create-role --role-name SageMakerDataScienceEngineerRole --assume-role-policy-document file://SageMakerServiceRoleTrustPolicy.json
aws iam create-role --role-name SageMakerDataScienceAdministratorRole --assume-role-policy-document file://SageMakerServiceRoleTrustPolicy.json
```

#### 03 Attach Policies to Roles

You MUST attach the aformentioned custom policies to the roles.  Note that the AWS Account ID in the policy ARN MUST be replaced for your respective environment.

```
aws iam attach-role-policy --role-name SageMakerDataScienceEngineerRole --policy-arn arn:aws:iam::645411899653:policy/SageMakerDataScienceEngineerPolicy
aws iam attach-role-policy --role-name SageMakerDataScienceEngineerRole --policy-arn arn:aws:iam::645411899653:policy/SageMakerComputePolicy
aws iam attach-role-policy --role-name SageMakerDataScienceEngineerRole --policy-arn arn:aws:iam::645411899653:policy/SageMakerStoragePolicy
aws iam attach-role-policy --role-name SageMakerDataScienceEngineerRole --policy-arn arn:aws:iam::645411899653:policy/SageMakerSecurityPolicy
aws iam attach-role-policy --role-name SageMakerDataScienceAdministratorRole --policy-arn arn:aws:iam::645411899653:policy/SageMakerDataScienceAdministratorPolicy
aws iam attach-role-policy --role-name SageMakerDataScienceAdministratorRole --policy-arn arn:aws:iam::645411899653:policy/SageMakerComputePolicy
aws iam attach-role-policy --role-name SageMakerDataScienceAdministratorRole --policy-arn arn:aws:iam::645411899653:policy/SageMakerStoragePolicy
aws iam attach-role-policy --role-name SageMakerDataScienceAdministratorRole --policy-arn arn:aws:iam::645411899653:policy/SageMakerSecurityPolicy
```

#### 04.Optional

Attach additional policies as needed based on your scenario and use cases.

```
aws iam attach-role-policy --role-name SageMakerDataScienceEngineerRole --policy-arn arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess
aws iam attach-role-policy --role-name SageMakerDataScienceEngineerRole --policy-arn arn:aws:iam::aws:policy/AmazonSageMakerReadOnly
aws iam attach-role-policy --role-name SageMakerDataScienceEngineerRole --policy-arn arn:aws:iam::aws:policy/AWSServiceCatalogEndUserFullAccess
aws iam attach-role-policy --role-name SageMakerDataScienceEngineerRole --policy-arn arn:aws:iam::aws:policy/AmazonEC2ContainerRegistryReadOnly
aws iam attach-role-policy --role-name SageMakerDataScienceAdministratorRole --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess
aws iam attach-role-policy --role-name SageMakerDataScienceAdministratorRole --policy-arn arn:aws:iam::aws:policy/AmazonSageMakerReadOnly
aws iam attach-role-policy --role-name SageMakerDataScienceAdministratorRole --policy-arn arn:aws:iam::aws:policy/AWSServiceCatalogAdminFullAccess
aws iam attach-role-policy --role-name SageMakerDataScienceAdministratorRole --policy-arn arn:aws:iam::aws:policy/AmazonEC2ContainerRegistryFullAccess
```

##### SCRATCH attaching policy update

There is a default IAM limit of 5 versions of a policy.  If you find yourself updating a policy frequently, you may need to update and set the latest as the default.

```
aws iam create-policy-version  --policy-arn arn:aws:iam::645411899653:policy/SageMakerDataScienceEngineerPolicy --policy-document file://SageMakerDataScienceEngineerPolicy.json --set-as-default
```