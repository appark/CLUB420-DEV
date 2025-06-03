# Club420 Carousel Store Filter

**Professional WooCommerce store filtering system with smooth transitions and age gate integration**

![Project Status](https://img.shields.io/badge/Status-Production%20Ready-brightgreen)
![WordPress](https://img.shields.io/badge/WordPress-Compatible-blue)
![WooCommerce](https://img.shields.io/badge/WooCommerce-Compatible-purple)
![Divi](https://img.shields.io/badge/Divi%20Theme-Compatible-orange)

## Overview

This project transforms a clunky WooCommerce carousel filtering system into a smooth, professional experience. Originally a 3-day debugging challenge, now a complete solution with beautiful transitions, age verification, and database-level filtering.

### Key Features

- Age Gate & Store Picker - Professional modal system
- Database-Level Filtering - Only queries relevant products
- Smooth Transitions - Custom branded animations 
- Clean Interface - No visual clutter
- Responsive Design - Works on all devices
- Divi Builder Compatible - Silent editing mode detection
- Future-Proof - Uses WordPress hooks, survives plugin updates

### Before vs After

**Before**: Jarky page reloads, all products showing regardless of store selection  
**After**: Smooth transitions, perfect filtering, professional UX

## Quick Start

### Prerequisites
- WordPress with WooCommerce
- Divi Theme
- Code snippet manager (or functions.php access)

### Installation

1. **Add PHP Snippets** (see `/CODE/php-snippets/`)
   - `custom-fields.php` - Adds store URL fields to products
   - `query-filter.php` - Filters WooCommerce queries by store

2. **Add Divi Body JavaScript** (see `/CODE/divi-body-javascript.html`)
   - Go to: Divi → Theme Options → Integration → Body
   - Paste the complete code

3. **Configure Products**
   - Edit each product in WooCommerce
   - Add Davis/Dixon store URLs in the new custom fields

4. **Test**
   - Visit your site
   - Go through age gate → store picker
   - Switch stores and verify smooth filtering

## Architecture

```
User selects store → localStorage updated → Page reloads with ?store_filter=davis/dixon 
→ WordPress hook filters WooCommerce query → Only relevant products load in carousel
→ Smooth transitions cover the reload process
```

### Store Mapping
- **Davis Store**: `79043044-f024-4b70-8714-4fcad409f978` → `f-street` → `?store_filter=davis`
- **Dixon Store**: `7029749f-9c6d-419e-b037-5c1b566f3df9` → `highway-80` → `?store_filter=dixon`

### Integration Points
- Tymber Menu System - Seamless integration
- Age Gate - `tymber-user-has-allowed-age` localStorage
- Store Selection - `last-store-selected` localStorage
- WooCommerce - Custom fields `_club420_davis_url`, `_club420_dixon_url`

## Technical Details

### Components

1. **PHP Backend** (WordPress Hooks)
   - Custom fields for product store URLs
   - WooCommerce query filtering by store parameter

2. **JavaScript Frontend** (Divi Body)
   - Age gate and store picker modals
   - Smooth transition animations with custom logos
   - Content visibility controls
   - Auto-redirect for direct URL access

3. **CSS Styling**
   - Professional modal designs
   - Responsive animations
   - Clean, modern aesthetics

### Smart Detection Features
- **Divi Builder Mode** - Shows all sections when editing
- **WordPress Admin Bar** - Prevents white space gaps
- **Auto-Redirect** - Handles direct URL access without parameters

## Customization

### Logo Replacement
Update these URLs in the JavaScript:
```html
<img src="YOUR_DESKTOP_LOGO_URL" class="logo-desktop">
<img src="YOUR_MOBILE_LOGO_URL" class="logo-mobile">
```

### Store Configuration
Update store IDs and mappings in the JavaScript:
```javascript
const davisID = 'YOUR_DAVIS_STORE_ID';
const dixonID = 'YOUR_DIXON_STORE_ID';
```

### Styling
All CSS is included in the main code. Modify colors, animations, and layouts as needed.

## Troubleshooting

### Common Issues

**Age gate not appearing?**
- Check localStorage: `tymber-user-has-allowed-age`
- Verify code is in Divi Body section

**Products not filtering?**
- Confirm PHP snippets are active
- Check custom fields have store URLs
- Verify URL parameter is being added

**White space above admin bar?**
- Code includes WordPress admin bar detection
- Should automatically prevent white gaps

**All sections visible in Divi Builder?**
- This is intentional for editing
- Code detects builder mode automatically

## Success Metrics

- Original 3-day problem: SOLVED
- Database-level filtering: IMPLEMENTED
- Zero conflicts: Perfect integration
- Professional UX: Smooth transitions
- Clean interface: No visual clutter
- Future-proof: Plugin-update safe

**Total Implementation Time**: ~4 hours  
**Plugin Files Modified**: 0 (future-proof!)  
**Systems Integrated**: Age Gate + Store Picker + Tymber Menus + WooCommerce + Divi Carousels

## Documentation

- Implementation Guide (DOCS/implementation-guide.md)
- Troubleshooting (DOCS/troubleshooting.md) 
- Changelog (DOCS/changelog.md)

## Contributing

Found a bug or have an improvement? Feel free to:
1. Open an issue
2. Submit a pull request
3. Share your implementation story

## License

This project is open source. Feel free to use, modify, and distribute.

## Acknowledgments

- **Problem Duration**: Originally 3 days of frustration
- **Solution Time**: 4 hours of focused development
- **Result**: Production-ready professional system

---

**Status**: COMPLETE & PRODUCTION READY

*Transforms WooCommerce carousel filtering from clunky to professional with smooth transitions, age verification, and database-level filtering.*
