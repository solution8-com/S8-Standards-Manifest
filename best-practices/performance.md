# Performance Best Practices

## Overview

Performance optimization is critical for user satisfaction, SEO rankings, and operational costs. This guide covers strategies to make your applications fast, efficient, and scalable.

## Performance Metrics

### Core Web Vitals

**Largest Contentful Paint (LCP)**: Measures loading performance
- **Target**: < 2.5 seconds
- **Good**: Content loads quickly and visibly

**First Input Delay (FID)**: Measures interactivity
- **Target**: < 100 milliseconds
- **Good**: Page responds immediately to user input

**Cumulative Layout Shift (CLS)**: Measures visual stability
- **Target**: < 0.1
- **Good**: No unexpected layout shifts

**Additional Metrics**:
- **Time to First Byte (TTFB)**: < 600ms
- **First Contentful Paint (FCP)**: < 1.8s
- **Time to Interactive (TTI)**: < 3.8s

## Frontend Performance

### 1. Asset Optimization

#### Image Optimization

```html
<!-- ✅ GOOD: Responsive images with modern formats -->
<picture>
  <source 
    srcset="image.avif" 
    type="image/avif">
  <source 
    srcset="image.webp" 
    type="image/webp">
  <img 
    src="image.jpg" 
    alt="Description"
    loading="lazy"
    width="800" 
    height="600">
</picture>

<!-- ✅ Responsive images for different screen sizes -->
<img 
  srcset="
    small.jpg 400w,
    medium.jpg 800w,
    large.jpg 1200w"
  sizes="
    (max-width: 400px) 400px,
    (max-width: 800px) 800px,
    1200px"
  src="medium.jpg"
  alt="Description">
```

**Best Practices**:
- Use modern formats (WebP, AVIF) with fallbacks
- Implement lazy loading for below-the-fold images
- Serve appropriately sized images
- Compress images (80-85% quality for JPEG)
- Use CDN for image delivery
- Consider image dimensions in layout (prevent CLS)

#### JavaScript Optimization

```javascript
// ✅ GOOD: Code splitting with dynamic imports
// Instead of importing everything upfront
// import HeavyComponent from './HeavyComponent';

// Load on demand
const loadHeavyComponent = async () => {
  const { HeavyComponent } = await import('./HeavyComponent');
  return HeavyComponent;
};

// React example with lazy loading
import { lazy, Suspense } from 'react';

const HeavyComponent = lazy(() => import('./HeavyComponent'));

function App() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <HeavyComponent />
    </Suspense>
  );
}
```

**Webpack Configuration**:

```javascript
// webpack.config.js
module.exports = {
  optimization: {
    splitChunks: {
      chunks: 'all',
      cacheGroups: {
        vendor: {
          test: /[\\/]node_modules[\\/]/,
          name: 'vendors',
          priority: 10
        },
        common: {
          minChunks: 2,
          priority: 5,
          reuseExistingChunk: true
        }
      }
    },
    minimize: true,
    usedExports: true, // Tree shaking
  }
};
```

**Best Practices**:
- Minify and compress JavaScript
- Remove unused code (tree shaking)
- Use code splitting for large bundles
- Defer non-critical JavaScript
- Avoid blocking the main thread

#### CSS Optimization

```html
<!-- ✅ GOOD: Critical CSS inline, rest deferred -->
<head>
  <style>
    /* Critical CSS inlined for above-the-fold content */
    body { margin: 0; font-family: sans-serif; }
    .header { background: #333; color: white; }
  </style>
  
  <!-- Defer non-critical CSS -->
  <link rel="preload" href="styles.css" as="style" onload="this.onload=null;this.rel='stylesheet'">
  <noscript><link rel="stylesheet" href="styles.css"></noscript>
</head>
```

**Best Practices**:
- Inline critical CSS
- Remove unused CSS
- Minify CSS files
- Use CSS containment
- Avoid @import in CSS

### 2. Rendering Optimization

#### Virtual Scrolling

```typescript
// ✅ GOOD: Virtual scrolling for large lists
import { FixedSizeList } from 'react-window';

function LargeList({ items }) {
  const Row = ({ index, style }) => (
    <div style={style}>
      {items[index].name}
    </div>
  );

  return (
    <FixedSizeList
      height={600}
      itemCount={items.length}
      itemSize={50}
      width="100%">
      {Row}
    </FixedSizeList>
  );
}
```

#### Debouncing and Throttling

```javascript
// ✅ GOOD: Debounce expensive operations
function debounce(func, wait) {
  let timeout;
  return function executedFunction(...args) {
    clearTimeout(timeout);
    timeout = setTimeout(() => func(...args), wait);
  };
}

// Usage: Search input
const handleSearch = debounce((query) => {
  fetch(`/api/search?q=${query}`)
    .then(res => res.json())
    .then(data => updateResults(data));
}, 300);

// ✅ GOOD: Throttle scroll events
function throttle(func, limit) {
  let inThrottle;
  return function(...args) {
    if (!inThrottle) {
      func(...args);
      inThrottle = true;
      setTimeout(() => inThrottle = false, limit);
    }
  };
}

// Usage: Scroll handler
window.addEventListener('scroll', throttle(() => {
  console.log('Scroll position:', window.scrollY);
}, 100));
```

#### Memoization

```javascript
// ✅ GOOD: React memoization
import { useMemo, useCallback, memo } from 'react';

const ExpensiveComponent = memo(({ data, onUpdate }) => {
  // Memoize expensive calculations
  const processedData = useMemo(() => {
    return data.map(item => 
      complexCalculation(item)
    );
  }, [data]);

  // Memoize callbacks
  const handleClick = useCallback(() => {
    onUpdate(processedData);
  }, [processedData, onUpdate]);

  return <div onClick={handleClick}>{/* ... */}</div>;
});
```

### 3. Network Optimization

#### Resource Hints

```html
<!-- ✅ GOOD: Use resource hints -->
<head>
  <!-- DNS prefetch for third-party domains -->
  <link rel="dns-prefetch" href="//cdn.example.com">
  
  <!-- Preconnect for critical resources -->
  <link rel="preconnect" href="https://api.example.com">
  
  <!-- Prefetch resources for next page -->
  <link rel="prefetch" href="/next-page.html">
  
  <!-- Preload critical resources -->
  <link rel="preload" href="font.woff2" as="font" type="font/woff2" crossorigin>
  <link rel="preload" href="critical.js" as="script">
</head>
```

#### HTTP/2 and HTTP/3

```nginx
# ✅ GOOD: Enable HTTP/2
server {
    listen 443 ssl http2;
    
    # Enable server push for critical resources
    http2_push /css/critical.css;
    http2_push /js/app.js;
}
```

#### Compression

```javascript
// ✅ GOOD: Enable compression in Express
const compression = require('compression');
const express = require('express');
const app = express();

app.use(compression({
  level: 6,
  threshold: 1024, // Only compress if > 1KB
  filter: (req, res) => {
    if (req.headers['x-no-compression']) {
      return false;
    }
    return compression.filter(req, res);
  }
}));
```

**Nginx Configuration**:

```nginx
# ✅ GOOD: Gzip compression
gzip on;
gzip_vary on;
gzip_min_length 1024;
gzip_comp_level 6;
gzip_types 
  text/plain 
  text/css 
  text/xml 
  text/javascript 
  application/json 
  application/javascript 
  application/xml+rss;
```

### 4. Service Workers and Caching

```javascript
// ✅ GOOD: Service worker with caching strategy
// sw.js
const CACHE_NAME = 'v1';
const STATIC_ASSETS = [
  '/',
  '/css/styles.css',
  '/js/app.js',
  '/images/logo.png'
];

// Install - cache static assets
self.addEventListener('install', (event) => {
  event.waitUntil(
    caches.open(CACHE_NAME)
      .then(cache => cache.addAll(STATIC_ASSETS))
  );
});

// Fetch - network first, fall back to cache
self.addEventListener('fetch', (event) => {
  event.respondWith(
    fetch(event.request)
      .then(response => {
        const responseClone = response.clone();
        caches.open(CACHE_NAME)
          .then(cache => cache.put(event.request, responseClone));
        return response;
      })
      .catch(() => caches.match(event.request))
  );
});
```

## Backend Performance

### 1. API Optimization

#### Response Compression

```python
# ✅ GOOD: FastAPI with compression
from fastapi import FastAPI
from fastapi.middleware.gzip import GZipMiddleware

app = FastAPI()
app.add_middleware(GZipMiddleware, minimum_size=1000)

@app.get("/api/data")
async def get_data():
    return {"data": large_dataset}
```

#### Pagination

```javascript
// ✅ GOOD: Cursor-based pagination
app.get('/api/posts', async (req, res) => {
  const limit = parseInt(req.query.limit) || 20;
  const cursor = req.query.cursor;
  
  const query = Post.find()
    .sort({ createdAt: -1 })
    .limit(limit + 1);
  
  if (cursor) {
    query.where('createdAt').lt(cursor);
  }
  
  const posts = await query.exec();
  const hasMore = posts.length > limit;
  
  if (hasMore) {
    posts.pop(); // Remove extra item
  }
  
  res.json({
    data: posts,
    nextCursor: hasMore ? posts[posts.length - 1].createdAt : null
  });
});
```

#### Response Caching

```javascript
// ✅ GOOD: Cache API responses
const NodeCache = require('node-cache');
const cache = new NodeCache({ stdTTL: 600 }); // 10 minutes

app.get('/api/products/:id', async (req, res) => {
  const cacheKey = `product:${req.params.id}`;
  
  // Check cache first
  const cached = cache.get(cacheKey);
  if (cached) {
    return res.json(cached);
  }
  
  // Fetch from database
  const product = await Product.findById(req.params.id);
  
  // Store in cache
  cache.set(cacheKey, product);
  
  res.json(product);
});
```

### 2. Async Processing

```python
# ✅ GOOD: Offload heavy tasks to background workers
from celery import Celery
from fastapi import BackgroundTasks

celery_app = Celery('tasks', broker='redis://localhost:6379')

@celery_app.task
def process_large_file(file_path):
    # Heavy processing
    result = analyze_file(file_path)
    return result

@app.post("/upload")
async def upload_file(file: UploadFile):
    file_path = save_file(file)
    
    # Queue task instead of blocking
    task = process_large_file.delay(file_path)
    
    return {
        "task_id": task.id,
        "status": "processing"
    }

@app.get("/task/{task_id}")
async def get_task_status(task_id: str):
    task = celery_app.AsyncResult(task_id)
    return {
        "status": task.state,
        "result": task.result if task.ready() else None
    }
```

### 3. Connection Pooling

```python
# ✅ GOOD: Database connection pooling
from sqlalchemy import create_engine
from sqlalchemy.pool import QueuePool

engine = create_engine(
    DATABASE_URL,
    poolclass=QueuePool,
    pool_size=20,          # Number of connections to maintain
    max_overflow=10,       # Max connections beyond pool_size
    pool_timeout=30,       # Timeout waiting for connection
    pool_recycle=3600,     # Recycle connections after 1 hour
    pool_pre_ping=True     # Verify connection before using
)
```

```javascript
// ✅ GOOD: Redis connection pooling
const redis = require('redis');
const { promisify } = require('util');

const client = redis.createClient({
  host: 'localhost',
  port: 6379,
  retry_strategy: (options) => {
    if (options.total_retry_time > 1000 * 60 * 60) {
      return new Error('Retry time exhausted');
    }
    return Math.min(options.attempt * 100, 3000);
  }
});

const getAsync = promisify(client.get).bind(client);
const setAsync = promisify(client.set).bind(client);
```

## Database Optimization

### 1. Query Optimization

```sql
-- ❌ BAD: N+1 query problem
SELECT * FROM users WHERE id IN (1,2,3);
-- Then for each user:
SELECT * FROM orders WHERE user_id = ?;

-- ✅ GOOD: Use JOIN to fetch in one query
SELECT 
  u.*,
  o.id as order_id,
  o.total as order_total
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
WHERE u.id IN (1,2,3);
```

```javascript
// ✅ GOOD: Use ORM efficiently (Sequelize example)
// Instead of
const users = await User.findAll();
for (const user of users) {
  user.orders = await Order.findAll({ where: { userId: user.id } });
}

// Do this:
const users = await User.findAll({
  include: [{ model: Order }]
});
```

### 2. Indexing

```sql
-- ✅ GOOD: Create indexes for frequent queries
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_orders_user_created ON orders(user_id, created_at);

-- ✅ Partial index for specific conditions
CREATE INDEX idx_active_users ON users(email) WHERE is_active = true;

-- ✅ Covering index to avoid table lookup
CREATE INDEX idx_user_profile ON users(id, email, name, created_at);
```

**Index Best Practices**:
- Index columns used in WHERE, JOIN, ORDER BY
- Use composite indexes for multi-column queries
- Don't over-index (impacts write performance)
- Monitor and analyze query plans
- Consider index size vs. benefit

### 3. Database Caching

```python
# ✅ GOOD: Redis caching layer
import redis
import json

redis_client = redis.Redis(host='localhost', port=6379, decode_responses=True)

def get_user_profile(user_id):
    cache_key = f"user:{user_id}:profile"
    
    # Try cache first
    cached = redis_client.get(cache_key)
    if cached:
        return json.loads(cached)
    
    # Cache miss - query database
    user = db.query(User).filter(User.id == user_id).first()
    profile = {
        'id': user.id,
        'name': user.name,
        'email': user.email
    }
    
    # Store in cache (expire after 1 hour)
    redis_client.setex(cache_key, 3600, json.dumps(profile))
    
    return profile

def update_user_profile(user_id, data):
    # Update database
    db.query(User).filter(User.id == user_id).update(data)
    db.commit()
    
    # Invalidate cache
    cache_key = f"user:{user_id}:profile"
    redis_client.delete(cache_key)
```

### 4. Database Partitioning

```sql
-- ✅ GOOD: Partition large tables by date
CREATE TABLE orders (
    id SERIAL,
    user_id INTEGER,
    total DECIMAL(10,2),
    created_at TIMESTAMP
) PARTITION BY RANGE (created_at);

CREATE TABLE orders_2024_q1 PARTITION OF orders
    FOR VALUES FROM ('2024-01-01') TO ('2024-04-01');

CREATE TABLE orders_2024_q2 PARTITION OF orders
    FOR VALUES FROM ('2024-04-01') TO ('2024-07-01');
```

## Caching Strategies

### 1. Cache-Aside (Lazy Loading)

```javascript
// ✅ Application checks cache, loads on miss
async function getCachedData(key) {
  let data = await cache.get(key);
  
  if (!data) {
    data = await database.query(key);
    await cache.set(key, data, { ttl: 3600 });
  }
  
  return data;
}
```

### 2. Write-Through Cache

```javascript
// ✅ Update cache on every write
async function updateData(key, value) {
  await database.update(key, value);
  await cache.set(key, value);
}
```

### 3. Write-Behind Cache

```javascript
// ✅ Write to cache immediately, database asynchronously
async function updateDataAsync(key, value) {
  await cache.set(key, value);
  
  // Queue database update
  queue.add('db-update', { key, value });
}
```

### 4. Cache Invalidation

```javascript
// ✅ GOOD: Strategic cache invalidation
class CacheManager {
  async invalidateUser(userId) {
    const patterns = [
      `user:${userId}:*`,
      `posts:user:${userId}`,
      `followers:${userId}`,
      `feed:${userId}`
    ];
    
    for (const pattern of patterns) {
      await this.deletePattern(pattern);
    }
  }
  
  async deletePattern(pattern) {
    const keys = await redis.keys(pattern);
    if (keys.length > 0) {
      await redis.del(...keys);
    }
  }
}
```

## Load Testing and Profiling

### Load Testing with k6

```javascript
// load-test.js
import http from 'k6/http';
import { check, sleep } from 'k6';

export const options = {
  stages: [
    { duration: '2m', target: 100 },  // Ramp up
    { duration: '5m', target: 100 },  // Stay at 100 users
    { duration: '2m', target: 200 },  // Ramp to 200
    { duration: '5m', target: 200 },  // Stay at 200
    { duration: '2m', target: 0 },    // Ramp down
  ],
  thresholds: {
    http_req_duration: ['p(95)<500'], // 95% of requests under 500ms
    http_req_failed: ['rate<0.01'],   // Error rate under 1%
  },
};

export default function () {
  const res = http.get('https://api.example.com/products');
  
  check(res, {
    'status is 200': (r) => r.status === 200,
    'response time < 500ms': (r) => r.timings.duration < 500,
  });
  
  sleep(1);
}
```

**Run the test**:
```bash
k6 run load-test.js
```

### Application Profiling

```javascript
// ✅ GOOD: Performance monitoring
const { PerformanceObserver, performance } = require('perf_hooks');

// Measure function execution time
function measurePerformance(fn, label) {
  return async (...args) => {
    const start = performance.now();
    const result = await fn(...args);
    const duration = performance.now() - start;
    
    console.log(`${label}: ${duration.toFixed(2)}ms`);
    
    return result;
  };
}

// Usage
const fetchUserData = measurePerformance(
  async (userId) => {
    return await db.getUser(userId);
  },
  'fetchUserData'
);
```

### Continuous Performance Monitoring

```javascript
// ✅ GOOD: Real-time performance tracking
const client = require('prom-client');

// Create metrics
const httpRequestDuration = new client.Histogram({
  name: 'http_request_duration_seconds',
  help: 'Duration of HTTP requests in seconds',
  labelNames: ['method', 'route', 'status_code']
});

// Middleware to track requests
app.use((req, res, next) => {
  const start = Date.now();
  
  res.on('finish', () => {
    const duration = (Date.now() - start) / 1000;
    httpRequestDuration
      .labels(req.method, req.route?.path || req.path, res.statusCode)
      .observe(duration);
  });
  
  next();
});

// Expose metrics endpoint
app.get('/metrics', async (req, res) => {
  res.set('Content-Type', client.register.contentType);
  res.end(await client.register.metrics());
});
```

## Performance Budget

```json
{
  "budgets": [
    {
      "resourceSizes": [
        {
          "resourceType": "script",
          "budget": 300
        },
        {
          "resourceType": "total",
          "budget": 500
        }
      ]
    },
    {
      "timings": [
        {
          "metric": "interactive",
          "budget": 3000
        },
        {
          "metric": "first-contentful-paint",
          "budget": 1800
        }
      ]
    }
  ]
}
```

## Performance Checklist

### Frontend
- [ ] Images optimized and lazy-loaded
- [ ] JavaScript minified and code-split
- [ ] CSS optimized and critical CSS inlined
- [ ] Fonts optimized (subset, preload)
- [ ] Third-party scripts loaded async/defer
- [ ] Service worker implemented for caching
- [ ] Resource hints used (preconnect, prefetch)
- [ ] Bundle size analyzed and optimized

### Backend
- [ ] Database queries optimized with indexes
- [ ] API responses cached appropriately
- [ ] Connection pooling configured
- [ ] Background jobs for heavy processing
- [ ] Response compression enabled
- [ ] CDN configured for static assets
- [ ] Rate limiting implemented
- [ ] Horizontal scaling capability

### Monitoring
- [ ] Performance metrics tracked
- [ ] Real User Monitoring (RUM) enabled
- [ ] Synthetic monitoring configured
- [ ] Alerts set for performance degradation
- [ ] Regular load testing scheduled
- [ ] Performance budget defined

## Tools and Resources

### Testing Tools
- **Lighthouse**: Automated auditing
- **WebPageTest**: Detailed performance analysis
- **Chrome DevTools**: Profiling and debugging
- **k6**: Load testing
- **Apache JMeter**: Performance testing

### Monitoring Tools
- **New Relic**: Application performance monitoring
- **Datadog**: Infrastructure monitoring
- **Sentry**: Error tracking with performance
- **Grafana**: Metrics visualization
- **Prometheus**: Metrics collection

### Analysis Tools
- **webpack-bundle-analyzer**: Bundle size analysis
- **source-map-explorer**: JavaScript bundle analysis
- **PurgeCSS**: Remove unused CSS
- **ImageOptim**: Image compression

---

**Remember**: Performance is a feature. Continuously monitor, measure, and optimize to deliver the best user experience.
