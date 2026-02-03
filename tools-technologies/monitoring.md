# Monitoring & Observability

## Overview

Observability is the ability to understand the internal state of a system by examining its outputs. Modern monitoring goes beyond simple uptime checks to provide deep insights into application behavior, performance, and user experience.

## The Three Pillars of Observability

### 1. Metrics
Numerical measurements over time (CPU usage, request rate, error rate)

### 2. Logs
Discrete events with contextual information

### 3. Traces
Request flow through distributed systems

**Golden Signals:**
- **Latency**: Time to service requests
- **Traffic**: Demand on the system
- **Errors**: Rate of failed requests
- **Saturation**: Resource utilization

## Application Performance Monitoring (APM)

### New Relic

**Features:**
- Full-stack visibility
- Distributed tracing
- Custom dashboards
- AI-powered insights (New Relic AI)
- Mobile app monitoring

**When to Use:**
- Need comprehensive APM solution
- Multi-language support required
- Want powerful analytics
- Need mobile monitoring

**Setup Example (Node.js):**
```javascript
// newrelic.js
'use strict'

exports.config = {
  app_name: ['My Application'],
  license_key: 'your-license-key',
  logging: {
    level: 'info'
  },
  distributed_tracing: {
    enabled: true
  },
  transaction_tracer: {
    enabled: true,
    transaction_threshold: 'apdex_f',
    record_sql: 'obfuscated'
  }
}

// Load at application start
require('newrelic');
```

**Best Practices:**
- Set meaningful transaction names
- Use custom attributes for business context
- Configure error grouping
- Set up SLA alerts
- Use deployment markers

### Datadog

**Features:**
- Infrastructure monitoring
- APM and distributed tracing
- Log management
- Synthetics (synthetic monitoring)
- Security monitoring

**When to Use:**
- Need unified platform
- Heavy infrastructure monitoring
- Want out-of-box integrations
- Need security monitoring

**Setup Example (Python):**
```python
from ddtrace import tracer, patch_all

# Automatically trace libraries
patch_all()

# Custom instrumentation
@tracer.wrap(service='web-app', resource='checkout')
def checkout(user_id, cart):
    span = tracer.current_span()
    span.set_tag('user.id', user_id)
    span.set_tag('cart.total', cart.total)
    # Business logic
```

**Best Practices:**
- Use unified service tagging
- Implement custom metrics for business KPIs
- Set up composite monitors
- Use trace sampling intelligently
- Configure log pipelines

### Dynatrace

**Features:**
- AI-powered root cause analysis
- Automatic discovery and dependency mapping
- Real user monitoring (RUM)
- Deep code-level insights
- Automated baselining

**When to Use:**
- Enterprise-scale monitoring
- Need AI-driven insights
- Complex microservices architecture
- Want automatic problem detection

### OpenTelemetry

**Modern Standard for Observability**

**Why We Use It:**
- Vendor-neutral
- Single instrumentation, multiple backends
- Active community development
- CNCF standard

**Architecture:**
```
Application → OpenTelemetry SDK → OpenTelemetry Collector → Backend (Jaeger, Prometheus, etc.)
```

**Example (Node.js):**
```javascript
const { NodeTracerProvider } = require('@opentelemetry/sdk-trace-node');
const { registerInstrumentations } = require('@opentelemetry/instrumentation');
const { HttpInstrumentation } = require('@opentelemetry/instrumentation-http');
const { ExpressInstrumentation } = require('@opentelemetry/instrumentation-express');

const provider = new NodeTracerProvider();
provider.register();

registerInstrumentations({
  instrumentations: [
    new HttpInstrumentation(),
    new ExpressInstrumentation(),
  ],
});
```

**Best Practices:**
- Use semantic conventions
- Add context-rich attributes
- Implement proper sampling
- Export to multiple backends
- Version your schemas

### Prometheus + Grafana

**Prometheus (Metrics Collection):**

**When to Use:**
- Kubernetes environments
- Need powerful query language
- Want open-source solution
- Time-series metrics focus

**Example Metrics (Node.js):**
```javascript
const client = require('prom-client');

// Create a Registry
const register = new client.Registry();

// Add default metrics
client.collectDefaultMetrics({ register });

// Custom counter
const httpRequestsTotal = new client.Counter({
  name: 'http_requests_total',
  help: 'Total HTTP requests',
  labelNames: ['method', 'route', 'status_code'],
  registers: [register]
});

// Custom histogram
const httpRequestDuration = new client.Histogram({
  name: 'http_request_duration_seconds',
  help: 'HTTP request duration',
  labelNames: ['method', 'route'],
  registers: [register]
});

// Expose metrics endpoint
app.get('/metrics', async (req, res) => {
  res.set('Content-Type', register.contentType);
  res.end(await register.metrics());
});
```

**Grafana (Visualization):**

**Features:**
- Beautiful dashboards
- Multiple data sources
- Alerting capabilities
- Template variables
- Plugin ecosystem

**Dashboard Best Practices:**
- Use consistent naming conventions
- Implement drill-down capabilities
- Set up variable templates
- Use appropriate visualization types
- Document dashboard purpose

### Application Insights (Azure)

**When to Use:**
- Azure-native applications
- .NET stack
- Integration with Azure services
- Need for Application Map

**Features:**
- Automatic dependency tracking
- Live metrics
- Application Map
- Availability tests
- Smart detection

## Log Aggregation and Analysis

### ELK Stack (Elasticsearch, Logstash, Kibana)

**Elasticsearch**: Search and analytics engine
**Logstash**: Data processing pipeline
**Kibana**: Visualization and exploration

**Architecture:**
```
Application → Filebeat/Logstash → Elasticsearch → Kibana
```

**Logstash Configuration:**
```ruby
input {
  beats {
    port => 5044
  }
}

filter {
  if [type] == "nginx" {
    grok {
      match => { "message" => "%{COMBINEDAPACHELOG}" }
    }
    date {
      match => [ "timestamp", "dd/MMM/yyyy:HH:mm:ss Z" ]
    }
    geoip {
      source => "clientip"
    }
  }
}

output {
  elasticsearch {
    hosts => ["localhost:9200"]
    index => "logs-%{+YYYY.MM.dd}"
  }
}
```

**Best Practices:**
- Use index lifecycle management
- Implement log rotation
- Set up index templates
- Use Kibana Spaces for multi-tenancy
- Monitor cluster health
- Implement proper security

### Splunk

**When to Use:**
- Enterprise log management
- Need powerful search (SPL)
- Compliance and security requirements
- Machine learning capabilities

**Features:**
- Real-time search and analysis
- Alerting and dashboards
- Machine learning toolkit
- Security operations (SIEM)
- IT Service Intelligence

**Search Processing Language (SPL):**
```spl
index=web_logs status>=400
| stats count by status, url
| where count > 100
| sort -count
```

**Best Practices:**
- Use summary indexing for expensive searches
- Implement data models
- Set up scheduled reports
- Use field extractions
- Monitor license usage

### CloudWatch Logs (AWS)

**When to Use:**
- AWS-native applications
- Simple log aggregation needs
- Integration with other AWS services

**Features:**
- Log groups and streams
- Metric filters
- Insights queries
- Subscription filters
- Cross-account logging

**CloudWatch Insights Query:**
```sql
fields @timestamp, @message
| filter @message like /ERROR/
| stats count(*) by bin(5m)
| sort @timestamp desc
```

**Best Practices:**
- Set retention policies
- Use log groups per application
- Create metric filters for important patterns
- Export to S3 for long-term storage
- Use Lambda for custom processing

### Loki (Grafana)

**Why Consider It:**
- Designed for Kubernetes
- Cost-effective (indexes labels, not content)
- Native Grafana integration
- Lightweight compared to ELK

**LogQL Query:**
```logql
{app="web-server", env="production"} 
  |= "error" 
  | json 
  | line_format "{{.level}}: {{.message}}"
```

## Error Tracking

### Sentry

**Features:**
- Real-time error tracking
- Release tracking
- Performance monitoring
- Breadcrumbs for debugging
- Issue assignment and workflows

**Setup Example (JavaScript):**
```javascript
import * as Sentry from "@sentry/browser";

Sentry.init({
  dsn: "your-dsn-here",
  environment: process.env.NODE_ENV,
  release: "my-app@" + process.env.npm_package_version,
  
  // Performance monitoring
  tracesSampleRate: 0.1,
  
  // Session replay
  replaysSessionSampleRate: 0.1,
  replaysOnErrorSampleRate: 1.0,
  
  beforeSend(event, hint) {
    // Filter or modify events
    if (event.exception) {
      // Add custom logic
    }
    return event;
  }
});

// Add user context
Sentry.setUser({
  id: user.id,
  email: user.email,
  username: user.username
});

// Add custom context
Sentry.setContext("character", {
  name: "Mighty Fighter",
  level: 19
});
```

**Best Practices:**
- Configure source maps for JavaScript
- Use breadcrumbs effectively
- Set up release tracking
- Configure issue ownership
- Use custom fingerprinting
- Filter noise (PII, known errors)
- Set up integrations (Slack, Jira)

### Rollbar

**Features:**
- Error grouping
- Deploy tracking
- People tracking
- RQL (Rollbar Query Language)
- Telemetry for debugging

**When to Use:**
- Need simple error tracking
- Want good grouping algorithms
- Need person tracking across errors

### Bugsnag

**Features:**
- Error monitoring
- Stability scoring
- Release stages
- Breadcrumbs
- Session tracking

**When to Use:**
- Mobile app error tracking
- Need stability metrics
- Want simple deployment

## Performance Monitoring

### Real User Monitoring (RUM)

**Purpose:** Measure actual user experience

**Tools:**
- **Google Analytics**: Basic page performance
- **New Relic Browser**: Detailed browser monitoring
- **Datadog RUM**: User sessions and interactions
- **Elastic RUM**: JavaScript monitoring

**Key Metrics:**
- **First Contentful Paint (FCP)**: First content rendered
- **Largest Contentful Paint (LCP)**: Main content loaded
- **Time to Interactive (TTI)**: Page becomes interactive
- **Cumulative Layout Shift (CLS)**: Visual stability
- **First Input Delay (FID)**: Interactivity responsiveness

**Web Vitals Implementation:**
```javascript
import {getCLS, getFID, getFCP, getLCP, getTTFB} from 'web-vitals';

function sendToAnalytics(metric) {
  const body = JSON.stringify({
    name: metric.name,
    value: metric.value,
    rating: metric.rating,
    delta: metric.delta,
    id: metric.id,
  });
  
  // Use `navigator.sendBeacon()` if available, falling back to `fetch()`
  if (navigator.sendBeacon) {
    navigator.sendBeacon('/analytics', body);
  } else {
    fetch('/analytics', {body, method: 'POST', keepalive: true});
  }
}

getCLS(sendToAnalytics);
getFID(sendToAnalytics);
getFCP(sendToAnalytics);
getLCP(sendToAnalytics);
getTTFB(sendToAnalytics);
```

### Synthetic Monitoring

**Purpose:** Proactive monitoring from various locations

**Tools:**
- **Pingdom**: Uptime and performance monitoring
- **StatusCake**: Website monitoring
- **Datadog Synthetics**: API and browser tests
- **Checkly**: Programmable monitoring

**Use Cases:**
- API endpoint monitoring
- Multi-step user flows
- Geographic performance testing
- Third-party service monitoring

**Example (Checkly):**
```javascript
const { check } = require('checkly');

check('Home Page Load', {
  frequency: 5, // minutes
  locations: ['us-east-1', 'eu-west-1'],
  script: `
    const browser = await puppeteer.launch();
    const page = await browser.newPage();
    await page.goto('https://example.com');
    
    // Check for specific element
    await page.waitForSelector('h1');
    
    // Measure performance
    const metrics = await page.metrics();
    expect(metrics.TaskDuration).toBeLessThan(3000);
    
    await browser.close();
  `
});
```

### Database Monitoring

**PostgreSQL:**
- **pg_stat_statements**: Query performance
- **pgBadger**: Log analyzer
- **Datadog Database Monitoring**: Query metrics

**MySQL:**
- **Performance Schema**: Performance data
- **Percona Monitoring**: Comprehensive monitoring
- **MySQL Enterprise Monitor**: Official tool

**Key Metrics:**
- Query execution time
- Connection pool usage
- Cache hit ratio
- Lock wait time
- Replication lag

## Alerting and Incident Response

### Alerting Best Practices

**1. Alert on Symptoms, Not Causes:**
```
✅ Good: "Homepage response time > 2s"
❌ Bad: "CPU usage > 80%"
```

**2. Make Alerts Actionable:**
Every alert should have:
- Clear description of the problem
- Impact on users/business
- Suggested remediation steps
- Runbook link

**3. Avoid Alert Fatigue:**
- Set appropriate thresholds
- Use alert suppression during maintenance
- Group related alerts
- Implement alert tuning reviews

**4. Use Alert Severity Levels:**
- **P1/Critical**: Immediate response needed, major user impact
- **P2/High**: Quick response needed, some user impact
- **P3/Medium**: Response within hours, minor impact
- **P4/Low**: Response within days, no immediate impact

### Alert Rules Examples

**Prometheus AlertManager:**
```yaml
groups:
- name: application_alerts
  interval: 30s
  rules:
  - alert: HighErrorRate
    expr: |
      rate(http_requests_total{status=~"5.."}[5m])
      / rate(http_requests_total[5m]) > 0.05
    for: 5m
    labels:
      severity: critical
    annotations:
      summary: "High error rate detected"
      description: "Error rate is {{ $value | humanizePercentage }} on {{ $labels.instance }}"
      runbook_url: "https://wiki.example.com/runbooks/high-error-rate"

  - alert: HighLatency
    expr: |
      histogram_quantile(0.95, 
        rate(http_request_duration_seconds_bucket[5m])
      ) > 2
    for: 10m
    labels:
      severity: warning
    annotations:
      summary: "High request latency"
      description: "95th percentile latency is {{ $value }}s"
```

### Incident Management Tools

**PagerDuty:**
- On-call scheduling
- Incident routing
- Escalation policies
- Post-mortem automation

**Opsgenie:**
- Alert aggregation
- On-call management
- Incident timeline
- Integration with monitoring tools

**VictorOps (Splunk On-Call):**
- Collaborative incident response
- Timeline visualization
- Post-incident reviews

**Best Practices:**
- Define on-call rotations
- Create escalation policies
- Document runbooks
- Conduct post-mortems
- Track MTTR (Mean Time To Resolution)
- Implement blameless culture

### Incident Response Workflow

**1. Detection**
- Automated monitoring alerts
- User reports
- Internal discovery

**2. Triage**
- Assess severity
- Assign responder
- Create incident ticket

**3. Investigation**
- Gather logs and metrics
- Check recent changes
- Form hypothesis

**4. Mitigation**
- Implement fix or workaround
- Verify resolution
- Monitor for recurrence

**5. Resolution**
- Confirm full resolution
- Update stakeholders
- Close incident

**6. Post-Mortem**
- Document timeline
- Identify root cause
- Create action items
- Share learnings

## Dashboard and Visualization Tools

### Grafana

**Why We Use It:**
- Open source
- Multiple data sources
- Rich visualization options
- Alerting capabilities
- Large community

**Dashboard Best Practices:**

**Organization:**
- Use folders for categorization
- Implement naming conventions
- Create dashboard for each service
- Build executive-level summary dashboards

**Design:**
- Put most important metrics at top
- Use consistent colors and units
- Add descriptions to panels
- Use template variables for flexibility
- Implement drill-down links

**Example Dashboard JSON:**
```json
{
  "dashboard": {
    "title": "Application Overview",
    "tags": ["application", "overview"],
    "timezone": "browser",
    "panels": [
      {
        "title": "Request Rate",
        "type": "graph",
        "targets": [
          {
            "expr": "rate(http_requests_total[5m])",
            "legendFormat": "{{method}} {{route}}"
          }
        ]
      }
    ],
    "templating": {
      "list": [
        {
          "name": "environment",
          "type": "query",
          "query": "label_values(up, environment)"
        }
      ]
    }
  }
}
```

### Kibana

**Visualizations:**
- Line/Area charts for time series
- Pie charts for proportions
- Heat maps for correlation
- Data tables for raw data
- Markdown for documentation

**Dashboards:**
- Organize related visualizations
- Add filters and time pickers
- Use saved searches
- Export and share

### Custom Dashboards

**Tools for Building:**
- **Metabase**: Open-source BI
- **Redash**: SQL-based dashboards
- **Superset**: Modern data exploration

**Use Cases:**
- Business metrics dashboards
- Customer analytics
- Operational reporting
- Executive summaries

## Observability as Code

### Infrastructure Monitoring as Code

**Terraform + Datadog:**
```hcl
resource "datadog_monitor" "high_cpu" {
  name    = "High CPU Usage"
  type    = "metric alert"
  message = "CPU usage is above 80% @pagerduty"
  
  query = "avg(last_5m):avg:system.cpu.user{*} by {host} > 80"
  
  thresholds = {
    critical = 80
    warning  = 70
  }
  
  notify_no_data    = true
  no_data_timeframe = 10
  
  tags = ["team:platform", "environment:production"]
}
```

### Log Parsing as Code

**FluentBit Configuration:**
```ini
[INPUT]
    Name              tail
    Path              /var/log/app.log
    Parser            json
    Tag               app.logs

[FILTER]
    Name              parser
    Match             app.*
    Key_Name          log
    Parser            app_parser
    Preserve_Key      On

[OUTPUT]
    Name              es
    Match             *
    Host              elasticsearch
    Port              9200
    Index             app-logs
```

## Best Practices

### Monitoring Strategy

1. **Start with SLOs (Service Level Objectives)**
   - Define SLIs (Service Level Indicators)
   - Set realistic targets
   - Monitor error budget
   - Use SLOs to drive alerting

2. **Implement Progressive Monitoring**
   - Infrastructure metrics
   - Application metrics
   - Business metrics
   - User experience metrics

3. **Correlation and Context**
   - Link logs, metrics, and traces
   - Add business context to technical data
   - Use consistent tagging
   - Maintain service catalog

4. **Continuous Improvement**
   - Review alerts regularly
   - Update runbooks
   - Conduct chaos engineering
   - Learn from incidents

### Data Retention

**Strategy:**
- **High-resolution data**: 7-30 days
- **Aggregated data**: 90-365 days
- **Long-term trends**: 1-2 years
- **Compliance logs**: Per regulatory requirements

**Cost Optimization:**
- Use tiered storage
- Implement data sampling
- Archive to object storage
- Delete unused metrics

### Security and Compliance

1. **Sensitive Data**
   - Scrub PII from logs
   - Encrypt data in transit and at rest
   - Implement access controls
   - Audit access to monitoring data

2. **Compliance**
   - Retain audit logs appropriately
   - Implement data sovereignty
   - Meet industry standards (PCI, HIPAA, SOC2)
   - Document monitoring practices

### Team Organization

**Responsibilities:**
- **Developers**: Instrument applications
- **SRE/Ops**: Manage infrastructure monitoring
- **On-call**: Respond to alerts
- **Leadership**: Review SLOs and incidents

**Skills Development:**
- Training on monitoring tools
- Runbook creation workshops
- Incident response drills
- Post-mortem reviews

---

**Related:**
- [Development Tools](dev-tools.md) - Local debugging and profiling
- [Infrastructure](infrastructure.md) - Systems to monitor
- [Documentation](documentation.md) - Runbook documentation
