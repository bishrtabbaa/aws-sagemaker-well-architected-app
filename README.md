# aws-sagemaker-cost-optimization-app

## :brain: Amazon SageMaker Well-Architected App Overview

[Amazon SageMaker](https://aws.amazon.com/sagemaker/) is a managed service for data science and machine learning (ML) workflows. You can use Amazon SageMaker to simplify the process of building, training, and deploying ML models.

This Git repo contains IAM policy documents, Python code, and CloudFormation templates to help you optimize your SageMaker environment for security, cost, and performance.  It follows the prescriptive guidance found in the [AWS Machine Learning Well-Architected Lens] (https://aws.amazon.com/blogs/architecture/introducing-the-well-architected-framework-for-machine-learning/) and the [Secure ML Platform on AWS Whitepaper] (https://docs.aws.amazon.com/whitepapers/latest/build-secure-enterprise-ml-platform/build-secure-enterprise-ml-platform.html)

### CLI commands

#### 01 Policies
```
aws iam create-policy --policy-name SageMakerComputePolicy --policy-document file://SageMakerComputePolicy.json
aws iam create-policy --policy-name SageMakerStoragePolicy --policy-document file://SageMakerStoragePolicy.json
aws iam create-policy --policy-name SageMakerSecurityPolicy --policy-document file://SageMakerSecurityPolicy.json
aws iam create-policy --policy-name SageMakerDataScienceEngineerPolicy --policy-document file://SageMakerDataScienceEngineerPolicy.json
aws iam create-policy --policy-name SageMakerDataScienceAdministratorPolicy --policy-document file://SageMakerDataScienceAdministratorPolicy.json
```

#### 02 Roles
```
aws iam create-role --role-name SageMakerDataScienceEngineerRole --assume-role-policy-document file://SageMakerServiceRoleTrustPolicy.json
aws iam create-role --role-name SageMakerDataScienceAdministratorRole --assume-role-policy-document file://SageMakerServiceRoleTrustPolicy.json
```

#### 03 Attach Policies to Roles ... DataScienceEngineer, DataScienceAdministrator
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
```
aws iam attach-role-policy --role-name SageMakerDataScienceEngineerRole --policy-arn arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess
aws iam attach-role-policy --role-name SageMakerDataScienceEngineerRole --policy-arn arn:aws:iam::aws:policy/AmazonSageMakerReadOnly
aws iam attach-role-policy --role-name SageMakerDataScienceEngineerRole --policy-arn arn:aws:iam::aws:policy/AWSServiceCatalogEndUserFullAccess
aws iam attach-role-policy --role-name SageMakerDataScienceEngineerRole --policy-arn arn:aws:iam::aws:policy/AmazonEC2ContainerRegistryReadOnly
aws iam attach-role-policy --role-name SageMakerDataScienceEngineerRole --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess
aws iam attach-role-policy --role-name SageMakerDataScienceEngineerRole --policy-arn arn:aws:iam::aws:policy/AmazonSageMakerReadOnly
aws iam attach-role-policy --role-name SageMakerDataScienceEngineerRole --policy-arn arn:aws:iam::aws:policy/AWSServiceCatalogAdminFullAccess
aws iam attach-role-policy --role-name SageMakerDataScienceEngineerRole --policy-arn arn:aws:iam::aws:policy/AmazonEC2ContainerRegistryFullAccess
```

##### SCRATCH attaching policy update
aws iam create-policy-version  --policy-arn arn:aws:iam::645411899653:policy/SageMakerDataScienceEngineerPolicy --policy-document file://SageMakerDataScienceEngineerPolicy.json --set-as-default