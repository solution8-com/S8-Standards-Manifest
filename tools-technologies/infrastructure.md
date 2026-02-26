# Infrastructure

## Overview

Modern infrastructure is cloud-native, containerized, and managed as code. This guide covers the tools and practices for building, deploying, and managing scalable, reliable infrastructure.

## Cloud Providers

### Amazon Web Services (AWS)

**When to Use:**
- Need broadest service catalog
- Global presence required
- Enterprise-grade compliance
- Deep integration with specific AWS services

**Core Services:**

**Compute:**
- **EC2**: Virtual servers
- **Lambda**: Serverless functions
- **ECS/EKS**: Container orchestration
- **Fargate**: Serverless containers

**Storage:**
- **S3**: Object storage
- **EBS**: Block storage
- **EFS**: File storage
- **Glacier**: Archive storage

**Database:**
- **RDS**: Managed relational databases
- **DynamoDB**: NoSQL database
- **ElastiCache**: In-memory caching
- **Aurora**: High-performance MySQL/PostgreSQL

**Networking:**
- **VPC**: Virtual private cloud
- **Route 53**: DNS service
- **CloudFront**: CDN
- **API Gateway**: API management

**Best Practices:**
- Use IAM roles instead of access keys
- Enable CloudTrail for audit logging
- Implement least privilege access
- Use AWS Organizations for multi-account strategy
- Tag resources consistently
- Enable Cost Explorer for cost monitoring

### Microsoft Azure

**When to Use:**
- Microsoft stack (.NET, Windows Server)
- Integration with Microsoft 365
- Hybrid cloud scenarios
- Strong enterprise support

**Core Services:**
- **Virtual Machines**: Compute instances
- **App Service**: PaaS for web apps
- **Azure Functions**: Serverless compute
- **AKS**: Kubernetes service
- **Cosmos DB**: Multi-model database
- **Blob Storage**: Object storage
- **Azure DevOps**: CI/CD platform

**Best Practices:**
- Use Managed Identities for authentication
- Implement Azure Policy for governance
- Use Resource Groups for organization
- Enable Azure Monitor
- Follow Well-Architected Framework

### Google Cloud Platform (GCP)

**When to Use:**
- Data analytics and ML workloads
- Kubernetes-native applications
- Need for BigQuery
- Developer-friendly tools

**Core Services:**
- **Compute Engine**: Virtual machines
- **Cloud Functions**: Serverless functions
- **GKE**: Kubernetes engine
- **Cloud Run**: Serverless containers
- **BigQuery**: Data warehouse
- **Cloud Storage**: Object storage
- **Cloud SQL**: Managed databases

**Best Practices:**
- Use service accounts for authentication
- Organize with projects and folders
- Enable Cloud Audit Logs
- Use VPC Service Controls
- Implement Organization Policies

### Multi-Cloud Strategy

**Benefits:**
- Avoid vendor lock-in
- Leverage best-of-breed services
- Redundancy and disaster recovery
- Negotiate better pricing

**Challenges:**
- Increased complexity
- Higher operational overhead
- Need for cloud-agnostic tools
- Staff training across platforms

**Recommended Approach:**
- Use Kubernetes for compute portability
- Standardize on Terraform for IaC
- Abstract cloud-specific services when possible
- Document cloud-specific implementations

## Container Technologies

### Docker

**Core Concepts:**

**Dockerfile Best Practices:**
```dockerfile
# Use specific base image versions
FROM node:18-alpine

# Set working directory
WORKDIR /app

# Copy dependency files first (better caching)
COPY package*.json ./

# Install dependencies
RUN npm ci --only=production

# Copy application code
COPY . .

# Use non-root user
RUN addgroup -g 1001 -S nodejs && \
    adduser -S nodejs -u 1001
USER nodejs

# Expose port
EXPOSE 3000

# Health check
HEALTHCHECK --interval=30s --timeout=3s \
  CMD node healthcheck.js

# Start application
CMD ["node", "server.js"]
```

**Multi-stage Builds:**
```dockerfile
# Build stage
FROM node:18 AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Production stage
FROM node:18-alpine
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules
USER node
CMD ["node", "dist/server.js"]
```

**Best Practices:**
- Use official base images
- Pin image versions
- Minimize layer count
- Use .dockerignore
- Scan images for vulnerabilities
- Keep images small
- Never store secrets in images

### Kubernetes

**When to Use:**
- Need container orchestration at scale
- High availability requirements
- Microservices architecture
- Multi-cloud deployment

**Core Concepts:**

**Deployment Example:**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-app
  labels:
    app: web-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web-app
  template:
    metadata:
      labels:
        app: web-app
    spec:
      containers:
      - name: web-app
        image: myregistry/web-app:v1.2.3
        ports:
        - containerPort: 8080
        env:
        - name: DATABASE_URL
          valueFrom:
            secretKeyRef:
              name: db-credentials
              key: url
        resources:
          requests:
            memory: "256Mi"
            cpu: "250m"
          limits:
            memory: "512Mi"
            cpu: "500m"
        livenessProbe:
          httpGet:
            path: /health
            port: 8080
          initialDelaySeconds: 30
          periodSeconds: 10
        readinessProbe:
          httpGet:
            path: /ready
            port: 8080
          initialDelaySeconds: 5
          periodSeconds: 5
```

**Service and Ingress:**
```yaml
apiVersion: v1
kind: Service
metadata:
  name: web-app-service
spec:
  selector:
    app: web-app
  ports:
  - protocol: TCP
    port: 80
    targetPort: 8080
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: web-app-ingress
  annotations:
    cert-manager.io/cluster-issuer: letsencrypt-prod
spec:
  tls:
  - hosts:
    - app.example.com
    secretName: web-app-tls
  rules:
  - host: app.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: web-app-service
            port:
              number: 80
```

**Best Practices:**
- Use namespaces for isolation
- Implement resource quotas and limits
- Use ConfigMaps and Secrets appropriately
- Enable RBAC
- Use Pod Security Standards
- Implement network policies
- Use Helm for package management
- Set up proper monitoring and logging

### Container Registries

**Options:**
- **Docker Hub**: Public and private registries
- **Amazon ECR**: AWS container registry
- **Google Artifact Registry**: GCP registry
- **Azure Container Registry**: Azure registry
- **Harbor**: Self-hosted enterprise registry
- **GitHub Container Registry**: Integrated with GitHub

**Best Practices:**
- Scan images for vulnerabilities
- Implement image signing
- Use private registries for proprietary code
- Set up automated cleanup policies
- Tag images semantically (semver)
- Use content trust

## Infrastructure as Code (IaC)

### Terraform

**Why We Use It:**
- Cloud-agnostic
- Declarative syntax
- Large provider ecosystem
- State management
- Strong community

**Project Structure:**
```
terraform/
├── modules/
│   ├── networking/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   └── outputs.tf
│   └── compute/
│       ├── main.tf
│       ├── variables.tf
│       └── outputs.tf
├── environments/
│   ├── dev/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   └── terraform.tfvars
│   └── prod/
│       ├── main.tf
│       ├── variables.tf
│       └── terraform.tfvars
└── backend.tf
```

**Example Module:**
```hcl
# modules/webserver/main.tf
variable "instance_count" {
  description = "Number of instances"
  type        = number
  default     = 2
}

variable "instance_type" {
  description = "EC2 instance type"
  type        = string
  default     = "t3.micro"
}

resource "aws_instance" "web" {
  count         = var.instance_count
  ami           = data.aws_ami.ubuntu.id
  instance_type = var.instance_type

  tags = {
    Name = "web-server-${count.index + 1}"
    Environment = var.environment
  }
}

output "instance_ids" {
  value = aws_instance.web[*].id
}
```

**Best Practices:**
- Use remote state (S3, Terraform Cloud)
- Enable state locking
- Use modules for reusability
- Version control everything
- Use workspaces for environments
- Implement automated testing (terratest)
- Plan before apply
- Use variable files for configuration
- Document modules

### AWS CloudFormation

**When to Use:**
- AWS-only infrastructure
- Deep AWS integration needed
- AWS-native tooling preferred

**Stack Example:**
```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: Web application infrastructure

Parameters:
  InstanceType:
    Type: String
    Default: t3.micro
    AllowedValues:
      - t3.micro
      - t3.small
      - t3.medium

Resources:
  WebServerInstance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: !Ref InstanceType
      ImageId: !Ref LatestAmiId
      SecurityGroupIds:
        - !Ref WebServerSecurityGroup
      Tags:
        - Key: Name
          Value: WebServer

  WebServerSecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: Allow HTTP/HTTPS traffic
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: 80
          ToPort: 80
          CidrIp: 0.0.0.0/0

Outputs:
  InstanceId:
    Description: Instance ID
    Value: !Ref WebServerInstance
```

### Pulumi

**Why Consider It:**
- Use familiar programming languages
- Type safety and IDE support
- Native testing capabilities
- Strong abstraction capabilities

**Example (TypeScript):**
```typescript
import * as aws from "@pulumi/aws";

const webServer = new aws.ec2.Instance("web-server", {
    instanceType: "t3.micro",
    ami: "ami-0c55b159cbfafe1f0",
    tags: {
        Name: "web-server",
    },
});

export const publicIp = webServer.publicIp;
```

### Configuration Management

**Ansible:**
- Agentless automation
- YAML-based playbooks
- Strong community modules

**Chef/Puppet:**
- Agent-based management
- Ruby DSL
- Enterprise features

**Trend:** Moving away from configuration management toward immutable infrastructure and containers.

## Databases and Data Stores

### Relational Databases

**PostgreSQL:**
- **Use for**: Complex queries, ACID compliance, JSON support
- **Managed Options**: AWS RDS, Google Cloud SQL, Azure Database
- **Best Practices**: 
  - Use connection pooling (PgBouncer)
  - Enable query logging for slow queries
  - Regular backups and point-in-time recovery
  - Use read replicas for scaling reads

**MySQL/MariaDB:**
- **Use for**: Web applications, simple queries
- **Managed Options**: AWS RDS, Google Cloud SQL
- **Best Practices**:
  - Enable binary logging
  - Use InnoDB engine
  - Implement proper indexing

### NoSQL Databases

**MongoDB:**
- **Use for**: Document storage, flexible schemas
- **Managed Options**: MongoDB Atlas, AWS DocumentDB
- **Best Practices**:
  - Design schema for query patterns
  - Use indexes effectively
  - Implement sharding for scale

**Redis:**
- **Use for**: Caching, sessions, real-time analytics
- **Managed Options**: AWS ElastiCache, Redis Enterprise
- **Best Practices**:
  - Set eviction policies
  - Use persistence appropriately
  - Monitor memory usage
  - Use Redis Cluster for high availability

**DynamoDB:**
- **Use for**: Serverless applications, high-scale key-value
- **Best Practices**:
  - Design partition keys carefully
  - Use on-demand billing for unpredictable workloads
  - Implement DynamoDB Streams for change capture

### Time-Series Databases

**InfluxDB:**
- Metrics and monitoring data
- Built-in downsampling
- SQL-like query language

**TimescaleDB:**
- PostgreSQL extension
- Full SQL support
- Familiar tooling

### Search Engines

**Elasticsearch:**
- Full-text search
- Log analytics
- Real-time indexing

**Best Practices:**
- Size clusters appropriately
- Use index lifecycle management
- Monitor cluster health
- Implement proper security

## Networking and CDN

### Content Delivery Networks

**CloudFlare:**
- Global CDN
- DDoS protection
- Edge computing (Workers)
- Free tier available

**AWS CloudFront:**
- Integration with AWS services
- Origin shield
- Lambda@Edge

**Fastly:**
- Real-time purging
- VCL configuration
- Edge computing

**Best Practices:**
- Cache static assets aggressively
- Use appropriate TTLs
- Implement cache invalidation strategy
- Enable compression
- Use HTTP/2 and HTTP/3
- Set up proper security headers

### Load Balancers

**Application Load Balancer (AWS):**
- HTTP/HTTPS traffic
- Host and path-based routing
- WebSocket support
- Container and microservices friendly

**Network Load Balancer (AWS):**
- TCP/UDP traffic
- Ultra-low latency
- Static IP addresses
- Millions of requests per second

**Best Practices:**
- Enable health checks
- Use multiple availability zones
- Configure proper timeouts
- Enable access logs
- Use SSL/TLS termination

### DNS

**Route 53 (AWS):**
- Highly available DNS
- Health checks and failover
- Traffic flow policies
- Domain registration

**Cloud DNS (GCP):**
- Global anycast network
- Integration with GCP services
- DNSSEC support

**Best Practices:**
- Use short TTLs for critical records
- Implement health checks
- Use geolocation routing when appropriate
- Enable DNSSEC
- Monitor DNS query patterns

### Service Mesh

**Istio:**
- Traffic management
- Security (mTLS)
- Observability
- Kubernetes-native

**Linkerd:**
- Lightweight
- Simpler than Istio
- Fast and secure

**When to Use:**
- Microservices architecture
- Need for advanced traffic management
- Service-to-service security requirements
- Enhanced observability needs

## Message Queues and Event Streaming

### Message Queues

**RabbitMQ:**
- Traditional message broker
- Multiple protocols (AMQP, MQTT, STOMP)
- Flexible routing
- Good for task queues

**Amazon SQS:**
- Fully managed
- Simple to use
- Integrates with AWS services
- Standard and FIFO queues

**Best Practices:**
- Handle message failures gracefully
- Implement dead letter queues
- Monitor queue depth
- Set appropriate visibility timeouts
- Use batching for efficiency

### Event Streaming

**Apache Kafka:**
- High-throughput event streaming
- Durable storage
- Stream processing
- Strong ecosystem

**Managed Options:**
- Confluent Cloud
- AWS MSK (Managed Streaming for Kafka)
- Azure Event Hubs

**Best Practices:**
- Design topic partitioning carefully
- Monitor consumer lag
- Set appropriate retention policies
- Use schema registry (Avro, Protobuf)
- Implement proper error handling

**AWS Kinesis:**
- Real-time data streaming
- Integration with AWS services
- Auto-scaling
- Pay per use

## Secrets Management

**HashiCorp Vault:**
- Centralized secrets storage
- Dynamic secrets
- Encryption as a service
- Access control and audit logging

**AWS Secrets Manager:**
- Automatic rotation
- Fine-grained IAM permissions
- Integration with RDS
- Audit with CloudTrail

**Best Practices:**
- Never commit secrets to version control
- Rotate secrets regularly
- Use different secrets per environment
- Implement least privilege access
- Enable audit logging
- Encrypt secrets at rest and in transit

## Backup and Disaster Recovery

**Strategies:**

**3-2-1 Rule:**
- 3 copies of data
- 2 different media types
- 1 off-site backup

**Automated Backups:**
- Database automated backups (RDS, DynamoDB)
- EBS snapshots
- S3 versioning and replication
- Infrastructure as Code in version control

**Disaster Recovery Plans:**
- Define RTO (Recovery Time Objective)
- Define RPO (Recovery Point Objective)
- Regular testing of recovery procedures
- Document runbooks
- Implement multi-region architecture for critical systems

**Tools:**
- **Velero**: Kubernetes backup and restore
- **AWS Backup**: Centralized backup management
- **Commvault**: Enterprise backup solution

## Cost Optimization

**Strategies:**

1. **Right-sizing**: Match resources to actual needs
2. **Reserved Instances**: Commit to long-term usage
3. **Spot Instances**: Use for fault-tolerant workloads
4. **Auto-scaling**: Scale resources with demand
5. **Storage Tiering**: Move data to appropriate storage classes
6. **Resource Cleanup**: Delete unused resources
7. **Monitoring**: Track costs and set alerts

**Tools:**
- **AWS Cost Explorer**: Visualize and analyze costs
- **CloudHealth**: Multi-cloud cost management
- **Kubecost**: Kubernetes cost monitoring
- **Infracost**: Estimate Terraform costs

---

**Related:**
- [Development Tools](dev-tools.md) - Local development setup
- [Monitoring](monitoring.md) - Infrastructure observability
- [Documentation](documentation.md) - Infrastructure documentation
