# AWS ECS Blue/Green Deployment Lab 🚀

This project demonstrates a production-grade CI/CD workflow for containerized applications using **Amazon ECS (Fargate)** and **AWS CodeDeploy**.

## 📐 Architecture Diagram
The following diagram represents the cloud infrastructure and deployment flow:

```mermaid
graph TD
    subgraph "Public Internet"
        User((User))
    end

    subgraph "AWS Cloud (VPC)"
        ALB[Application Load Balancer]
        
        subgraph "ECS Cluster"
            Service[ECS Service: my-service]
            TaskBlue[Task Set: Blue - v5]
            TaskGreen[Task Set: Green - v6]
        end
        
        subgraph "Deployment Control"
            CD[AWS CodeDeploy]
            TG1[(Target Group 1: Blue)]
            TG2[(Target Group 2: Green)]
        end
    end

    subgraph "Registry"
        ECR[(Amazon ECR)]
    end

    User --> ALB
    ALB -- Port 80 --> TG1
    ALB -- Port 8080 --> TG2
    TG1 --> TaskBlue
    TG2 --> TaskGreen
    CD -- "Traffic Shift" --> ALB
    ECR -- "Image Pull" --> TaskGreen
```

## ✅ Deployment Validation & Logs
*Note: Infrastructure was decommissioned post-testing for cost optimization.*

### 1. Verifying Running Tasks
```bash
$ aws ecs list-tasks --cluster ecs-cluster --service-name my-service --desired-status RUNNING
{
    "taskArns": [
        "arn:aws:ecs:us-east-1:582726398736:task/ecs-cluster/e1c4d47f494643938a0a5b7eddb03910"
    ]
}
```

---
**Author:** [Adetunji Mathew Babatunde](http://www.linkedin.com)
**Certifications:** AWS SAA, AWS DVA, AWS AI Practitioner.
