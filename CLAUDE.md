# CLAUDE.md - Stiqued Modernization Project

## Project Overview
This project modernizes casjay's Stiqued fork (based on the original Stikked pastebin) to support PHP 7.0+ through PHP 8.3+ and implement mobile-first responsive design with modern dark/light theme system.

**Repository**: https://github.com/casjay/stiqued

## Current State Analysis
- **PHP Requirement**: Currently PHP 5.6+ (end-of-life since 2019)
- **Framework**: CodeIgniter 3.x (older version)
- **Themes**: Multiple themes available but **NOT mobile-responsive**
  - default, bootstrap, gabdark, i386, stikkedizr, cleanwhite, snowkat, geocities
- **CSS Framework**: Bootstrap 3.x era styling
- **Known Issues**: 
  - Carabiner library PHP 8 compatibility problems
  - Uses deprecated mysql drivers
  - No mobile-first responsive design
  - No modern dark/light mode system

## Primary Objectives

### 1. PHP 7.0+ to 8.3+ Compatibility (Priority: Critical)
**Target**: Full compatibility with PHP 7.0, 7.1, 7.2, 7.3, 7.4, 8.0, 8.1, 8.2, 8.3

#### Framework Updates:
- [ ] **Upgrade to CodeIgniter 3.1.13**: Latest version with PHP 8.0-8.3 support
- [ ] **Fix Carabiner Library**: Address "required parameter follows optional parameter" error
  - File: `/application/libraries/Carabiner.php` line 464
  - Reference: https://github.com/tonydewan/Carabiner/pull/13

#### Critical PHP Compatibility Tasks:
- [ ] **Replace deprecated functions**:
  ```php
  // OLD (removed in PHP 7+)
  mysql_connect() → mysqli_connect() or PDO
  mysql_query() → mysqli_query() or PDO prepare/execute
  ereg() → preg_match()
  split() → explode() or preg_split()
  each() → foreach loops
  ```

- [ ] **Database Driver Migration**:
  - Update config to use `mysqli` instead of `mysql`
  - Implement prepared statements throughout
  - Add error handling for database operations

- [ ] **PHP 8+ Specific Fixes**:
  - Fix dynamic property creation warnings
  - Update function parameter order (optional before required)
  - Handle strict type declarations
  - Update array syntax where beneficial

#### Security Improvements:
- [ ] **Modern Authentication**: Update to `password_hash()` and `password_verify()`
- [ ] **CSRF Protection**: Implement CodeIgniter's CSRF tokens
- [ ] **Input Sanitization**: Review and update all user input handling
- [ ] **SQL Injection Prevention**: Ensure all queries use prepared statements
- [ ] **Pretty URLs**: Implement clean URLs without web server rewriting dependency

#### Pretty URLs Implementation (No Web Server Rewriting Required):
- [ ] **CodeIgniter URI Routing**: Use `application/config/routes.php` for clean URLs
- [ ] **Index.php Method**: Keep `index.php` in URLs but make them clean
- [ ] **Fallback Support**: Ensure compatibility with all web servers

```php
// htdocs/application/config/routes.php
$route['view/(:any)'] = 'main/view/$1';
$route['view/(:any)/(:any)'] = 'main/view/$1/$2';
$route['raw/(:any)'] = 'main/view_raw/$1';
$route['download/(:any)'] = 'main/download/$1';
$route['embed/(:any)'] = 'main/embed/$1';
$route['diff/(:any)'] = 'main/view/$1/diff';
$route['api'] = 'api/create';
$route['api/create'] = 'api/create';
$route['api/show/(:any)'] = 'api/show/$1';
$route['trends'] = 'main/trends';
$route['lists'] = 'main/lists';
$route['lists/rss'] = 'main/rss';
$route['archive'] = 'main/archive';
$route['about'] = 'main/about';
$route['404_override'] = '';
$route['translate_uri_dashes'] = FALSE;

// Pretty URL Examples (with index.php):
// yoursite.com/index.php/view/abc123
// yoursite.com/index.php/raw/abc123  
// yoursite.com/index.php/trends
// yoursite.com/index.php/api/create
```

```php
// htdocs/application/config/config.php
// Enable query strings for compatibility
$config['enable_query_strings'] = TRUE;
$config['controller_trigger'] = 'c';
$config['function_trigger'] = 'm';
$config['directory_trigger'] = 'd';

// Allow both URI protocols
$config['uri_protocol'] = 'REQUEST_URI';
$config['url_suffix'] = '';
$config['language'] = 'english';
$config['charset'] = 'UTF-8';
```

### 2. Mobile-First Responsive Theme System (Priority: High)
**Target**: Convert ALL existing themes to mobile-first while preserving their unique visual identity

#### Current Theme Inventory (All to be preserved & converted):
```
/htdocs/themes/
├── default/           # Base theme - PRIMARY TEMPLATE for theming system
├── bootstrap/         # Bootstrap 3.x based - convert to Bootstrap 5
├── gabdark/          # Dark theme (not responsive) - preserve dark aesthetic
├── gabdark3/         # Alternative dark theme - preserve unique style
├── i386/             # Terminal-style theme - preserve retro terminal look
├── stikkedizr/       # Custom theme - preserve visual identity
├── cleanwhite/       # Clean light theme - preserve minimalist design  
├── snowkat/          # Custom theme - preserve unique styling
└── geocities/        # Retro 90s theme - preserve nostalgic 90s aesthetic
```

#### Theme Conversion Strategy:

##### A. Default Theme as Master Template:
- [ ] **Primary Focus**: Default theme becomes the master template for the theming system
- [ ] **Theming Architecture**: Implement modern CSS custom properties in default theme
- [ ] **Reference Implementation**: All other themes will follow default theme's responsive patterns
- [ ] **Bootstrap 5.3+**: Upgrade default theme from Bootstrap 3.x
  - Remove jQuery dependency (Bootstrap 5 is vanilla JS)
  - Update grid system (Bootstrap 5 uses CSS Grid + Flexbox)
  - Implement utility-first approach

##### B. Preserve Visual Identity:
- [ ] **Keep Unique Aesthetics**: Each theme retains its distinctive look and feel
- [ ] **Mobile-First Conversion**: Convert layouts to responsive while preserving visual character
- [ ] **No Design Changes**: Only make responsive, don't alter the visual design language

##### C. Individual Theme Conversion Plan:

**1. Default Theme (Master Template)**:
- [ ] Complete responsive overhaul with CSS custom properties
- [ ] Modern dark/light theme toggle system
- [ ] Bootstrap 5 integration as foundation
- [ ] Mobile-first breakpoints and layouts

**2. Bootstrap Theme**:
- [ ] Upgrade from Bootstrap 3.x to 5.x framework
- [ ] Maintain Bootstrap's visual design language
- [ ] Convert to mobile-first grid system
- [ ] Preserve existing component styling

**3. gabdark & gabdark3 Themes**:
- [ ] Keep existing dark color schemes intact
- [ ] Make layouts responsive while preserving dark aesthetic
- [ ] Ensure readability on mobile devices
- [ ] Maintain current dark theme character

**4. i386 Theme**:
- [ ] Preserve terminal/console aesthetic completely
- [ ] Make terminal-style layouts mobile-friendly
- [ ] Ensure monospace fonts work on mobile
- [ ] Keep retro computing visual identity

**5. stikkedizr Theme**:
- [ ] Analyze current custom styling
- [ ] Convert layout to responsive while keeping unique design
- [ ] Preserve custom visual elements
- [ ] Maintain theme's distinctive character

**6. cleanwhite Theme**:
- [ ] Keep minimalist, clean design approach
- [ ] Make white/light layouts mobile-optimized
- [ ] Preserve clean typography and spacing
- [ ] Maintain current aesthetic philosophy

**7. snowkat Theme**:
- [ ] Preserve unique custom styling
- [ ] Convert to responsive layout patterns
- [ ] Keep theme's distinctive visual elements
- [ ] Maintain current design identity

**8. geocities Theme**:
- [ ] **Preserve 90s nostalgic aesthetic completely**
- [ ] Make retro layouts mobile-responsive (challenging but important)
- [ ] Ensure 90s design elements work on mobile
- [ ] Keep the intentionally retro/nostalgic visual style
```css
/* Mobile First Breakpoint Strategy */
/* Base styles: Mobile (320px+) */
.paste-container {
  padding: 1rem;
  font-size: 14px;
}

/* Tablet (768px+) */
@media (min-width: 768px) {
  .paste-container {
    padding: 1.5rem;
    font-size: 16px;
  }
}

/* Desktop (1024px+) */
@media (min-width: 1024px) {
  .paste-container {
    padding: 2rem;
    max-width: 1200px;
    margin: 0 auto;
  }
}

/* Large Desktop (1440px+) */
@media (min-width: 1440px) {
  .paste-container {
    padding: 2.5rem;
  }
}
```

##### D. Mobile-First Responsive Design Implementation:
##### E. Modern Dark/Light Theme System (Default Theme Only):
**Implementation**: CSS Custom Properties + `light-dark()` function (2024 standard)

```css
/* Applied ONLY to Default Theme */
:root {
  color-scheme: light dark;
  
  /* Primary Colors */
  --primary: light-dark(#007bff, #4dabf7);
  --primary-hover: light-dark(#0056b3, #339af0);
  
  /* Background Colors */
  --bg-primary: light-dark(#ffffff, #121212);
  --bg-secondary: light-dark(#f8f9fa, #1e1e1e);
  --bg-code: light-dark(#f5f5f5, #2d2d2d);
  
  /* Text Colors */
  --text-primary: light-dark(#212529, #e9ecef);
  --text-secondary: light-dark(#6c757d, #adb5bd);
  --text-muted: light-dark(#868e96, #6c757d);
  
  /* Border Colors */
  --border-color: light-dark(#dee2e6, #495057);
  --border-focus: light-dark(#80bdff, #339af0);
  
  /* Syntax Highlighting */
  --syntax-keyword: light-dark(#d73a49, #ff6b6b);
  --syntax-string: light-dark(#032f62, #51cf66);
  --syntax-comment: light-dark(#6a737d, #868e96);
  --syntax-number: light-dark(#005cc5, #4dabf7);
}

/* Theme Toggle Support for Default Theme */
[data-theme="light"] {
  color-scheme: light;
}

[data-theme="dark"] {
  color-scheme: dark;
}

/* Apply Variables in Default Theme */
body {
  background-color: var(--bg-primary);
  color: var(--text-primary);
}

.paste-content {
  background-color: var(--bg-code);
  border: 1px solid var(--border-color);
}
```

**Note**: Other themes (gabdark, i386, geocities, etc.) keep their existing color schemes and visual identity unchanged.

##### F. Enhanced Mobile Features:
- [ ] **Touch-Friendly Interface**:
  - Minimum 44px tap targets
  - Swipe gestures for theme switching
  - Pull-to-refresh functionality
  - Touch-optimized syntax highlighting

- [ ] **Mobile Navigation**:
  - Collapsible hamburger menu
  - Bottom navigation bar option
  - Sticky header with essential controls

- [ ] **Performance Optimizations**:
  - Lazy loading for large pastes
  - Optimized font loading
  - Compressed images and assets
  - Service worker for offline support

### 3. Development Environment & Testing Strategy

#### Docker Multi-Version Setup:
```yaml
# docker-compose.yml
version: '3.8'
services:
  php70:
    build: ./docker/php7.0
    volumes:
      - .:/var/www/html
    ports:
      - "8070:80"
    environment:
      - PHP_VERSION=7.0
      
  php71:
    build: ./docker/php7.1
    volumes:
      - .:/var/www/html
    ports:
      - "8071:80"
      
  php72:
    build: ./docker/php7.2
    volumes:
      - .:/var/www/html
    ports:
      - "8072:80"
      
  php73:
    build: ./docker/php7.3
    volumes:
      - .:/var/www/html
    ports:
      - "8073:80"
      
  php74:
    build: ./docker/php7.4
    volumes:
      - .:/var/www/html
    ports:
      - "8074:80"
      
  php80:
    build: ./docker/php8.0
    volumes:
      - .:/var/www/html
    ports:
      - "8080:80"
      
  php81:
    build: ./docker/php8.1
    volumes:
      - .:/var/www/html
    ports:
      - "8081:80"
      
  php82:
    build: ./docker/php8.2
    volumes:
      - .:/var/www/html
    ports:
      - "8082:80"
      
  php83:
    build: ./docker/php8.3
    volumes:
      - .:/var/www/html
    ports:
      - "8083:80"
      
  mysql:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: stiqued
    ports:
      - "3306:3306"
    volumes:
      - mysql_data:/var/lib/mysql
      
  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
    volumes:
      - ./docker/nginx.conf:/etc/nginx/nginx.conf
    depends_on:
      - php83

volumes:
  mysql_data:
```

#### Dockerfile Templates:
```dockerfile
# docker/php8.3/Dockerfile
FROM php:8.3-apache

# Install required extensions
RUN docker-php-ext-install mysqli pdo pdo_mysql gd

# Enable Apache modules
RUN a2enmod rewrite

# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Set working directory
WORKDIR /var/www/html

# Copy PHP configuration
COPY php.ini /usr/local/etc/php/
```

#### Automated Testing Pipeline:
```bash
#!/bin/bash
# test-all-versions.sh

PHP_VERSIONS=("7.0" "7.1" "7.2" "7.3" "7.4" "8.0" "8.1" "8.2" "8.3")

for version in "${PHP_VERSIONS[@]}"; do
    echo "Testing PHP $version..."
    docker-compose up -d php${version//./}
    
    # Run PHP compatibility checks
    docker exec stiqued_php${version//./}_1 php -l application/
    
    # Run basic functionality tests
    curl -f http://localhost:80${version//./}/
    
    # Run theme tests
    for theme in default bootstrap gabdark i386 stikkedizr cleanwhite snowkat geocities; do
        curl -f "http://localhost:80${version//./}/?theme=$theme"
    done
    
    docker-compose down
done
```

## Implementation Phases

### Phase 1: Foundation & PHP Compatibility (Weeks 1-2)

#### Week 1: Environment Setup
- [ ] **Day 1-2**: Set up Docker development environment
  - Create Dockerfiles for all PHP versions
  - Set up docker-compose configuration
  - Create automated testing scripts

- [ ] **Day 3-4**: CodeIgniter Framework Update
  - Upgrade to CodeIgniter 3.1.13
  - Update configuration files
  - Test basic functionality

- [ ] **Day 5-7**: Critical PHP Fixes
  - Fix Carabiner library compatibility
  - Update database configuration to mysqli
  - Replace deprecated functions

#### Week 2: Database & Security + Pretty URLs
- [ ] **Day 8-10**: Database Layer Modernization
  - Implement prepared statements
  - Update all database queries
  - Add connection error handling

- [ ] **Day 11-14**: Security & URL Improvements
  - Implement CSRF protection
  - Update password hashing
  - Review input sanitization
  - **Implement pretty URLs using CodeIgniter routing**
  - Security testing across PHP versions

### Phase 2: Theme System Overhaul (Weeks 3-4)

#### Week 3: Theme Infrastructure
- [ ] **Day 15-17**: CSS Framework Upgrade
  - Integrate Bootstrap 5.3+
  - Remove jQuery dependencies
  - Update base theme structure

- [ ] **Day 18-21**: Modern Theme System Implementation
  - Implement CSS custom properties
  - Add `light-dark()` function support
  - Create theme toggle functionality
  - Add localStorage persistence

#### Week 4: Theme Conversion & Mobile-First Implementation
- [ ] **Day 22-24**: Convert Existing Themes to Mobile-First
  - Convert each theme individually to responsive
  - Preserve unique visual identity of each theme
  - Test responsive behavior across devices

- [ ] **Day 25-28**: Theme-Specific Optimizations
  - Optimize syntax highlighting for mobile per theme
  - Ensure each theme works well on touch devices
  - Performance improvements per theme
  - Accessibility enhancements while preserving aesthetics

### Phase 3: Advanced Features & Polish (Weeks 5-6)

#### Week 5: Enhanced Functionality
- [ ] **Day 29-31**: Modern JavaScript Features
  - Remove jQuery dependencies
  - Implement vanilla JS solutions
  - Add progressive enhancement

- [ ] **Day 32-35**: Advanced Mobile Features
  - Swipe gestures
  - Pull-to-refresh
  - Offline support (service worker)
  - App-like experience

#### Week 6: Cross-Browser & Device Testing
- [ ] **Day 36-38**: Browser Compatibility
  - Test across major browsers
  - Fix browser-specific issues
  - Validate responsive behavior

- [ ] **Day 39-42**: Mobile Device Testing
  - Test on various devices
  - Performance optimization
  - User experience refinement

### Phase 4: Documentation & Deployment (Week 7-8)

#### Week 7: Documentation
- [ ] **Day 43-45**: Technical Documentation
  - Update installation guide
  - Create theme development guide
  - Document API changes

- [ ] **Day 46-49**: User Documentation
  - Update user manual
  - Create troubleshooting guide
  - Performance optimization guide

#### Week 8: Final Testing & Release
- [ ] **Day 50-52**: Comprehensive Testing
  - Full regression testing
  - Performance benchmarking
  - Security audit

- [ ] **Day 53-56**: Release Preparation
  - Version tagging
  - Release notes
  - Migration guide

## Technical Specifications

### Minimum System Requirements:
- **PHP**: 7.0+ (recommend 8.1+)
- **Database**: MySQL 5.7+ / MariaDB 10.3+ / PostgreSQL 9.6+
- **Web Server**: Apache 2.4+ / Nginx 1.18+
- **Memory**: 128MB minimum, 256MB recommended

### Browser Support Targets:
- **Mobile**: iOS Safari 14+, Android Chrome 90+
- **Desktop**: Chrome 90+, Firefox 88+, Safari 14+, Edge 90+
- **Progressive Enhancement**: Graceful degradation for older browsers

### Performance Targets:
- **Mobile Load Time**: < 3 seconds on 3G
- **First Contentful Paint**: < 1.5 seconds
- **Largest Contentful Paint**: < 2.5 seconds
- **Core Web Vitals**: All metrics in "Good" range
- **Lighthouse Score**: 90+ for Performance, Accessibility, Best Practices

## File Structure (Correct Stiqued Layout)

**Important**: All server components are under `htdocs/` as per the original Stikked/Stiqued architecture.

```
/stiqued-modern/
├── htdocs/                      # Web server document root
│   ├── application/             # CodeIgniter application
│   │   ├── config/
│   │   │   ├── stikked.php      # Main config (note: not stiqued.php)
│   │   │   ├── database.php     # DB config with mysqli
│   │   │   ├── config.php       # CodeIgniter config
│   │   │   └── routes.php       # URL routing
│   │   ├── controllers/
│   │   │   ├── Main.php         # Main controller
│   │   │   ├── Api.php          # API endpoints
│   │   │   └── Admin.php        # Admin interface
│   │   ├── models/
│   │   │   ├── Paste_model.php  # Paste operations
│   │   │   └── User_model.php   # User management
│   │   ├── views/
│   │   │   ├── templates/       # Base templates
│   │   │   └── themes/          # Theme-specific views
│   │   └── libraries/
│   │       ├── Carabiner.php    # Fixed for PHP 8+
│   │       └── Encryption.php   # Updated encryption
│   ├── system/                  # CodeIgniter system files
│   │   ├── core/
│   │   ├── libraries/
│   │   ├── helpers/
│   │   └── database/
│   ├── themes/                  # Theme assets and templates
│   │   ├── default/
│   │   │   ├── css/
│   │   │   │   ├── main.css     # Mobile-first styles
│   │   │   │   └── theme-vars.css # CSS custom properties
│   │   │   ├── js/
│   │   │   │   ├── theme-toggle.js  # Theme switching
│   │   │   │   └── mobile.js        # Mobile enhancements
│   │   │   ├── images/          # Theme images
│   │   │   └── views/           # Theme templates
│   │   ├── bootstrap/           # Bootstrap 5 based theme
│   │   ├── gabdark/            # Modernized dark theme
│   │   ├── gabdark3/           # Alternative dark theme
│   │   ├── i386/               # Terminal theme (responsive)
│   │   ├── stikkedizr/         # Custom responsive theme
│   │   ├── cleanwhite/         # Clean light theme
│   │   ├── snowkat/            # Custom responsive theme
│   │   └── geocities/          # Retro theme (responsive)
│   ├── static/                  # Static assets
│   │   ├── asset/              # Minified assets (writable)
│   │   ├── css/
│   │   │   ├── bootstrap.min.css    # Bootstrap 5
│   │   │   ├── global-themes.css    # Global theme system
│   │   │   └── mobile-base.css      # Mobile base styles
│   │   ├── js/
│   │   │   ├── bootstrap.bundle.min.js  # Bootstrap 5 JS
│   │   │   ├── highlight.min.js         # Syntax highlighting
│   │   │   ├── codemirror/             # CodeMirror editor
│   │   │   └── app.js                   # Main application JS
│   │   └── img/
│   │       ├── icons/           # Application icons
│   │       └── qr/              # QR code images
│   ├── .htaccess               # Apache rewrite rules
│   └── index.php               # Application entry point
├── docker/                     # Docker configuration
│   ├── php7.0/
│   │   └── Dockerfile
│   ├── php7.1/
│   │   └── Dockerfile
│   ├── php7.2/
│   │   └── Dockerfile
│   ├── php7.3/
│   │   └── Dockerfile
│   ├── php7.4/
│   │   └── Dockerfile
│   ├── php8.0/
│   │   └── Dockerfile
│   ├── php8.1/
│   │   └── Dockerfile
│   ├── php8.2/
│   │   └── Dockerfile
│   ├── php8.3/
│   │   └── Dockerfile
│   ├── nginx.conf
│   └── stiqued.php             # Docker-specific config
├── tests/                      # Testing framework
│   ├── php-compatibility/      # PHP version tests
│   ├── responsive/             # Mobile/responsive tests
│   └── themes/                 # Theme functionality tests
├── doc/                        # Documentation
│   ├── INSTALLATION.md
│   ├── CREATING_THEMES.md      # Theme development guide
│   ├── TRANSLATING_STIQUED.md  # Translation guide
│   ├── TROUBLESHOOTING.md      # Troubleshooting guide
│   └── MIGRATION.md            # Upgrade guide
├── docker-compose.yml          # Docker orchestration
├── test-all-versions.sh        # PHP version testing script
└── README.md                   # Project documentation
```

### Key Structure Notes:
- **`htdocs/`** is the web server document root containing all PHP files
- **`htdocs/application/`** contains the CodeIgniter application logic
- **`htdocs/system/`** contains CodeIgniter framework files
- **`htdocs/themes/`** contains all theme assets and templates
- **`htdocs/static/`** contains shared static assets (CSS, JS, images)
- **Config file** is `htdocs/application/config/stikked.php` (not stiqued.php)
- **Assets minification** uses the `static/asset/` writable directory

## Success Criteria

### Functional Requirements:
- [ ] All features work flawlessly on PHP 7.0 through 8.3
- [ ] All themes are fully responsive and mobile-optimized
- [ ] Dark/Light mode toggle works in default theme
- [ ] **Pretty URLs work without web server configuration**
- [ ] No deprecated function usage
- [ ] Backward compatibility maintained for existing pastes
- [ ] All security vulnerabilities addressed

### Performance Requirements:
- [ ] Mobile load time under 3 seconds
- [ ] Desktop load time under 1.5 seconds
- [ ] Core Web Vitals in "Good" range
- [ ] Lighthouse performance score 90+
- [ ] Responsive design works on all target devices

### Code Quality Requirements:
- [ ] PSR-12 coding standards compliance
- [ ] Comprehensive error handling
- [ ] Security best practices implemented
- [ ] Documentation coverage 90%+
- [ ] Automated testing coverage 80%+

## Migration Guide for Existing Users

### Backup Requirements:
```bash
# 1. Backup database
mysqldump -u root -p stiqued > stiqued_backup.sql

# 2. Backup config files
cp application/config/stiqued.php stiqued_config_backup.php

# 3. Backup custom themes (if any)
tar -czf custom_themes_backup.tar.gz htdocs/themes/
```

### Upgrade Process:
1. **Pre-upgrade**: Run compatibility check script
2. **Upgrade**: Replace files (preserve config)
3. **Database**: Run migration scripts if needed
4. **Config**: Update configuration for new features
5. **Test**: Verify all functionality works
6. **Themes**: Re-apply custom theme modifications

## Additional Pretty URLs Implementation Details

### CodeIgniter-Based Pretty URLs (No Web Server Config Required)

#### Advantages of This Approach:
- ✅ **Works on any web server** (Apache, Nginx, IIS, LiteSpeed)
- ✅ **No .htaccess required** - pure PHP solution
- ✅ **No mod_rewrite dependency** - works on shared hosting
- ✅ **Backward compatible** - existing URLs still work
- ✅ **Easy to maintain** - all routing in one PHP file

#### URL Structure Examples:
```
// Current URLs (still work):
yoursite.com/index.php?c=main&m=view&paste_id=abc123
yoursite.com/?c=main&m=view&paste_id=abc123

// New Pretty URLs (with index.php):
yoursite.com/index.php/view/abc123
yoursite.com/index.php/raw/abc123
yoursite.com/index.php/trends
yoursite.com/index.php/api/create

// Optional: Even prettier (if web server supports it):
yoursite.com/view/abc123  # Works if .htaccess available
yoursite.com/raw/abc123   # Falls back to index.php method
```

#### Implementation in Controllers:
```php
// htdocs/application/controllers/Main.php
public function view($paste_id = '', $action = '') 
{
    // Handle both old and new URL formats
    if (empty($paste_id)) {
        $paste_id = $this->input->get('paste_id');
    }
    
    if ($action === 'diff') {
        return $this->view_diff($paste_id);
    }
    
    // Existing view logic...
}

public function view_raw($paste_id = '')
{
    if (empty($paste_id)) {
        $paste_id = $this->input->get('paste_id');
    }
    
    // Existing raw view logic...
}
```

#### Helper Functions for URL Generation:
```php
// htdocs/application/helpers/stikked_helper.php
if (!function_exists('paste_url')) {
    function paste_url($paste_id, $action = '') {
        $CI =& get_instance();
        
        if ($action) {
            return base_url("index.php/{$action}/{$paste_id}");
        }
        
        return base_url("index.php/view/{$paste_id}");
    }
}

if (!function_exists('api_url')) {
    function api_url($endpoint = '') {
        if ($endpoint) {
            return base_url("index.php/api/{$endpoint}");
        }
        
        return base_url("index.php/api");
    }
}
```

#### Graceful Degradation:
```php
// htdocs/application/config/config.php
// Detect if pretty URLs are supported
$config['index_page'] = 'index.php';

// Auto-detect URL rewriting capability
if (isset($_SERVER['REQUEST_URI']) && strpos($_SERVER['REQUEST_URI'], 'index.php') === FALSE) {
    // If server supports URL rewriting, hide index.php
    $config['index_page'] = '';
}
```

This approach ensures **maximum compatibility** while providing cleaner URLs without requiring any web server configuration!

## Modern Developer Experience Features (Phase 5+)

### 🔧 **Real-time Syntax Validation**
**Implementation**: Integrate with language-specific linters and parsers

```javascript
// htdocs/static/js/syntax-validator.js
class SyntaxValidator {
    constructor() {
        this.validators = {
            'javascript': this.validateJavaScript,
            'python': this.validatePython,
            'php': this.validatePHP,
            'json': this.validateJSON,
            // Add more languages as needed
        };
        
        this.debounceTimer = null;
    }
    
    validateOnType(editor, language) {
        clearTimeout(this.debounceTimer);
        this.debounceTimer = setTimeout(() => {
            this.validate(editor.getValue(), language);
        }, 500); // Validate 500ms after user stops typing
    }
    
    validate(code, language) {
        if (this.validators[language]) {
            const errors = this.validators[language](code);
            this.displayErrors(errors);
        }
    }
    
    validateJavaScript(code) {
        // Use ESLint or similar for JS validation
        try {
            new Function(code); // Basic syntax check
            return [];
        } catch (error) {
            return [{
                line: error.lineNumber || 1,
                message: error.message,
                type: 'error'
            }];
        }
    }
    
    validateJSON(code) {
        try {
            JSON.parse(code);
            return [];
        } catch (error) {
            return [{
                line: this.getJSONErrorLine(error.message, code),
                message: error.message,
                type: 'error'
            }];
        }
    }
}
```

### ✨ **Auto-formatting & Code Beautification**
**Implementation**: Integrate Prettier, Black (Python), PHP-CS-Fixer, etc.

```javascript
// htdocs/static/js/code-formatter.js
class CodeFormatter {
    constructor() {
        this.formatters = {
            'javascript': this.formatJavaScript,
            'json': this.formatJSON,
            'html': this.formatHTML,
            'css': this.formatCSS,
            'python': this.formatPython,
            'php': this.formatPHP
        };
    }
    
    async formatCode(code, language) {
        if (this.formatters[language]) {
            return await this.formatters[language](code);
        }
        return code; // Return unchanged if no formatter
    }
    
    formatJavaScript(code) {
        // Use Prettier via CDN or embedded
        return prettier.format(code, {
            parser: 'babel',
            tabWidth: 2,
            semi: true,
            singleQuote: true
        });
    }
    
    formatJSON(code) {
        try {
            const parsed = JSON.parse(code);
            return JSON.stringify(parsed, null, 2);
        } catch (error) {
            return code; // Return original if invalid JSON
        }
    }
    
    formatHTML(code) {
        // Use js-beautify for HTML
        return html_beautify(code, {
            indent_size: 2,
            wrap_line_length: 120
        });
    }
}

// Integration with existing CodeMirror/ACE editor
document.addEventListener('DOMContentLoaded', function() {
    const formatter = new CodeFormatter();
    const formatButton = document.getElementById('format-code');
    
    if (formatButton) {
        formatButton.addEventListener('click', async function() {
            const editor = getActiveEditor(); // Get current editor instance
            const language = getSelectedLanguage();
            const originalCode = editor.getValue();
            
            try {
                const formattedCode = await formatter.formatCode(originalCode, language);
                editor.setValue(formattedCode);
                
                // Show success feedback
                showNotification('Code formatted successfully!', 'success');
            } catch (error) {
                showNotification('Error formatting code: ' + error.message, 'error');
            }
        });
    }
});
```

### 🔄 **Git Integration**
**Implementation**: GitHub/GitLab API integration for gists and snippets

```php
// htdocs/application/libraries/Git_integration.php
<?php
class Git_integration {
    
    private $github_api = 'https://api.github.com';
    private $gitlab_api = 'https://gitlab.com/api/v4';
    
    public function import_github_gist($gist_id, $token = null) {
        $url = "{$this->github_api}/gists/{$gist_id}";
        
        $headers = ['User-Agent: Stiqued-Bot/1.0'];
        if ($token) {
            $headers[] = "Authorization: token {$token}";
        }
        
        $response = $this->make_request($url, $headers);
        
        if ($response && isset($response['files'])) {
            $pastes = [];
            foreach ($response['files'] as $filename => $file) {
                $pastes[] = [
                    'title' => $filename,
                    'content' => $file['content'],
                    'language' => $this->detect_language($file['language']),
                    'description' => $response['description'] ?? ''
                ];
            }
            return $pastes;
        }
        
        return false;
    }
    
    public function export_to_github_gist($paste_data, $token, $public = false) {
        $url = "{$this->github_api}/gists";
        
        $gist_data = [
            'description' => $paste_data['title'] ?? 'Exported from Stiqued',
            'public' => $public,
            'files' => [
                ($paste_data['title'] ?? 'paste') . $this->get_file_extension($paste_data['language']) => [
                    'content' => $paste_data['content']
                ]
            ]
        ];
        
        $headers = [
            'User-Agent: Stiqued-Bot/1.0',
            "Authorization: token {$token}",
            'Content-Type: application/json'
        ];
        
        return $this->make_request($url, $headers, 'POST', json_encode($gist_data));
    }
    
    private function detect_language($github_language) {
        // Map GitHub language names to Stikked/GeSHi language names
        $language_map = [
            'JavaScript' => 'javascript',
            'Python' => 'python',
            'PHP' => 'php',
            'HTML' => 'html',
            'CSS' => 'css',
            'Java' => 'java',
            'C++' => 'cpp',
            'C' => 'c',
            'Ruby' => 'ruby',
            'Go' => 'go',
            'Rust' => 'rust',
            'TypeScript' => 'typescript'
        ];
        
        return $language_map[$github_language] ?? 'text';
    }
}
```

### 📊 **Advanced Diff Views**
**Implementation**: Enhanced diff visualization with multiple view modes

```javascript
// htdocs/static/js/advanced-diff.js
class AdvancedDiffViewer {
    constructor(container) {
        this.container = container;
        this.modes = ['side-by-side', 'inline', 'semantic'];
        this.currentMode = 'side-by-side';
    }
    
    renderDiff(originalText, modifiedText, language) {
        const diffContainer = document.createElement('div');
        diffContainer.className = 'advanced-diff-viewer';
        
        // Add mode switcher
        const modeSelector = this.createModeSelector();
        diffContainer.appendChild(modeSelector);
        
        // Render diff based on current mode
        const diffContent = this.renderDiffContent(originalText, modifiedText, language);
        diffContainer.appendChild(diffContent);
        
        this.container.appendChild(diffContainer);
    }
    
    createModeSelector() {
        const selector = document.createElement('div');
        selector.className = 'diff-mode-selector';
        
        this.modes.forEach(mode => {
            const button = document.createElement('button');
            button.textContent = mode.replace('-', ' ').replace(/\b\w/g, l => l.toUpperCase());
            button.className = mode === this.currentMode ? 'active' : '';
            button.addEventListener('click', () => this.switchMode(mode));
            selector.appendChild(button);
        });
        
        return selector;
    }
    
    renderSideBySideDiff(original, modified, language) {
        const container = document.createElement('div');
        container.className = 'side-by-side-diff';
        
        const originalPane = document.createElement('div');
        originalPane.className = 'diff-pane original';
        originalPane.innerHTML = this.highlightCode(original, language);
        
        const modifiedPane = document.createElement('div');
        modifiedPane.className = 'diff-pane modified';
        modifiedPane.innerHTML = this.highlightCode(modified, language);
        
        container.appendChild(originalPane);
        container.appendChild(modifiedPane);
        
        return container;
    }
    
    renderInlineDiff(original, modified, language) {
        const lines = this.generateInlineDiff(original, modified);
        const container = document.createElement('div');
        container.className = 'inline-diff';
        
        lines.forEach(line => {
            const lineElement = document.createElement('div');
            lineElement.className = `diff-line ${line.type}`;
            lineElement.innerHTML = this.highlightCode(line.content, language);
            container.appendChild(lineElement);
        });
        
        return container;
    }
}
```

### 👥 **Real-time Collaboration**
**Implementation**: WebSocket-based collaborative editing

```javascript
// htdocs/static/js/collaboration.js
class CollaborativeEditor {
    constructor(pasteId, userId) {
        this.pasteId = pasteId;
        this.userId = userId;
        this.socket = null;
        this.collaborators = new Map();
        this.operations = [];
    }
    
    connect() {
        // Use Socket.IO or native WebSockets
        this.socket = new WebSocket(`ws://${location.host}/ws/collaborate/${this.pasteId}`);
        
        this.socket.onopen = () => {
            this.socket.send(JSON.stringify({
                type: 'join',
                userId: this.userId,
                pasteId: this.pasteId
            }));
        };
        
        this.socket.onmessage = (event) => {
            const message = JSON.parse(event.data);
            this.handleMessage(message);
        };
    }
    
    handleMessage(message) {
        switch (message.type) {
            case 'user-joined':
                this.addCollaborator(message.user);
                break;
            case 'user-left':
                this.removeCollaborator(message.userId);
                break;
            case 'operation':
                this.applyOperation(message.operation);
                break;
            case 'cursor-moved':
                this.updateCollaboratorCursor(message.userId, message.position);
                break;
        }
    }
    
    sendOperation(operation) {
        if (this.socket && this.socket.readyState === WebSocket.OPEN) {
            this.socket.send(JSON.stringify({
                type: 'operation',
                operation: operation,
                userId: this.userId,
                pasteId: this.pasteId
            }));
        }
    }
    
    applyOperation(operation) {
        // Apply operational transform algorithm
        const editor = getActiveEditor();
        
        // Transform operation against pending operations
        const transformedOp = this.transformOperation(operation);
        
        // Apply to editor
        editor.getDoc().replaceRange(
            transformedOp.text,
            transformedOp.from,
            transformedOp.to,
            'collaboration'
        );
        
        // Update collaborator cursors
        this.updateCursors();
    }
}
```

### 📈 **Version History**
**Implementation**: Git-like versioning for paste changes

```php
// htdocs/application/models/Paste_version_model.php
<?php
class Paste_version_model extends CI_Model {
    
    public function create_version($paste_id, $content, $user_id = null, $message = '') {
        $previous_version = $this->get_latest_version($paste_id);
        $version_number = ($previous_version ? $previous_version->version + 1 : 1);
        
        // Generate diff from previous version
        $diff = null;
        if ($previous_version) {
            $diff = $this->generate_diff($previous_version->content, $content);
        }
        
        $version_data = [
            'paste_id' => $paste_id,
            'version' => $version_number,
            'content' => $content,
            'diff' => $diff,
            'user_id' => $user_id,
            'message' => $message,
            'created_at' => date('Y-m-d H:i:s'),
            'content_hash' => sha1($content)
        ];
        
        return $this->db->insert('paste_versions', $version_data);
    }
    
    public function get_version_history($paste_id, $limit = 50) {
        return $this->db
            ->where('paste_id', $paste_id)
            ->order_by('version', 'DESC')
            ->limit($limit)
            ->get('paste_versions')
            ->result();
    }
    
    public function restore_version($paste_id, $version_number) {
        $version = $this->db
            ->where('paste_id', $paste_id)
            ->where('version', $version_number)
            ->get('paste_versions')
            ->row();
            
        if ($version) {
            // Update main paste with version content
            $this->db
                ->where('id', $paste_id)
                ->update('pastes', ['data' => $version->content]);
                
            // Create new version entry for the restoration
            $this->create_version($paste_id, $version->content, null, "Restored to version {$version_number}");
            
            return true;
        }
        
        return false;
    }
}
```

These modern developer experience features would make Stiqued incredibly powerful for developers - combining the simplicity of a pastebin with the sophistication of modern IDE features!

### Phase 5 (Modern Developer Experience Extensions):
- [ ] **Real-time Syntax Validation**: Live error checking for code pastes
- [ ] **Auto-formatting & Code Beautification**: Automatically format pasted code
- [ ] **Git Integration**: Import/export from GitHub gists and repositories
- [ ] **Advanced Diff Views**: Side-by-side, inline, and semantic diff modes
- [ ] **Collaboration Mode**: Real-time multi-user editing with live cursors
- [ ] **Version History**: Track paste changes over time with git-like versioning
- [ ] **Smart Language Detection**: Auto-detect programming language with higher accuracy
- [ ] **Code Folding**: Collapse/expand functions, classes, and blocks
- [ ] **Intelligent Auto-complete**: Context-aware code suggestions
- [ ] **Performance**: Redis caching, CDN support for enhanced speed

## Notes for Claude Code Implementation

### Development Workflow:
1. **Start with Docker setup** - Essential for multi-version testing
2. **Incremental approach** - Fix compatibility issues gradually
3. **Test frequently** - After each major change
4. **Theme system first** - Establish CSS architecture early
5. **Mobile-first always** - Start with smallest screen, scale up

### Key Implementation Tips:
- **Preserve Visual Identity**: Each theme keeps its distinctive character and color schemes
- **Mobile-First Always**: Start with smallest screen, scale up for each theme individually
- **Default Theme Focus**: Use default theme as the template for modern theming architecture
- **No Visual Redesign**: Convert to responsive without changing the aesthetic appeal
- **Theme Isolation**: Keep theme-specific styles isolated and maintainable

### Common Pitfalls to Avoid:
- Don't break existing paste URLs
- Maintain config file compatibility where possible
- **Don't change theme visual aesthetics** - only make responsive
- **Preserve theme character** - i386 should still look terminal-like, geocities should stay retro
- Verify touch targets are adequate size across all themes
- Check that each theme's color schemes work well on mobile
- Test that unique theme features (like i386's terminal styling) work on touch devices

This modernization will transform Stiqued into a contemporary, mobile-first pastebin that works seamlessly across all modern PHP versions while **preserving each theme's unique visual identity and character**.
