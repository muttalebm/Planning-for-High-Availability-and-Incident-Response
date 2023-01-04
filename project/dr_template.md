# Infrastructure

## AWS Zones
Primary Zone: `us-east-2`

Availability Zones:`us-east-2a, us-east-2b`

Secondary Zone: `us-west-1`

Availability Zones: `us-west-1a,us-west-1b`

## Servers and Clusters

### Table 1.1 Summary
| Asset                | Purpose                                        | Size                    | Qty                 | DR                      |
|----------------------|------------------------------------------------|-------------------------|---------------------|-------------------------|
| EC2 instance Primary | Application Server                             | t3.micro                | 3                   | replicated in us-west-1 |
| EC2 instance DR      | Replicated Application Server                  | t3.micro                | 3                   |                         |
| EKS Cluster Primary  | K8s cluster for monitoring Cluster for primary | t3.medium               | 3(1 control,2 node) | Replicated in us-west-1 |
| EKS Cluster DR       | K8s cluster for monitoring Cluster for DR      | t3.medium               | 3(1 control,2 node) |                         |
| VPC                  | Application Network                            | Replicated in us-west-1 |                     |                         |
| VPC DR               | Application Network DR                         |                         |                     |                         |
| Load balancer        | Application load balancer                      | Replicated in us-west-1 |                     |                         |
| Load balancer DR     | Application load balancer for DR               |                         |                     |                         |
| MySQL RDS            | Application Database                           | db.t3.micro             | 2(1 Master,1 slave) | replicated in us-west-1 |
| MySQL RDS DR         | Application Database                           | db.t3.micro             | 2(1 Master,1 slave) |                         |

### Descriptions
#### EC2 Instances

The Main Application server that hosts the application, contains 3 EC 2 instances in both Zones
2 Key Pairs for 2 Regions

#### EKS Cluster

The Grafana prometheus monitoring cluster available in both zones. Each EKS cluster has 1 control panel and 2 worker nodes 

#### VPC

Your own virtual private network, allows Application and other infrastructure to talk to each other. Deployed in both zones
#### Load Balancer 

Handles user traffic to application server and distributes to all nodes in a availability zone to ensure HA   

#### MySQL RDS Database

A 2 node MySQL cluster available in both zones, with 5 days backup retention. The secondary zone replicates from primary zone

### S3 Buckets

Two S3 buckets for terraform in two regions

## DR Plan
### Pre-Steps:
- 3 EC2 instances, in a separate Region with HA enabled
- SSH keys for administering the EC2 instances
- Backend database running on 3 Amazon RDS nodes for the website, which is replicated cluster from Region 1
- Database backups stored in S3 for recovery
- Load balancer for the API
- Kubernetes cluster for monitoring stack

## Steps:

 - Check that Route 53 fai  lover routing for the application endpoint has completed.
 - Confirm the RDS cluster failed over.
 - Confirm the failed over application instance is able to read/write from/to the failed over RDS cluster