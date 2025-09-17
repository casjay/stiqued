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

### 2. Mobile-First Responsive Theme System (Priority: High)
**Target**: All themes converted to mobile-first with modern dark/light mode

#### Current Theme Analysis:
```
/htdocs/themes/
├── default/           # Basic theme
├── bootstrap/         # Bootstrap 3.x based
├── gabdark/          # Dark theme (not responsive)
├── i386/             # Terminal-style theme
├── stikkedizr/       # Custom theme
├── cleanwhite/       # Clean light theme  
├── snowkat/          # Custom theme
└── geocities/        # Retro 90s theme
```

#### Modern Theme Implementation Strategy:

##### A. CSS Framework Upgrade:
- [ ] **Bootstrap 5.3+**: Upgrade from Bootstrap 3.x
  - Remove jQuery dependency (Bootstrap 5 is vanilla JS)
  - Update grid system (Bootstrap 5 uses CSS Grid + Flexbox)
  - Implement utility-first approach

##### B. Mobile-First Responsive Design:
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

##### C. Modern Dark/Light Theme System:
**Implementation**: CSS Custom Properties + `light-dark()` function (2024 standard)

```css
/* Modern Theme Variables */
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

/* Theme Toggle Support */
[data-theme="light"] {
  color-scheme: light;
}

[data-theme="dark"] {
  color-scheme: dark;
}

/* Apply Variables */
body {
  background-color: var(--bg-primary);
  color: var(--text-primary);
}

.paste-content {
  background-color: var(--bg-code);
  border: 1px solid var(--border-color);
}
```

##### D. Enhanced Mobile Features:
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

#### Week 2: Database & Security
- [ ] **Day 8-10**: Database Layer Modernization
  - Implement prepared statements
  - Update all database queries
  - Add connection error handling

- [ ] **Day 11-14**: Security Enhancements
  - Implement CSRF protection
  - Update password hashing
  - Review input sanitization
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

#### Week 4: Mobile-First Conversion
- [ ] **Day 22-24**: Responsive Base Layouts
  - Convert all themes to mobile-first
  - Implement responsive navigation
  - Update grid systems

- [ ] **Day 25-28**: Mobile Optimizations
  - Touch-friendly interfaces
  - Optimized syntax highlighting
  - Performance improvements
  - Accessibility enhancements

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

## File Structure (Modernized)

```
/stiqued-modern/
├── application/
│   ├── config/
│   │   ├── stiqued.php          # Main config
│   │   ├── database.php         # DB config with mysqli
│   │   └── routes.php           # URL routing
│   ├── controllers/
│   │   ├── Main.php             # Main controller
│   │   ├── Api.php              # API endpoints
│   │   └── Admin.php            # Admin interface
│   ├── models/
│   │   ├── Paste_model.php      # Paste operations
│   │   └── User_model.php       # User management
│   ├── views/
│   │   ├── templates/           # Base templates
│   │   └── themes/              # Theme-specific views
│   └── libraries/
│       ├── Carabiner.php        # Fixed for PHP 8+
│       └── Encryption.php       # Updated encryption
├── htdocs/
│   ├── themes/
│   │   ├── default/
│   │   │   ├── css/
│   │   │   │   ├── main.css     # Mobile-first styles
│   │   │   │   ├── dark.css     # Dark theme variables
│   │   │   │   └── light.css    # Light theme variables
│   │   │   ├── js/
│   │   │   │   ├── theme-toggle.js  # Theme switching
│   │   │   │   └── mobile.js        # Mobile enhancements
│   │   │   └── views/           # Theme templates
│   │   ├── bootstrap/           # Bootstrap 5 based
│   │   ├── gabdark/            # Modernized dark theme
│   │   ├── i386/               # Terminal theme (responsive)
│   │   ├── stikkedizr/         # Custom responsive theme
│   │   ├── cleanwhite/         # Clean light theme
│   │   ├── snowkat/            # Custom responsive theme
│   │   └── geocities/          # Retro theme (responsive)
│   ├── assets/
│   │   ├── css/
│   │   │   ├── bootstrap.min.css    # Bootstrap 5
│   │   │   ├── themes.css           # Global theme system
│   │   │   └── mobile.css           # Mobile optimizations
│   │   ├── js/
│   │   │   ├── bootstrap.bundle.min.js  # Bootstrap 5 JS
│   │   │   ├── highlight.min.js         # Syntax highlighting
│   │   │   └── app.js                   # Main application JS
│   │   └── img/
│   └── index.php                # Entry point
├── docker/
│   ├── php7.0/Dockerfile
│   ├── php7.1/Dockerfile
│   ├── php7.2/Dockerfile
│   ├── php7.3/Dockerfile
│   ├── php7.4/Dockerfile
│   ├── php8.0/Dockerfile
│   ├── php8.1/Dockerfile
│   ├── php8.2/Dockerfile
│   ├── php8.3/Dockerfile
│   └── nginx.conf
├── tests/
│   ├── php-compatibility/       # PHP version tests
│   ├── responsive/              # Mobile/responsive tests
│   └── themes/                  # Theme functionality tests
├── docs/
│   ├── INSTALLATION.md
│   ├── THEMING.md
│   ├── API.md
│   └── MIGRATION.md
├── docker-compose.yml
├── test-all-versions.sh
└── README.md
```

## Success Criteria

### Functional Requirements:
- [ ] All features work flawlessly on PHP 7.0 through 8.3
- [ ] All themes are fully responsive and mobile-optimized
- [ ] Dark/Light mode toggle works across all themes
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

## Additional Features for Future Consideration

### Phase 5 (Optional Extensions):
- [ ] **API v2**: RESTful API with authentication
- [ ] **Plugin System**: Extensible architecture
- [ ] **Advanced Themes**: More theme options
- [ ] **Performance**: Redis caching, CDN support
- [ ] **Analytics**: Usage statistics and monitoring
- [ ] **Social Features**: User profiles, favorites
- [ ] **Advanced Security**: 2FA, rate limiting
- [ ] **Mobile App**: PWA or native app
- [ ] **Collaboration**: Real-time editing, comments

## Notes for Claude Code Implementation

### Development Workflow:
1. **Start with Docker setup** - Essential for multi-version testing
2. **Incremental approach** - Fix compatibility issues gradually
3. **Test frequently** - After each major change
4. **Theme system first** - Establish CSS architecture early
5. **Mobile-first always** - Start with smallest screen, scale up

### Key Implementation Tips:
- Use feature detection over browser detection
- Implement progressive enhancement
- Prioritize accessibility throughout
- Test on real devices, not just browser dev tools
- Consider offline functionality for core features
- Optimize for slow connections (3G)

### Common Pitfalls to Avoid:
- Don't break existing paste URLs
- Maintain config file compatibility where possible
- Ensure theme switching doesn't cause layout shifts
- Test dark mode in actual dark environments
- Verify touch targets are adequate size
- Check color contrast ratios in all themes

This modernization will transform Stiqued into a contemporary, mobile-first pastebin that works seamlessly across all modern PHP versions while maintaining its core functionality and extending its capabilities for today's web standards.

