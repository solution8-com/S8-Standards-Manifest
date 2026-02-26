# Accessibility Best Practices

## Overview

Web accessibility ensures that websites, tools, and technologies are designed and developed so that people with disabilities can use them. This includes people with auditory, cognitive, neurological, physical, speech, and visual disabilities.

## Why Accessibility Matters

### Legal and Ethical Reasons
- **Legal Compliance**: ADA, Section 508, EN 301 549, and other regulations
- **Inclusive Design**: Everyone deserves equal access to information and services
- **Social Responsibility**: 15% of the world's population has some form of disability

### Business Benefits
- **Larger Audience**: Reach 1+ billion people with disabilities worldwide
- **Better SEO**: Accessible sites are more discoverable by search engines
- **Improved UX**: Accessibility improvements benefit all users
- **Brand Reputation**: Demonstrates commitment to inclusivity
- **Reduced Legal Risk**: Avoid accessibility lawsuits

## WCAG (Web Content Accessibility Guidelines)

### WCAG 2.1 Conformance Levels

**Level A** (Minimum): Basic accessibility features
**Level AA** (Recommended): Addresses major accessibility barriers
**Level AAA** (Enhanced): Highest level of accessibility

### Four POUR Principles

#### 1. Perceivable
Information and UI components must be presentable to users in ways they can perceive.

**Guidelines**:
- Provide text alternatives for non-text content
- Provide captions and alternatives for multimedia
- Create content that can be presented in different ways
- Make it easier for users to see and hear content

#### 2. Operable
UI components and navigation must be operable.

**Guidelines**:
- Make all functionality available from keyboard
- Give users enough time to read and use content
- Don't design content that causes seizures
- Help users navigate and find content

#### 3. Understandable
Information and UI operation must be understandable.

**Guidelines**:
- Make text readable and understandable
- Make content appear and operate in predictable ways
- Help users avoid and correct mistakes

#### 4. Robust
Content must be robust enough to be interpreted by a wide variety of user agents.

**Guidelines**:
- Maximize compatibility with current and future tools

## Semantic HTML

### Use Proper HTML Elements

```html
<!-- ❌ BAD: Non-semantic divs -->
<div class="header">
  <div class="nav">
    <div class="nav-item">Home</div>
    <div class="nav-item">About</div>
  </div>
</div>
<div class="main-content">
  <div class="article">
    <div class="title">Article Title</div>
    <div class="content">Article content...</div>
  </div>
</div>

<!-- ✅ GOOD: Semantic HTML -->
<header>
  <nav>
    <ul>
      <li><a href="/">Home</a></li>
      <li><a href="/about">About</a></li>
    </ul>
  </nav>
</header>
<main>
  <article>
    <h1>Article Title</h1>
    <p>Article content...</p>
  </article>
</main>
```

### Semantic HTML Elements

```html
<!-- ✅ Document Structure -->
<header>Site or section header</header>
<nav>Navigation links</nav>
<main>Main content (one per page)</main>
<article>Self-contained content</article>
<section>Thematic grouping</section>
<aside>Tangential content</aside>
<footer>Site or section footer</footer>

<!-- ✅ Text Content -->
<h1> to <h6>Headings (hierarchical)</h6>
<p>Paragraphs</p>
<ul>, <ol>, <li>Lists</li>
<blockquote>Quotations</blockquote>
<figure>, <figcaption>Figures with captions</figcaption></figure>

<!-- ✅ Interactive Elements -->
<button>Clickable buttons</button>
<a href="">Links</a>
<form>, <input>, <label>, <textarea>Forms</textarea></label></input></form>
```

### Heading Hierarchy

```html
<!-- ✅ GOOD: Logical heading structure -->
<h1>Page Title</h1>
  <h2>Main Section</h2>
    <h3>Subsection</h3>
    <h3>Another Subsection</h3>
  <h2>Another Main Section</h2>
    <h3>Subsection</h3>

<!-- ❌ BAD: Skipping heading levels -->
<h1>Page Title</h1>
  <h4>Subsection</h4> <!-- Skipped h2 and h3 -->
```

## ARIA (Accessible Rich Internet Applications)

### ARIA Roles

```html
<!-- ✅ Landmark roles -->
<div role="banner">Site header</div>
<div role="navigation">Navigation</div>
<div role="main">Main content</div>
<div role="complementary">Sidebar</div>
<div role="contentinfo">Footer</div>
<div role="search">Search form</div>

<!-- ✅ Widget roles -->
<div role="button" tabindex="0">Custom button</div>
<div role="tab" tabindex="0">Tab</div>
<div role="dialog" aria-modal="true">Modal dialog</div>
<div role="alert">Important message</div>
```

**Note**: Prefer semantic HTML over ARIA when possible:
```html
<!-- ✅ BETTER: Use native elements -->
<button>Click me</button>
<!-- Instead of -->
<div role="button" tabindex="0">Click me</div>

<nav>Navigation</nav>
<!-- Instead of -->
<div role="navigation">Navigation</div>
```

### ARIA States and Properties

```html
<!-- ✅ aria-label: Accessible name -->
<button aria-label="Close dialog">
  <span aria-hidden="true">×</span>
</button>

<!-- ✅ aria-labelledby: Reference to label -->
<h2 id="dialog-title">Confirm Action</h2>
<div role="dialog" aria-labelledby="dialog-title">
  <!-- Dialog content -->
</div>

<!-- ✅ aria-describedby: Additional description -->
<input 
  type="email" 
  id="email"
  aria-describedby="email-hint">
<span id="email-hint">We'll never share your email</span>

<!-- ✅ aria-expanded: Expandable sections -->
<button 
  aria-expanded="false" 
  aria-controls="menu">
  Menu
</button>
<ul id="menu" hidden>
  <li>Item 1</li>
  <li>Item 2</li>
</ul>

<!-- ✅ aria-hidden: Hide decorative elements -->
<button>
  Save
  <span aria-hidden="true" class="icon">💾</span>
</button>

<!-- ✅ aria-live: Dynamic content updates -->
<div aria-live="polite" aria-atomic="true">
  <p>3 new messages</p>
</div>

<!-- ✅ aria-invalid: Form validation -->
<input 
  type="text" 
  aria-invalid="true"
  aria-describedby="error-msg">
<span id="error-msg" role="alert">
  Please enter a valid email
</span>
```

### Common ARIA Patterns

#### Accordion

```html
<div class="accordion">
  <h3>
    <button 
      id="accordion-button-1"
      aria-expanded="false"
      aria-controls="accordion-panel-1">
      Section 1
      <span aria-hidden="true">▼</span>
    </button>
  </h3>
  <div 
    id="accordion-panel-1"
    role="region"
    aria-labelledby="accordion-button-1"
    hidden>
    <p>Panel content...</p>
  </div>
</div>
```

#### Modal Dialog

```html
<div 
  role="dialog"
  aria-modal="true"
  aria-labelledby="dialog-title"
  aria-describedby="dialog-desc">
  
  <h2 id="dialog-title">Delete Item</h2>
  <p id="dialog-desc">Are you sure you want to delete this item?</p>
  
  <button>Cancel</button>
  <button>Delete</button>
</div>
```

#### Tab Panel

```html
<div role="tablist" aria-label="Settings">
  <button 
    role="tab" 
    aria-selected="true"
    aria-controls="panel-1"
    id="tab-1">
    General
  </button>
  <button 
    role="tab" 
    aria-selected="false"
    aria-controls="panel-2"
    id="tab-2">
    Privacy
  </button>
</div>

<div 
  role="tabpanel" 
  id="panel-1"
  aria-labelledby="tab-1">
  General settings...
</div>
<div 
  role="tabpanel" 
  id="panel-2"
  aria-labelledby="tab-2"
  hidden>
  Privacy settings...
</div>
```

## Keyboard Navigation

### Focus Management

```html
<!-- ✅ Make custom interactive elements focusable -->
<div 
  role="button" 
  tabindex="0"
  onclick="handleClick()"
  onkeydown="handleKeyDown(event)">
  Custom Button
</div>

<script>
function handleKeyDown(event) {
  // Activate on Enter or Space
  if (event.key === 'Enter' || event.key === ' ') {
    event.preventDefault();
    handleClick();
  }
}
</script>
```

### Focus Indicators

```css
/* ❌ BAD: Removing focus outline */
*:focus {
  outline: none;
}

/* ✅ GOOD: Custom focus indicator */
*:focus {
  outline: 2px solid #4A90E2;
  outline-offset: 2px;
}

/* ✅ BETTER: Use :focus-visible for keyboard-only */
*:focus-visible {
  outline: 2px solid #4A90E2;
  outline-offset: 2px;
}
```

### Skip Links

```html
<!-- ✅ Allow keyboard users to skip navigation -->
<a href="#main-content" class="skip-link">
  Skip to main content
</a>

<!-- Navigation -->
<nav>...</nav>

<main id="main-content" tabindex="-1">
  <!-- Main content -->
</main>

<style>
.skip-link {
  position: absolute;
  top: -40px;
  left: 0;
  background: #000;
  color: #fff;
  padding: 8px;
  text-decoration: none;
  z-index: 100;
}

.skip-link:focus {
  top: 0;
}
</style>
```

### Keyboard Shortcuts

```javascript
// ✅ Implement keyboard shortcuts
document.addEventListener('keydown', (event) => {
  // Ctrl/Cmd + K for search
  if ((event.ctrlKey || event.metaKey) && event.key === 'k') {
    event.preventDefault();
    openSearchDialog();
  }
  
  // Escape to close modals
  if (event.key === 'Escape') {
    closeCurrentModal();
  }
  
  // Arrow keys for navigation
  if (event.key === 'ArrowUp' || event.key === 'ArrowDown') {
    navigateList(event.key);
  }
});
```

### Focus Trap in Modals

```javascript
// ✅ Trap focus within modal
function trapFocus(element) {
  const focusableElements = element.querySelectorAll(
    'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])'
  );
  
  const firstFocusable = focusableElements[0];
  const lastFocusable = focusableElements[focusableElements.length - 1];
  
  element.addEventListener('keydown', (e) => {
    if (e.key !== 'Tab') return;
    
    if (e.shiftKey) {
      // Shift + Tab
      if (document.activeElement === firstFocusable) {
        e.preventDefault();
        lastFocusable.focus();
      }
    } else {
      // Tab
      if (document.activeElement === lastFocusable) {
        e.preventDefault();
        firstFocusable.focus();
      }
    }
  });
}
```

## Screen Reader Support

### Alternative Text

```html
<!-- ✅ Informative images -->
<img src="chart.png" alt="Sales increased 25% from Q1 to Q2">

<!-- ✅ Decorative images -->
<img src="decorative-line.png" alt="">

<!-- ✅ Functional images (links/buttons) -->
<a href="/search">
  <img src="search-icon.png" alt="Search">
</a>

<!-- ✅ Complex images -->
<figure>
  <img src="chart.png" alt="Sales chart">
  <figcaption>
    Detailed description: Sales started at $100K in January,
    peaked at $150K in March, and stabilized at $125K by June.
  </figcaption>
</figure>

<!-- ✅ SVG accessibility -->
<svg role="img" aria-labelledby="svg-title">
  <title id="svg-title">Company Logo</title>
  <!-- SVG content -->
</svg>
```

### Form Labels

```html
<!-- ✅ GOOD: Explicit labels -->
<label for="username">Username:</label>
<input type="text" id="username" name="username">

<!-- ✅ GOOD: Implicit labels -->
<label>
  Email:
  <input type="email" name="email">
</label>

<!-- ✅ GOOD: Fieldsets for groups -->
<fieldset>
  <legend>Contact Preference</legend>
  <label>
    <input type="radio" name="contact" value="email">
    Email
  </label>
  <label>
    <input type="radio" name="contact" value="phone">
    Phone
  </label>
</fieldset>

<!-- ✅ GOOD: Required fields -->
<label for="required-field">
  Name <span aria-label="required">*</span>
</label>
<input 
  type="text" 
  id="required-field"
  required
  aria-required="true">
```

### Live Regions

```html
<!-- ✅ Polite: Announce when convenient -->
<div aria-live="polite">
  <p>Form saved successfully</p>
</div>

<!-- ✅ Assertive: Announce immediately -->
<div aria-live="assertive" role="alert">
  <p>Error: Connection lost</p>
</div>

<!-- ✅ Status updates -->
<div role="status" aria-live="polite" aria-atomic="true">
  <p>Loading... 50% complete</p>
</div>
```

### Visually Hidden Content

```css
/* ✅ Screen reader only text */
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border-width: 0;
}
```

```html
<!-- ✅ Usage -->
<button>
  <span class="sr-only">Delete item</span>
  <span aria-hidden="true">🗑️</span>
</button>
```

## Color and Contrast

### WCAG Contrast Ratios

**Level AA**:
- Normal text: 4.5:1 minimum
- Large text (18pt+): 3:1 minimum

**Level AAA**:
- Normal text: 7:1 minimum
- Large text: 4.5:1 minimum

```css
/* ✅ GOOD: Sufficient contrast */
.button {
  background-color: #0066CC; /* Blue */
  color: #FFFFFF; /* White */
  /* Contrast ratio: 8.59:1 (AAA) */
}

.text {
  color: #333333; /* Dark gray */
  background-color: #FFFFFF; /* White */
  /* Contrast ratio: 12.63:1 (AAA) */
}

/* ❌ BAD: Insufficient contrast */
.low-contrast {
  color: #777777; /* Light gray */
  background-color: #FFFFFF; /* White */
  /* Contrast ratio: 4.47:1 (Fails AA for normal text) */
}
```

### Don't Rely on Color Alone

```html
<!-- ❌ BAD: Color only -->
<p style="color: red;">Error: Invalid input</p>

<!-- ✅ GOOD: Color + icon + text -->
<p class="error">
  <span class="icon" aria-hidden="true">⚠️</span>
  <strong>Error:</strong> Invalid input
</p>

<!-- ✅ GOOD: Multiple visual cues -->
<button class="primary" style="background: green; font-weight: bold;">
  ✓ Confirm
</button>
```

## Responsive and Zoom-Friendly

### Text Resizing

```css
/* ✅ Use relative units */
body {
  font-size: 16px; /* Base size */
}

h1 {
  font-size: 2rem; /* Scales with user's settings */
}

.container {
  max-width: 60rem; /* Scales proportionally */
}

/* ❌ Avoid fixed sizes for text */
.bad-text {
  font-size: 14px;
  height: 20px;
  overflow: hidden; /* Clips text when zoomed */
}
```

### Viewport and Zoom

```html
<!-- ✅ Allow user scaling -->
<meta name="viewport" content="width=device-width, initial-scale=1">

<!-- ❌ Don't disable zoom -->
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">
```

### Touch Targets

```css
/* ✅ Minimum 44x44px touch target */
.button {
  min-width: 44px;
  min-height: 44px;
  padding: 12px 24px;
}

/* ✅ Adequate spacing between targets */
.button-group button {
  margin: 8px;
}
```

## Accessibility Testing

### Automated Testing Tools

```bash
# Install axe-core for automated testing
npm install --save-dev axe-core

# Install Pa11y for CI/CD
npm install --save-dev pa11y
```

```javascript
// ✅ Automated testing with axe-core
const { AxePuppeteer } = require('@axe-core/puppeteer');
const puppeteer = require('puppeteer');

(async () => {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();
  await page.goto('https://example.com');
  
  const results = await new AxePuppeteer(page).analyze();
  
  console.log('Accessibility Violations:', results.violations.length);
  console.log(results.violations);
  
  await browser.close();
})();
```

```javascript
// ✅ React testing with jest-axe
import { render } from '@testing-library/react';
import { axe, toHaveNoViolations } from 'jest-axe';
import MyComponent from './MyComponent';

expect.extend(toHaveNoViolations);

test('should not have accessibility violations', async () => {
  const { container } = render(<MyComponent />);
  const results = await axe(container);
  expect(results).toHaveNoViolations();
});
```

### Manual Testing Checklist

**Keyboard Navigation**:
- [ ] Can navigate entire page with Tab/Shift+Tab
- [ ] Focus indicator always visible
- [ ] No keyboard traps
- [ ] Logical focus order
- [ ] All interactive elements keyboard accessible

**Screen Reader**:
- [ ] All images have appropriate alt text
- [ ] Form inputs have labels
- [ ] Page has proper heading structure
- [ ] Landmarks identify page regions
- [ ] Dynamic content announces properly

**Visual**:
- [ ] Sufficient color contrast
- [ ] Content readable when zoomed 200%
- [ ] No information conveyed by color alone
- [ ] Text spacing can be adjusted
- [ ] Touch targets at least 44x44px

**Content**:
- [ ] Language of page specified
- [ ] Link text descriptive
- [ ] Error messages clear and helpful
- [ ] No time limits or configurable
- [ ] No flashing content

### Browser Extensions

- **WAVE**: Visual feedback of accessibility issues
- **axe DevTools**: Detailed accessibility testing
- **Lighthouse**: Comprehensive audits
- **NVDA** (Windows) / **VoiceOver** (Mac): Screen readers
- **Colour Contrast Analyser**: Check contrast ratios

### Testing with Assistive Technologies

```markdown
**Screen Readers**:
- NVDA (Windows) - Free
- JAWS (Windows) - Commercial
- VoiceOver (Mac/iOS) - Built-in
- TalkBack (Android) - Built-in

**Testing Checklist**:
1. Navigate by headings (H key)
2. Navigate by landmarks (D key)
3. Navigate by links (K key)
4. Navigate by forms (F key)
5. Read page from top to bottom
6. Test all interactive elements
7. Verify dynamic content announces
```

## Common Accessibility Issues

### Issue: Missing Alt Text

```html
<!-- ❌ BAD -->
<img src="product.jpg">

<!-- ✅ GOOD -->
<img src="product.jpg" alt="Blue cotton t-shirt, size medium">
```

### Issue: Non-Accessible Modals

```javascript
// ✅ Accessible modal implementation
function openModal(modal) {
  // Save current focus
  const previousFocus = document.activeElement;
  
  // Show modal
  modal.removeAttribute('hidden');
  modal.setAttribute('aria-modal', 'true');
  
  // Move focus to modal
  const firstFocusable = modal.querySelector('button, [href], input');
  firstFocusable.focus();
  
  // Trap focus
  trapFocus(modal);
  
  // Return focus on close
  modal.addEventListener('close', () => {
    previousFocus.focus();
  }, { once: true });
}
```

### Issue: Inaccessible Custom Dropdowns

```html
<!-- ✅ Accessible custom select -->
<div class="custom-select">
  <button
    id="select-button"
    aria-haspopup="listbox"
    aria-expanded="false"
    aria-labelledby="select-label select-button">
    Choose option
  </button>
  <ul
    role="listbox"
    aria-labelledby="select-label"
    hidden>
    <li role="option" tabindex="0">Option 1</li>
    <li role="option" tabindex="0">Option 2</li>
    <li role="option" tabindex="0">Option 3</li>
  </ul>
</div>
```

## Accessibility Checklist

### Development
- [ ] Use semantic HTML elements
- [ ] Provide text alternatives for images
- [ ] Ensure sufficient color contrast (4.5:1)
- [ ] Make all functionality keyboard accessible
- [ ] Add ARIA labels where needed
- [ ] Use proper heading hierarchy
- [ ] Label all form inputs
- [ ] Include skip navigation links

### Testing
- [ ] Run automated accessibility tests
- [ ] Test with keyboard only
- [ ] Test with screen reader
- [ ] Verify zoom up to 200%
- [ ] Check color contrast
- [ ] Validate HTML
- [ ] Test on mobile devices

### Ongoing
- [ ] Include accessibility in design reviews
- [ ] Train team on accessibility
- [ ] Monitor accessibility metrics
- [ ] Address user feedback
- [ ] Stay updated on WCAG changes

## Resources

### Standards and Guidelines
- [WCAG 2.1 Guidelines](https://www.w3.org/WAI/WCAG21/quickref/)
- [ARIA Authoring Practices](https://www.w3.org/WAI/ARIA/apg/)
- [Section 508 Standards](https://www.section508.gov/)
- [A11Y Project Checklist](https://www.a11yproject.com/checklist/)

### Tools
- [axe DevTools](https://www.deque.com/axe/devtools/)
- [WAVE](https://wave.webaim.org/)
- [Pa11y](https://pa11y.org/)
- [Lighthouse](https://developers.google.com/web/tools/lighthouse)
- [Color Contrast Checker](https://webaim.org/resources/contrastchecker/)

### Learning Resources
- [WebAIM](https://webaim.org/)
- [A11ycasts (YouTube)](https://www.youtube.com/playlist?list=PLNYkxOF6rcICWx0C9LVWWVqvHlYJyqw7g)
- [Inclusive Components](https://inclusive-components.design/)
- [Accessibility Developer Guide](https://www.accessibility-developer-guide.com/)

---

**Remember**: Accessibility is not a feature—it's a fundamental requirement. Build inclusive experiences from the start, not as an afterthought.
