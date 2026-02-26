# CI/CD Pipeline

## Overview

Continuous Integration and Continuous Deployment (CI/CD) automates the process of building, testing, and deploying software. This guide covers pipeline design, best practices, and implementation strategies for reliable automated delivery.

## Table of Contents

- [CI/CD Fundamentals](#cicd-fundamentals)
- [Pipeline Architecture](#pipeline-architecture)
- [Build Automation](#build-automation)
- [Automated Testing](#automated-testing)
- [Deployment Strategies](#deployment-strategies)
- [Environment Management](#environment-management)
- [Security and Compliance](#security-and-compliance)
- [Monitoring and Observability](#monitoring-and-observability)
- [Best Practices](#best-practices)
- [Tools and Platforms](#tools-and-platforms)

## CI/CD Fundamentals

### Continuous Integration (CI)

**Definition**: Automatically building and testing code changes as they're committed.

**Key Principles**:
```markdown
✓ Commit code frequently (at least daily)
✓ Build every commit automatically
✓ Test every build
✓ Fix broken builds immediately
✓ Keep builds fast (< 10 minutes)
✓ Test in production-like environment
✓ Make it easy to get deliverables
✓ Everyone can see build results
```

### Continuous Deployment (CD)

**Definition**: Automatically deploying every change that passes tests to production.

**CD vs Continuous Delivery**:
```
Continuous Delivery:  Code → Build → Test → Stage → [Manual Approval] → Production
Continuous Deployment: Code → Build → Test → Stage → Production (automated)
```

### Benefits

**Speed**:
- Faster time to market
- Rapid feedback loops
- Quick bug fixes

**Quality**:
- Early bug detection
- Consistent builds
- Automated testing

**Reliability**:
- Repeatable processes
- Reduced human error
- Reliable deployments

**Collaboration**:
- Shared responsibility
- Transparent processes
- Better communication

## Pipeline Architecture

### Basic Pipeline Structure

```yaml
┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
│  Source  │ -> │  Build   │ -> │   Test   │ -> │  Stage   │ -> │  Deploy  │
│  Control │    │          │    │          │    │          │    │          │
└──────────┘    └──────────┘    └──────────┘    └──────────┘    └──────────┘
     │               │               │               │               │
     │               │               │               │               │
     v               v               v               v               v
   Git Push     Compile Code    Run Tests      Deploy to       Deploy to
                Package App     Security        Staging       Production
                                Scan
```

### Multi-Stage Pipeline Example

```yaml
# .github/workflows/ci-cd.yml
name: CI/CD Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main, develop]

env:
  NODE_VERSION: '18.x'
  REGISTRY: ghcr.io

jobs:
  # Stage 1: Code Quality
  code-quality:
    name: Code Quality Checks
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'npm'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Run linter
        run: npm run lint
      
      - name: Check formatting
        run: npm run format:check
      
      - name: Type check
        run: npm run type-check

  # Stage 2: Build
  build:
    name: Build Application
    runs-on: ubuntu-latest
    needs: code-quality
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'npm'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Build application
        run: npm run build
      
      - name: Upload build artifacts
        uses: actions/upload-artifact@v3
        with:
          name: build-artifacts
          path: dist/
          retention-days: 7

  # Stage 3: Test
  test:
    name: Run Tests
    runs-on: ubuntu-latest
    needs: build
    strategy:
      matrix:
        test-suite: [unit, integration, e2e]
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: ${{ env.NODE_VERSION }}
      
      - name: Install dependencies
        run: npm ci
      
      - name: Download build artifacts
        uses: actions/download-artifact@v3
        with:
          name: build-artifacts
          path: dist/
      
      - name: Run ${{ matrix.test-suite }} tests
        run: npm run test:${{ matrix.test-suite }}
      
      - name: Upload coverage
        if: matrix.test-suite == 'unit'
        uses: codecov/codecov-action@v3
        with:
          files: ./coverage/lcov.info
          flags: ${{ matrix.test-suite }}

  # Stage 4: Security Scan
  security:
    name: Security Scanning
    runs-on: ubuntu-latest
    needs: build
    steps:
      - uses: actions/checkout@v3
      
      - name: Run dependency audit
        run: npm audit --audit-level=moderate
      
      - name: Run Snyk security scan
        uses: snyk/actions/node@master
        env:
          SNYK_TOKEN: ${{ secrets.SNYK_TOKEN }}
      
      - name: Run SAST scan
        uses: github/codeql-action/analyze@v2

  # Stage 5: Build Docker Image
  docker:
    name: Build and Push Docker Image
    runs-on: ubuntu-latest
    needs: [test, security]
    if: github.event_name == 'push'
    outputs:
      image-tag: ${{ steps.meta.outputs.tags }}
    steps:
      - uses: actions/checkout@v3
      
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v2
      
      - name: Log in to Container Registry
        uses: docker/login-action@v2
        with:
          registry: ${{ env.REGISTRY }}
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}
      
      - name: Extract metadata
        id: meta
        uses: docker/metadata-action@v4
        with:
          images: ${{ env.REGISTRY }}/${{ github.repository }}
          tags: |
            type=ref,event=branch
            type=sha,prefix={{branch}}-
            type=semver,pattern={{version}}
      
      - name: Build and push
        uses: docker/build-push-action@v4
        with:
          context: .
          push: true
          tags: ${{ steps.meta.outputs.tags }}
          cache-from: type=gha
          cache-to: type=gha,mode=max

  # Stage 6: Deploy to Staging
  deploy-staging:
    name: Deploy to Staging
    runs-on: ubuntu-latest
    needs: docker
    if: github.ref == 'refs/heads/develop'
    environment:
      name: staging
      url: https://staging.example.com
    steps:
      - name: Deploy to staging
        uses: azure/webapps-deploy@v2
        with:
          app-name: myapp-staging
          images: ${{ needs.docker.outputs.image-tag }}
      
      - name: Run smoke tests
        run: |
          curl -f https://staging.example.com/health || exit 1

  # Stage 7: Deploy to Production
  deploy-production:
    name: Deploy to Production
    runs-on: ubuntu-latest
    needs: [docker, deploy-staging]
    if: github.ref == 'refs/heads/main'
    environment:
      name: production
      url: https://example.com
    steps:
      - name: Deploy to production
        uses: azure/webapps-deploy@v2
        with:
          app-name: myapp-production
          images: ${{ needs.docker.outputs.image-tag }}
      
      - name: Run smoke tests
        run: |
          curl -f https://example.com/health || exit 1
      
      - name: Notify deployment
        uses: 8398a7/action-slack@v3
        with:
          status: ${{ job.status }}
          text: 'Production deployment completed'
          webhook_url: ${{ secrets.SLACK_WEBHOOK }}
```

### Pipeline Stages Explained

**1. Code Quality** (1-2 min):
- Linting
- Code formatting
- Type checking
- Fast feedback

**2. Build** (2-5 min):
- Compile code
- Bundle assets
- Create artifacts
- Version tagging

**3. Test** (5-10 min):
- Unit tests
- Integration tests
- E2E tests (parallel)
- Coverage reporting

**4. Security** (2-5 min):
- Dependency scanning
- SAST analysis
- Container scanning
- License compliance

**5. Package** (2-3 min):
- Docker image build
- Artifact upload
- Registry push
- Image signing

**6. Deploy** (5-10 min):
- Infrastructure provisioning
- Application deployment
- Configuration management
- Smoke testing

## Build Automation

### Build Configuration

#### Node.js/JavaScript

```json
// package.json
{
  "scripts": {
    "prebuild": "npm run clean && npm run lint",
    "build": "webpack --mode production",
    "build:dev": "webpack --mode development",
    "build:analyze": "webpack-bundle-analyzer dist/stats.json",
    "clean": "rimraf dist coverage",
    "lint": "eslint src --ext .js,.jsx,.ts,.tsx",
    "format": "prettier --write 'src/**/*.{js,jsx,ts,tsx,json,css,md}'",
    "type-check": "tsc --noEmit"
  }
}
```

#### Docker Build

```dockerfile
# Dockerfile
# Stage 1: Build
FROM node:18-alpine AS builder

WORKDIR /app

# Copy dependency files
COPY package*.json ./

# Install dependencies
RUN npm ci --only=production

# Copy source code
COPY . .

# Build application
RUN npm run build

# Stage 2: Production
FROM node:18-alpine

WORKDIR /app

# Copy from build stage
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/package.json ./

# Set user
USER node

# Expose port
EXPOSE 3000

# Health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD node healthcheck.js

# Start application
CMD ["node", "dist/server.js"]
```

#### Multi-Platform Builds

```yaml
# .github/workflows/docker-multiplatform.yml
- name: Build multi-platform image
  uses: docker/build-push-action@v4
  with:
    context: .
    platforms: linux/amd64,linux/arm64
    push: true
    tags: |
      myregistry/myapp:latest
      myregistry/myapp:${{ github.sha }}
    cache-from: type=registry,ref=myregistry/myapp:buildcache
    cache-to: type=registry,ref=myregistry/myapp:buildcache,mode=max
```

### Build Optimization

**Caching Strategies**:

```yaml
# Dependency caching
- name: Cache dependencies
  uses: actions/cache@v3
  with:
    path: ~/.npm
    key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
    restore-keys: |
      ${{ runner.os }}-node-

# Build caching
- name: Cache build
  uses: actions/cache@v3
  with:
    path: dist
    key: ${{ runner.os }}-build-${{ github.sha }}
```

**Parallel Builds**:

```yaml
strategy:
  matrix:
    platform: [linux/amd64, linux/arm64]
    node-version: [16.x, 18.x, 20.x]
```

## Automated Testing

### Test Execution in Pipeline

```yaml
# Parallel test execution
test:
  strategy:
    matrix:
      test-type: [unit, integration, e2e]
      shard: [1, 2, 3, 4]
  steps:
    - name: Run tests
      run: npm run test:${{ matrix.test-type }} -- --shard=${{ matrix.shard }}/4
```

### Test Reporting

```yaml
- name: Publish test results
  uses: dorny/test-reporter@v1
  if: always()
  with:
    name: Test Results (${{ matrix.test-type }})
    path: test-results/*.xml
    reporter: jest-junit

- name: Upload test artifacts
  if: failure()
  uses: actions/upload-artifact@v3
  with:
    name: test-failures
    path: |
      test-results/
      screenshots/
      videos/
```

### Performance Testing

```yaml
performance-test:
  runs-on: ubuntu-latest
  steps:
    - name: Run load tests
      run: |
        artillery run load-test.yml --output report.json
    
    - name: Check performance budgets
      run: |
        node scripts/check-performance-budget.js report.json
    
    - name: Upload performance report
      uses: actions/upload-artifact@v3
      with:
        name: performance-report
        path: report.json
```

## Deployment Strategies

### Blue-Green Deployment

Deploy new version alongside old version, then switch traffic:

```yaml
# blue-green-deployment.yml
name: Blue-Green Deployment

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to green environment
        run: |
          kubectl apply -f k8s/deployment-green.yaml
          kubectl wait --for=condition=ready pod -l version=green
      
      - name: Run smoke tests on green
        run: |
          ./scripts/smoke-test.sh green.example.com
      
      - name: Switch traffic to green
        run: |
          kubectl patch service myapp -p '{"spec":{"selector":{"version":"green"}}}'
      
      - name: Monitor for 5 minutes
        run: |
          sleep 300
          ./scripts/check-metrics.sh
      
      - name: Decommission blue environment
        if: success()
        run: |
          kubectl delete deployment myapp-blue
```

**Blue-Green Kubernetes Manifests**:

```yaml
# deployment-green.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-green
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
      version: green
  template:
    metadata:
      labels:
        app: myapp
        version: green
    spec:
      containers:
      - name: myapp
        image: myregistry/myapp:v2.0.0
        ports:
        - containerPort: 3000

---
apiVersion: v1
kind: Service
metadata:
  name: myapp
spec:
  selector:
    app: myapp
    version: green  # Switch between blue/green
  ports:
  - port: 80
    targetPort: 3000
```

### Canary Deployment

Gradually roll out to subset of users:

```yaml
# canary-deployment.yml
- name: Deploy canary (10%)
  run: |
    kubectl apply -f k8s/deployment-canary.yaml
    kubectl patch service myapp -p '{"spec":{"selector":{"version":"canary"}}}'
    kubectl scale deployment myapp-canary --replicas=1
    kubectl scale deployment myapp-stable --replicas=9

- name: Monitor canary metrics
  run: |
    sleep 600  # Monitor for 10 minutes
    ./scripts/check-canary-metrics.sh

- name: Increase to 50%
  if: success()
  run: |
    kubectl scale deployment myapp-canary --replicas=5
    kubectl scale deployment myapp-stable --replicas=5

- name: Full rollout
  if: success()
  run: |
    kubectl scale deployment myapp-canary --replicas=10
    kubectl scale deployment myapp-stable --replicas=0
```

**Progressive Delivery with Flagger**:

```yaml
# flagger-canary.yaml
apiVersion: flagger.app/v1beta1
kind: Canary
metadata:
  name: myapp
spec:
  targetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: myapp
  service:
    port: 80
  analysis:
    interval: 1m
    threshold: 5
    maxWeight: 50
    stepWeight: 10
    metrics:
    - name: request-success-rate
      thresholdRange:
        min: 99
    - name: request-duration
      thresholdRange:
        max: 500
  webhooks:
    - name: load-test
      url: http://flagger-loadtester/
```

### Rolling Deployment

Gradual replacement of instances:

```yaml
# k8s/deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
spec:
  replicas: 10
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 2        # Max 2 extra pods during update
      maxUnavailable: 1  # Max 1 pod can be unavailable
  template:
    spec:
      containers:
      - name: myapp
        image: myregistry/myapp:v2.0.0
        readinessProbe:
          httpGet:
            path: /health
            port: 3000
          initialDelaySeconds: 5
          periodSeconds: 5
        livenessProbe:
          httpGet:
            path: /health
            port: 3000
          initialDelaySeconds: 15
          periodSeconds: 10
```

### Feature Flags

Decouple deployment from release:

```javascript
// Feature flag implementation
import { FeatureFlags } from './featureFlags';

function renderCheckout() {
  if (FeatureFlags.isEnabled('new-checkout-flow')) {
    return <NewCheckoutFlow />;
  }
  return <LegacyCheckoutFlow />;
}

// Gradual rollout
FeatureFlags.enable('new-checkout-flow', {
  percentage: 10,  // 10% of users
  userIds: ['beta-tester-1', 'beta-tester-2'],
  regions: ['us-east']
});
```

## Environment Management

### Environment Configuration

```yaml
# environments/
├── dev.env
├── staging.env
└── production.env
```

**Example Environment File**:

```bash
# staging.env
NODE_ENV=staging
API_URL=https://api-staging.example.com
DATABASE_URL=postgresql://user:pass@staging-db:5432/myapp
REDIS_URL=redis://staging-redis:6379
LOG_LEVEL=debug
FEATURE_FLAGS_ENDPOINT=https://flags-staging.example.com
```

### Secret Management

```yaml
# Using GitHub Secrets
- name: Deploy with secrets
  env:
    DATABASE_PASSWORD: ${{ secrets.DB_PASSWORD }}
    API_KEY: ${{ secrets.API_KEY }}
  run: |
    ./deploy.sh

# Using HashiCorp Vault
- name: Import secrets from Vault
  uses: hashicorp/vault-action@v2
  with:
    url: https://vault.example.com
    token: ${{ secrets.VAULT_TOKEN }}
    secrets: |
      secret/data/production/db password | DATABASE_PASSWORD
      secret/data/production/api key | API_KEY
```

### Infrastructure as Code

**Terraform Example**:

```hcl
# terraform/environments/staging/main.tf
module "application" {
  source = "../../modules/application"
  
  environment = "staging"
  app_name    = "myapp"
  
  instance_count = 2
  instance_type  = "t3.medium"
  
  database_config = {
    instance_class = "db.t3.small"
    storage_gb     = 20
  }
  
  auto_scaling = {
    min_size = 2
    max_size = 10
    target_cpu = 70
  }
}
```

**Deployment with Terraform**:

```yaml
- name: Terraform Init
  run: terraform init

- name: Terraform Plan
  run: terraform plan -out=tfplan

- name: Terraform Apply
  if: github.ref == 'refs/heads/main'
  run: terraform apply -auto-approve tfplan
```

## Security and Compliance

### Security Scanning

```yaml
security:
  runs-on: ubuntu-latest
  steps:
    # Dependency scanning
    - name: Run npm audit
      run: npm audit --audit-level=high
    
    # SAST (Static Application Security Testing)
    - name: Run CodeQL analysis
      uses: github/codeql-action/analyze@v2
    
    # Container scanning
    - name: Scan Docker image
      uses: aquasecurity/trivy-action@master
      with:
        image-ref: myregistry/myapp:${{ github.sha }}
        severity: 'CRITICAL,HIGH'
    
    # Secret scanning
    - name: Run TruffleHog
      uses: trufflesecurity/trufflehog@main
      with:
        path: ./
        base: ${{ github.event.repository.default_branch }}
```

### Compliance Checks

```yaml
compliance:
  runs-on: ubuntu-latest
  steps:
    - name: License compliance
      run: |
        npm install -g license-checker
        license-checker --production --failOn 'GPL;LGPL'
    
    - name: SBOM generation
      uses: anchore/sbom-action@v0
      with:
        format: cyclonedx-json
        artifact-name: sbom.json
    
    - name: Policy compliance
      uses: open-policy-agent/conftest-action@v1
      with:
        files: k8s/*.yaml
        policy: policy/
```

## Monitoring and Observability

### Health Checks

```javascript
// healthcheck.js
const http = require('http');

const options = {
  host: 'localhost',
  port: 3000,
  path: '/health',
  timeout: 2000
};

const request = http.request(options, (res) => {
  if (res.statusCode === 200) {
    process.exit(0);
  } else {
    process.exit(1);
  }
});

request.on('error', () => {
  process.exit(1);
});

request.end();
```

### Deployment Metrics

```yaml
- name: Send deployment metrics
  run: |
    curl -X POST https://metrics.example.com/api/deployments \
      -H "Content-Type: application/json" \
      -d '{
        "service": "myapp",
        "version": "${{ github.sha }}",
        "environment": "production",
        "timestamp": "'$(date -u +%Y-%m-%dT%H:%M:%SZ)'",
        "deployer": "${{ github.actor }}"
      }'
```

### Log Aggregation

```yaml
- name: Configure logging
  run: |
    kubectl apply -f - <<EOF
    apiVersion: v1
    kind: ConfigMap
    metadata:
      name: fluent-bit-config
    data:
      fluent-bit.conf: |
        [OUTPUT]
            Name es
            Match *
            Host elasticsearch.logging.svc
            Port 9200
            Index myapp-logs
    EOF
```

## Best Practices

### Pipeline Design

✅ **Keep pipelines fast** (< 10 minutes for feedback)  
✅ **Fail fast** (run quick checks first)  
✅ **Parallelize** independent jobs  
✅ **Cache dependencies** to speed up builds  
✅ **Use matrix builds** for multiple configurations  
✅ **Separate staging from production** pipelines  

### Security

✅ **Never commit secrets** to source control  
✅ **Use secret management** (Vault, AWS Secrets Manager)  
✅ **Scan dependencies** for vulnerabilities  
✅ **Sign containers** and artifacts  
✅ **Implement least privilege** access  
✅ **Audit all deployments**  

### Reliability

✅ **Implement health checks** for all services  
✅ **Use smoke tests** after deployment  
✅ **Enable rollback mechanisms**  
✅ **Monitor deployment metrics**  
✅ **Set up alerts** for failures  
✅ **Practice disaster recovery**  

### Developer Experience

✅ **Provide clear feedback** on failures  
✅ **Make logs easily accessible**  
✅ **Document pipeline configuration**  
✅ **Allow local pipeline testing**  
✅ **Minimize pipeline maintenance**  

## Tools and Platforms

### CI/CD Platforms

| Platform | Best For | Strengths |
|----------|----------|-----------|
| GitHub Actions | GitHub repos | Native integration, free tier |
| GitLab CI/CD | GitLab repos | Built-in, powerful features |
| Jenkins | Enterprise | Highly customizable, plugins |
| CircleCI | Cloud-native | Fast, great caching |
| Azure DevOps | Microsoft stack | Comprehensive tooling |
| AWS CodePipeline | AWS workloads | Native AWS integration |

### Container Registries

- **Docker Hub**: Public images, free tier
- **GitHub Container Registry**: GitHub integration
- **Amazon ECR**: AWS integration
- **Google GCR**: GCP integration
- **Azure ACR**: Azure integration
- **Harbor**: Self-hosted, enterprise features

### Deployment Tools

- **Kubernetes**: Container orchestration
- **Helm**: Kubernetes package manager
- **ArgoCD**: GitOps continuous delivery
- **Flux**: GitOps toolkit
- **Spinnaker**: Multi-cloud deployment

## Conclusion

An effective CI/CD pipeline:

- Automates repetitive tasks
- Provides fast feedback
- Ensures consistent quality
- Enables rapid deployment
- Reduces human error
- Improves team velocity

Remember: **CI/CD is a journey, not a destination**. Continuously improve your pipelines based on team feedback and evolving needs.
