# Club420 Cannabis Compliance & Location System

> **Professional WordPress + Tymber Integration for Cannabis Retailers**

![System Status](https://img.shields.io/badge/Status-Production%20Ready-brightgreen)
![Cannabis Compliance](https://img.shields.io/badge/Cannabis-Compliant-green)
![WordPress](https://img.shields.io/badge/WordPress-Compatible-blue)
![Mobile Responsive](https://img.shields.io/badge/Mobile-Responsive-orange)

## Overview

The Club420 system provides cannabis-compliant age verification and seamless store location switching for WordPress marketing sites integrated with Tymber (BLAZE) ecommerce platforms.

### Key Features

- 🍃 **Cannabis Age Gate** - Legal 21+ compliance with professional modal design
- 📍 **Location Picker** - Desktop header + mobile menu transformation  
- 🔗 **Cross-System State** - Seamless WordPress ↔ Tymber integration
- 🎯 **Content Visibility** - Store-specific content management
- 🤖 **Bot Detection** - Performance testing tool compatibility
- 📱 **Mobile Responsive** - Complete mobile experience

## System Architecture

```
User Journey:
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│   WordPress     │    │   Age Gate +     │    │   Tymber Menu   │
│   club420.com   │ →  │   Store Picker   │ →  │   /menu/...     │
│                 │    │                  │    │                 │
└─────────────────┘    └──────────────────┘    └─────────────────┘
        ↓                        ↓                        ↓
   Marketing Site          Cannabis Compliance        E-commerce
   Content/SEO             + Store Selection          Real Inventory
```

## Documentation

### Core System Components
- [🍃 Cannabis Compliance System](docs/cannabis-compliance.md) - Age gate, bot detection, legal compliance
- [📍 Location Picker System](docs/location-picker-system.md) - Desktop + mobile location selection
- [⚙️ Backend Integration](docs/backend-integration.md) - PHP snippets for store filtering & scheduling

### Implementation & Setup  
- [🚀 Implementation Guide](docs/implementation-guide.md) - Complete step-by-step setup
- [🔧 Troubleshooting](docs/troubleshooting.md) - Common issues and solutions

## Quick Start

### 1. Install Core JavaScript
Add to **Divi → Theme Options → Integration → Body**:
```html
<!-- Complete cannabis compliance + location picker system -->
[See Implementation Guide for full code]
```

### 2. WordPress Menu Setup
Create menu structure:
```
LOCATIONS (parent, dt-hide-on-desktop class)
├── Davis (sub-item, dt-hide-on-desktop class)  
└── Dixon (sub-item, dt-hide-on-desktop class)
```

### 3. Backend PHP Setup
Install core store filtering via **Code Snippets** plugin:
```php
// Store Filter + Cache Exclusion snippet
[See Backend Integration docs]
```

### 4. Tymber Proxy Configuration
Configure hosting proxy pass:
```nginx
location /menu/f-street/ { proxy_pass https://tymber-davis-url.com/; }
location /menu/highway-80/ { proxy_pass https://tymber-dixon-url.com/; }
```

[📖 **Full Implementation Guide →**](docs/implementation-guide.md)

## Repository Structure

```
CLUB420-DEV/
├── README.md                          # This overview
├── docs/                              # Detailed documentation
│   ├── cannabis-compliance.md         # Age gate system
│   ├── location-picker-system.md      # Location picker implementation  
│   ├── backend-integration.md         # PHP snippets (6 total)
│   ├── implementation-guide.md        # Step-by-step setup
│   └── troubleshooting.md            # Issues & solutions
├── src/                               # Source code
│   ├── php-snippets/                 # Backend PHP files
│   ├── css/                          # Styling (mobile menu)
│   └── javascript/                   # Frontend JavaScript
└── examples/                         # Usage examples
```

## Cannabis Industry Context

### Why This System Exists
Cannabis retailers face unique challenges:
- **Legal Compliance**: Age verification mandatory by law
- **Multi-System Architecture**: WordPress marketing + specialized ecommerce
- **User Experience**: Seamless navigation without repeated verification

### The BLAZE/Tymber Solution
- **BLAZE** acquired **Tymber** in January 2023 to solve iframe menu problems
- Provides native domain integration via proxy pass
- Our system bridges WordPress ↔ Tymber seamlessly

## Store Configuration

```javascript
const STORE_CONFIG = {
  davis: {
    id: '79043044-f024-4b70-8714-4fcad409f978',
    tymberPath: '/menu/f-street/',
    displayName: 'Davis'
  },
  dixon: {
    id: '7029749f-9c6d-419e-b037-5c1b566f3df9', 
    tymberPath: '/menu/highway-80/',
    displayName: 'Dixon'
  }
};
```

## Requirements

- **WordPress** with Divi Theme
- **Code Snippets** plugin for PHP backend
- **Cannabis retail license** (legal compliance)
- **Tymber/BLAZE** ecommerce integration
- **Hosting** with proxy pass capability

## Browser Support

- ✅ Chrome 70+
- ✅ Firefox 70+  
- ✅ Safari 12+
- ✅ Edge 79+
- ⚠️ IE 11 (limited support)

## Security & Compliance

### Cannabis Compliance
- ✅ Legal age verification (21+)
- ✅ Cross-system state persistence
- ✅ Bot detection for performance testing
- ✅ Privacy-compliant localStorage usage

### Security Features
- ✅ Input sanitization
- ✅ XSS protection
- ✅ No sensitive data storage
- ✅ Cannabis regulation compliance

## Contributing

This system is designed for licensed cannabis retailers. When contributing:

1. **Test thoroughly** in staging environment
2. **Maintain compliance** with cannabis regulations
3. **Document changes** comprehensively
4. **Verify cross-browser** compatibility

## License

This system is designed for cannabis compliance and should only be used by licensed cannabis retailers in legal jurisdictions.

---

## Support

For implementation support or questions:
- 📖 Check the [Implementation Guide](docs/implementation-guide.md)
- 🔧 Review [Troubleshooting](docs/troubleshooting.md)
- 💬 Open an issue for bugs or feature requests

---

**🍃 Club420: Complete Cannabis Compliance Solution - Production Ready! 📱💻**
