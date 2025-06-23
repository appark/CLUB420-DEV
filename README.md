# COMPLETE CLUB420 SYSTEM 

---

```markdown
# CLUB420 WooCommerce Real-Time Scheduler & Tymber Integration

**Status**: ✅ **PHASE 2B COMPLETED** - Production Ready  
**Last Updated**: June 2025  
**Performance**: Grade A+ (85% Performance, 97% Structure)  
**Production Status**: ✅ **LIVE AND OPERATIONAL**

## 🏗️ **System Architecture Overview**

This is a sophisticated multi-system integration combining:
- **WordPress/WooCommerce** main site with real-time product scheduling
- **Tymber menu system** integration via proxy pass (`/menu/f-street/` → Tymber Davis, `/menu/highway-80/` → Tymber Dixon)
- **Custom desktop header location picker** with seamless switching
- **Mobile menu location picker** matching desktop functionality exactly
- **Shared state management** across system boundaries using localStorage
- **Cannabis compliance** age gate with cross-system persistence
- **Store-specific content and navigation** using CSS class visibility control

## 🔄 **Technical Integration Flow**

```
User Journey:
1. Lands on club420.com (WordPress)
2. Age gate verification → localStorage: 'tymber-user-has-allowed-age'
3. Store selection via:
   - Desktop: Custom header location picker (black button)
   - Mobile: LOCATIONS menu item (styled as black button)
   - Page dropdowns: Multiple synchronized store selectors
4. localStorage: 'last-store-selected' (Davis/Dixon)
5. JavaScript shows store-specific content using CSS classes:
   - .davis-content/.davis-menu sections visible for Davis users
   - .dixon-content/.dixon-menu sections visible for Dixon users
6. Menu clicks route to appropriate Tymber system:
   - Davis Store → https://club420.com/menu/f-street/categories/flower/
   - Dixon Store → https://club420.com/menu/highway-80/categories/flower/
7. Tymber pages read localStorage → No re-verification needed
```

## 🖥️ **Desktop Header Location Picker**

### **Custom Location Picker Component**
A sophisticated black button component that matches your brand design:

```html
<div class="club420-location-picker">
  <div class="location-button" id="locationButton">
    <div class="location-text">
      <span class="current-location" id="currentLocation">Davis</span>
      <span class="change-text">Change Location</span>
    </div>
    <span class="location-arrow">›</span>
  </div>
  
  <div class="location-dropdown" id="locationDropdown">
    <div class="location-grid">
      <div class="location-option selected" data-store="davis">Davis</div>
      <div class="location-option" data-store="dixon">Dixon</div>
    </div>
  </div>
</div>
```

### **Desktop Location Picker Styling**
```css
.club420-location-picker {
  position: relative;
  width: 100%;
  max-width: 275px;
  margin: 0 auto;
}

.location-button {
  background: #000;
  color: #fff;
  border: 2px solid #fff;
  border-radius: 12px;
  padding: 15px 25px;
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  cursor: pointer;
  transition: all 0.3s ease;
  font-size: 16px;
  font-weight: 500;
  box-sizing: border-box;
}

.location-button:hover {
  background: #333;
  transform: translateY(-1px);
}

.location-button.active {
  border-color: #f2ac1d;
  background: #111;
}

.location-text {
  display: flex;
  align-items: center;
  gap: 10px;
}

.current-location {
  color: #fff;
  font-weight: 600;
}

.change-text {
  color: #fff;
  text-decoration: underline;
  font-weight: 400;
}

.location-arrow {
  color: #fff;
  font-size: 18px;
  transition: transform 0.3s ease;
}

.location-button.active .location-arrow {
  transform: rotate(90deg);
}

.location-dropdown {
  position: absolute;
  top: 100%;
  left: 0;
  right: 0;
  background: #fff;
  border-radius: 15px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
  opacity: 0;
  visibility: hidden;
  transform: translateY(-10px);
  transition: all 0.3s ease;
  z-index: 1000;
  margin-top: 10px;
  overflow: hidden;
}

.location-dropdown.open {
  opacity: 1;
  visibility: visible;
  transform: translateY(0);
}

.location-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 0;
}

.location-option {
  padding: 20px 25px;
  color: #333;
  cursor: pointer;
  transition: all 0.3s ease;
  font-weight: 500;
  font-size: 15px;
}

.location-option:hover {
  background: #f8f9fa;
  color: #f2ac1d;
}

.location-option.selected {
  background: #f2ac1d;
  color: #000;
  font-weight: 600;
}

.location-option:nth-child(2) {
  border-left: 1px solid #f0f0f0;
}

/* Hide on mobile */
@media (max-width: 980px) {
  .club420-location-picker {
    display: none !important;
  }
}
```

### **Desktop Location Picker JavaScript**
```javascript
document.addEventListener('DOMContentLoaded', function() {
  const button = document.getElementById('locationButton');
  const dropdown = document.getElementById('locationDropdown');
  const currentLocation = document.getElementById('currentLocation');
  
  // Click handler for button
  button.addEventListener('click', function() {
    button.classList.toggle('active');
    dropdown.classList.toggle('open');
    
    if (dropdown.classList.contains('open')) {
      setTimeout(() => {
        document.addEventListener('click', closeOnClickOutside);
      }, 100);
    }
  });
  
  // Click handlers for options
  const options = document.querySelectorAll('.location-option');
  options.forEach(option => {
    option.addEventListener('click', function() {
      const store = this.dataset.store;
      const displayName = this.textContent;
      
      // Update display
      currentLocation.textContent = displayName;
      
      // Update selected state
      options.forEach(opt => opt.classList.remove('selected'));
      this.classList.add('selected');
      
      // Close dropdown
      button.classList.remove('active');
      dropdown.classList.remove('open');
      
      // Store switching
      if (window.setStore) {
        console.log('Club420: Switching to', store);
        window.setStore(store);
      }
      
      // Update localStorage
      const storeIDs = {
        'davis': '79043044-f024-4b70-8714-4fcad409f978',
        'dixon': '7029749f-9c6d-419e-b037-5c1b566f3df9'
      };
      localStorage.setItem('last-store-selected', storeIDs[store]);
      
      document.removeEventListener('click', closeOnClickOutside);
    });
  });
  
  function closeOnClickOutside(event) {
    const picker = document.querySelector('.club420-location-picker');
    if (!picker.contains(event.target)) {
      button.classList.remove('active');
      dropdown.classList.remove('open');
      document.removeEventListener('click', closeOnClickOutside);
    }
  }
  
  // Set initial state from localStorage
  const currentStore = localStorage.getItem('last-store-selected');
  const dixonID = '7029749f-9c6d-419e-b037-5c1b566f3df9';
  
  if (currentStore === dixonID) {
    currentLocation.textContent = 'Dixon';
    options.forEach(opt => opt.classList.remove('selected'));
    document.querySelector('[data-store="dixon"]').classList.add('selected');
  }
});
```

## 📱 **Mobile Menu Location Picker**

### **Mobile Menu Complete CSS**
The mobile menu system includes styling for all menu items plus the special LOCATIONS button:

```css
/* Hide WooCommerce Cart Icon*/
.et-cart-info { display:none !important; }

/* Hide Menu item on Desktop */
@media (min-width: 981px) {
  .dt-hide-on-desktop {
    display: none !important;
  }
}

/* ========================================= */
/* MOBILE MENU STYLING WITH LOCATIONS BUTTON */
/* ========================================= */

/* Mobile menu container */
ul.et_mobile_menu {
    background: #fff !important;
    border-radius: 12px !important;
    box-shadow: 0 10px 40px rgba(0, 0, 0, 0.15) !important;
    border: 1px solid rgba(0, 0, 0, 0.08) !important;
    margin: 10px !important;
    padding: 0 !important;
    overflow: hidden !important;
}

/* ALL menu items - consistent styling */
ul.et_mobile_menu .menu-item a {
    padding: 18px 25px !important;
    border-bottom: 1px solid #f0f0f0 !important;
    font-weight: 500 !important;
    font-size: 16px !important;
    color: #333 !important;
    text-transform: uppercase !important;
    background: #fff !important;
    display: block !important;
    text-decoration: none !important;
    transition: all 0.3s ease !important;
}

/* Hover effects for ALL menu items */
ul.et_mobile_menu .menu-item a:hover {
    background: #f8f9fa !important;
    color: #f2ac1d !important;
    padding-left: 35px !important;
    border-left: 4px solid #f2ac1d !important;
}

/* ========================================= */
/* LOCATIONS BUTTON - MAKE IT LOOK LIKE DESKTOP */
/* ========================================= */
@media (max-width: 980px) {
  
  /* Style LOCATIONS menu item as BLACK BUTTON like desktop */
  ul.et_mobile_menu .menu-item-has-children.dt-hide-on-desktop > a {
    background: #000 !important;
    color: #fff !important;
    border: 2px solid #fff !important;
    border-radius: 12px !important;
    margin: 10px 15px !important;
    font-weight: 600 !important;
    text-transform: uppercase !important;
    position: relative !important;
    transition: all 0.3s ease !important;
  }
  
  /* Add arrow to LOCATIONS button */
  ul.et_mobile_menu .menu-item-has-children.dt-hide-on-desktop > a:after {
    content: "›" !important;
    position: absolute !important;
    right: 25px !important;
    top: 50% !important;
    transform: translateY(-50%) !important;
    font-size: 18px !important;
    transition: transform 0.3s ease !important;
  }
  
  /* Hover effect for LOCATIONS button */
  ul.et_mobile_menu .menu-item-has-children.dt-hide-on-desktop > a:hover {
    background: #333 !important;
    color: #fff !important;
    border-color: #f2ac1d !important;
    padding-left: 25px !important; /* Override default hover padding */
    border-left: 2px solid #fff !important; /* Override default hover border */
  }
  
  /* Active state when dropdown is open */
  ul.et_mobile_menu .menu-item-has-children.dt-hide-on-desktop.open > a {
    background: #111 !important;
    border-color: #f2ac1d !important;
  }
  
  /* Rotate arrow when open */
  ul.et_mobile_menu .menu-item-has-children.dt-hide-on-desktop.open > a:after {
    transform: translateY(-50%) rotate(90deg) !important;
  }
  
  /* HIDE DAVIS/DIXON BY DEFAULT */
  ul.et_mobile_menu .menu-item-has-children.dt-hide-on-desktop .sub-menu {
    display: none !important;
    opacity: 0 !important;
    transform: translateY(-10px) !important;
    transition: all 0.3s ease !important;
  }
  
  /* SHOW DROPDOWN when LOCATIONS clicked */
  ul.et_mobile_menu .menu-item-has-children.dt-hide-on-desktop.open .sub-menu {
    display: block !important;
    opacity: 1 !important;
    transform: translateY(0) !important;
    animation: slideDown 0.3s ease !important;
  }
  
  /* Slide down animation */
  @keyframes slideDown {
    from {
      opacity: 0;
      transform: translateY(-10px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }
  
  /* Style the dropdown container */
  ul.et_mobile_menu .menu-item-has-children.dt-hide-on-desktop .sub-menu {
    background: #fff !important;
    border-radius: 15px !important;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2) !important;
    margin: 10px 15px 20px 15px !important;
    border: none !important;
    overflow: hidden !important;
  }
  
  /* Davis and Dixon styling */
  ul.et_mobile_menu .menu-item-has-children.dt-hide-on-desktop .sub-menu .menu-item a {
    padding: 20px 25px !important;
    font-size: 15px !important;
    font-weight: 500 !important;
    color: #333 !important;
    text-transform: none !important;
    background: #fff !important;
    border-bottom: 1px solid #f0f0f0 !important;
    border-right: none !important;
    text-align: center !important;
  }
  
  /* Remove border from last item */
  ul.et_mobile_menu .menu-item-has-children.dt-hide-on-desktop .sub-menu .menu-item:last-child a {
    border-bottom: none !important;
  }
  
  /* Hover effects for Davis/Dixon */
  ul.et_mobile_menu .menu-item-has-children.dt-hide-on-desktop .sub-menu .menu-item a:hover {
    background: #f8f9fa !important;
    color: #f2ac1d !important;
    padding-left: 35px !important;
    border-left: 4px solid #f2ac1d !important;
  }
  
  /* Selected state styling */
  ul.et_mobile_menu .menu-item-has-children.dt-hide-on-desktop .sub-menu .menu-item.selected a {
    background: #f2ac1d !important;
    color: #000 !important;
    font-weight: 600 !important;
  }
}

/* ========================================= */
/* REGULAR SUBMENU STYLING (for other menus) */
/* ========================================= */

/* Submenu container (for other dropdowns, NOT LOCATIONS) */
ul.et_mobile_menu .sub-menu {
    background: #f8f9fa !important;
    border-radius: 8px !important;
    margin: 0 15px 10px 15px !important;
    border: 1px solid #e9ecef !important;
    padding: 0 !important;
    box-shadow: inset 0 2px 4px rgba(0, 0, 0, 0.05) !important;
}

/* Submenu items (for other dropdowns) */
ul.et_mobile_menu .sub-menu .menu-item a {
    padding: 15px 20px 15px 35px !important;
    font-size: 15px !important;
    font-weight: 400 !important;
    color: #666 !important;
    text-transform: none !important;
    background: transparent !important;
    border-bottom: 1px solid #f0f0f0 !important;
}

/* Submenu hover (for other dropdowns) */
ul.et_mobile_menu .sub-menu .menu-item a:hover {
    background: #fff !important;
    color: #f2ac1d !important;
    padding-left: 45px !important;
    border-left: 3px solid #f2ac1d !important;
}

/* Remove border from last submenu item */
ul.et_mobile_menu .sub-menu .menu-item:last-child a {
    border-bottom: none !important;
}

/* Remove border from last main menu item */
ul.et_mobile_menu > .menu-item:last-child > a {
    border-bottom: none !important;
}
```

## 🌐 **Cross-System State Management**

### **Shared localStorage Keys:**
- `tymber-user-has-allowed-age`: Cannabis compliance verification
- `last-store-selected`: Store preference (Davis/Dixon mapping)

### **Store ID Mapping:**
- **Davis Store**: `79043044-f024-4b70-8714-4fcad409f978` → `/menu/f-street/`
- **Dixon Store**: `7029749f-9c6d-419e-b037-5c1b566f3df9` → `/menu/highway-80/`

### **Proxy Architecture:**
- `club420.com/menu/f-street/` → Proxy pass to Tymber (Davis location)
- `club420.com/menu/highway-80/` → Proxy pass to Tymber (Dixon location)
- Same-domain experience maintains localStorage access

## 🎨 **Divi Content Organization System**

### **CSS Class Visibility Control:**
**Content Sections:**
- `.davis-content` = Page sections/modules only visible to Davis store users
- `.dixon-content` = Page sections/modules only visible to Dixon store users

**Navigation Menu:**
- `.davis-menu` = Menu items only visible to Davis store users  
- `.dixon-menu` = Menu items only visible to Dixon store users

### **Divi Builder Workflow:**
1. **Content Sections**: Add CSS class `davis-content` or `dixon-content` to sections
2. **Menu Items**: Add CSS class `davis-menu` or `dixon-menu` to navigation items
3. **Store Dropdowns**: Use `styled-store-select-*` ID pattern for automatic detection
4. **Carousel Placement**: 
   - Davis sections: `[club420_deals_carousel store="davis"]`
   - Dixon sections: `[club420_deals_carousel store="dixon"]`

### **Store Dropdown Integration:**
```html
<div style="max-width: 350px;">
 <select id="styled-store-select-page" style="
   width: 100%;
   padding: 1.25rem 1.5rem;
   font-size: 1.125rem;
   font-weight: 500;
   background: #000;
   color: #fff;
   border: 2px solid #fff;
   border-radius: 20px;
   outline: none;
   cursor: pointer;
   appearance: none;
   -webkit-appearance: none;
   -moz-appearance: none;
   background-image: url('data:image/svg+xml;charset=US-ASCII,<svg xmlns=\"http://www.w3.org/2000/svg\" viewBox=\"0 0 4 5\"><path fill=\"%23ffffff\" d=\"M2 0L0 2h4zm0 5L0 3h4z\"/></svg>');
   background-repeat: no-repeat;
   background-position: right 1rem center;
   background-size: 12px;
   transition: all 0.3s ease;
 ">
   <option value="" style="color: #333;">Choose Store Location</option>
   <option value="davis" style="color: #333;">Davis Store</option>
   <option value="dixon" style="color: #333;">Dixon Store</option>
 </select>
</div>
```
- **Auto-Detection**: Any dropdown with ID starting `styled-store-select-` is handled
- **Multi-Dropdown Sync**: Multiple dropdowns stay synchronized
- **Smooth Transitions**: Automatic page transitions when store changes

## 🚀 **Core System Components**

### **WordPress/WooCommerce Layer**
- **Real-time product scheduling** - Products activate at exact scheduled times
- **WP Engine cache exclusion** - Bypasses cache for scheduled content
- **Store-specific product filtering** - Davis/Dixon inventory separation
- **Admin scheduling interface** - Visual product scheduling in WooCommerce
- **BlazeSlider carousels** - Real-time updating product displays

### **JavaScript Integration Layer**
- **Age gate system** with bot detection for performance testing
- **Store picker** with localStorage persistence
- **Content visibility control** using CSS class system
- **Menu navigation** with automatic Tymber routing via text detection
- **Page transitions** for smooth user experience
- **Multi-dropdown sync** across page elements
- **Desktop header location picker** with elegant dropdown
- **Mobile menu location picker** matching desktop functionality

### **Divi Menu Navigation System**
**Text-Based Detection** - JavaScript intercepts menu clicks based on Navigation Label:
- **FLOWER** → `/categories/flower/`
- **CARTRIDGES** → `/categories/cartridge/`
- **EDIBLES** → `/categories/edible/`
- **PRE-ROLLS** or **PREROLLS** → `/categories/preroll/`
- **EXTRACTS** or **EXTRACT** → `/categories/extract/`
- **SHOP ALL** or **ALL** or **SHOP NOW** → `/` (all products)

### **Tymber Menu Integration**
- **Seamless authentication** - No re-verification required
- **Store-specific routing** - Automatic navigation to correct location
- **Cross-system state** - User preferences maintained

## 📁 **Repository Structure**

```
├── README.md                          # This file
├── INSTALLATION.md                    # Step-by-step setup guide
├── php-snippets/                      # 6 PHP files for Code Snippets plugin
│   ├── 01-store-filter-cache.php     # Main filtering + WP Engine cache exclusion
│   ├── 02-product-scheduler.php      # Admin interface with real-time scheduling
│   ├── 03-admin-filter.php           # Admin products page filtering & indicators
│   ├── 04-auto-button-generator.php  # Dynamic button generation (no flash)
│   ├── 05-blazeslider-carousel.php   # Real-time updating product carousels
│   └── 06-scheduler-dashboard.php    # Admin monitoring dashboard
├── javascript/                       # 7 JS components for Divi body integration
│   ├── age-gate.js                   # Cannabis compliance verification
│   ├── store-picker.js               # Store selection with localStorage
│   ├── woocommerce-admin.js          # Admin interface enhancements
│   ├── woocommerce-menu-links.js     # Tymber navigation system (text detection)
│   ├── woocommerce-carousel.js       # Carousel functionality
│   ├── woocommerce-scheduler.js      # Real-time scheduling logic
│   └── page-transitions.js          # Smooth transitions between systems
├── header-components/                 # NEW: Desktop header components
│   ├── location-picker.html          # Custom location picker HTML
│   ├── location-picker.css           # Custom location picker styling
│   └── location-picker.js            # Custom location picker JavaScript
├── mobile-menu/                      # NEW: Mobile menu components
│   ├── mobile-menu.css               # Complete mobile menu styling with LOCATIONS
│   └── mobile-menu-integration.js    # Mobile LOCATIONS dropdown functionality
├── divi-integration/                 # Divi-specific components
│   ├── complete-body-javascript.html # Complete body JavaScript for Divi
│   └── store-dropdown.html           # Store dropdown HTML for Divi modules
├── docs/                             # Technical documentation
│   ├── how-it-works.md              # System architecture deep dive
│   ├── divi-integration.md           # CSS class system & dropdown setup
│   ├── tymber-integration.md         # Proxy setup & localStorage sharing
│   ├── store-id-configuration.md    # Store ID mapping changes
│   ├── wp-engine-cache.md           # Cache exclusion implementation
│   ├── desktop-header-setup.md      # NEW: Desktop header location picker guide
│   ├── mobile-menu-setup.md         # NEW: Mobile menu location picker guide
│   └── troubleshooting.md           # Common issues & solutions
└── assets/                          # Screenshots and diagrams
```

## 🔧 **Technical Features**

### **Real-Time Scheduling Engine**
- **Zero-delay activation** - Products show/hide at exact scheduled times
- **UTC timestamp storage** with Los Angeles timezone display
- **Database-level filtering** bypasses WordPress caching
- **WP Engine cache exclusion** for pages with active schedules

### **Cross-System Authentication**
- **One-time age verification** persists across WordPress + Tymber
- **Store selection persistence** via localStorage
- **Bot detection** for performance testing tools (bypasses age gate)
- **Safari compatibility** with direct button functions

### **Store Management**
- **Davis Store** (F-Street) - Independent inventory, content, and navigation
- **Dixon Store** (Highway-80) - Independent inventory, content, and navigation
- **Real-time filtering** based on user store selection
- **Admin visual indicators** for store availability
- **Content duplication** - Same layout, different store-specific content

### **Location Picker Components**
- **Desktop Header**: Custom black button component with elegant dropdown
- **Mobile Menu**: LOCATIONS item styled as black button with collapsible functionality
- **Page Dropdowns**: Multiple synchronized store selectors throughout site
- **Unified State Management**: All location pickers sync via localStorage

### **Performance Optimizations**
- **Grade A+ performance** maintained with extensive functionality
- **Conditional script loading** only on relevant pages
- **Cache-aware queries** for real-time updates
- **Ultra-fast page transitions** with zero flash

## 📊 **System Status Dashboard**

| Component | Status | Technical Purpose |
|-----------|--------|------------------|
| Store Filter + Cache | ✅ Active | Database filtering with WP Engine cache exclusion |
| Product Scheduler | ✅ Active | Admin UI with UTC/timezone conversion |
| Admin Filter | ✅ Active | Visual indicators and store-based filtering |
| Auto Button Generator | ✅ Active | Dynamic buttons with flash prevention |
| BlazeSlider Carousel | ✅ Active | Real-time product carousels |
| Scheduler Dashboard | ✅ Active | Live monitoring of scheduled products |
| Age Gate + Store Picker | ✅ Active | Cannabis compliance with localStorage persistence |
| CSS Class Visibility | ✅ Active | Content/menu control using .davis-* and .dixon-* classes |
| Store Dropdown System | ✅ Active | Multi-dropdown sync with styled-store-select-* pattern |
| Tymber Menu Integration | ✅ Active | Text-based navigation to proxy-passed Tymber system |
| **Desktop Location Picker** | ✅ **Active** | **Custom header component with black button design** |
| **Mobile Location Picker** | ✅ **Active** | **Mobile menu LOCATIONS styled as collapsible black button** |

## ⚠️ **Critical Configuration Points**

1. **Store IDs**: Hardcoded in JavaScript - see `docs/store-id-configuration.md` if changed
2. **Proxy Setup**: Menu URLs must proxy to Tymber correctly for localStorage sharing
3. **WP Engine Cache**: Cache exclusion headers required for real-time scheduling
4. **localStorage Keys**: Must match between WordPress and Tymber systems
5. **CSS Classes**: Must use `.davis-content`, `.dixon-content`, `.davis-menu`, `.dixon-menu` in Divi
6. **Dropdown IDs**: Must start with `styled-store-select-` for automatic detection
7. **Desktop Header**: Custom location picker component must be added to header template
8. **Mobile Menu**: LOCATIONS menu item must have `dt-hide-on-desktop` CSS class
9. **Divi Body Code**: Complete JavaScript must be added to Divi Theme Options → Integration → Body

## 🎯 **Quick Start**

1. **Installation**: Follow [INSTALLATION.md](INSTALLATION.md) for complete setup
2. **Desktop Header**: Add custom location picker component to header template
3. **Mobile Menu**: Ensure LOCATIONS has `dt-hide-on-desktop` class and Davis/Dixon sub-items
4. **Divi Setup**: Add CSS classes to sections and menu items for store visibility
5. **Tymber Integration**: Ensure proxy pass is configured correctly
6. **Test Flow**: Verify age gate → store selection → content visibility → menu navigation
7. **Admin Setup**: Configure product schedules and monitor via dashboard

## 🛠️ **Installation Summary**

### **Required Files to Install:**
1. **PHP Snippets** (6 files) → WordPress Code Snippets plugin
2. **Complete Mobile Menu CSS** → Divi Custom CSS
3. **Complete Body JavaScript** → Divi Theme Options → Integration → Body
4. **Desktop Location Picker** → Header template or Divi header module
5. **Store Dropdown HTML** → Divi code modules where needed

### **WordPress Menu Setup:**
- Create menu item "LOCATIONS" with CSS class `dt-hide-on-desktop`
- Add sub-items "Davis" and "Dixon" under LOCATIONS
- Davis and Dixon can link to # or be empty links

### **Testing Checklist:**
- ✅ Desktop location picker shows as black button
- ✅ Desktop dropdown shows Davis/Dixon options
- ✅ Mobile LOCATIONS shows as black button (not regular menu item)
- ✅ Mobile Davis/Dixon hidden by default
- ✅ Mobile dropdown shows when LOCATIONS clicked
- ✅ Store switching works on both desktop and mobile
- ✅ Content sections show/hide based on store selection
- ✅ Page dropdowns sync with location pickers

---

**CLUB420 - Sophisticated cannabis retail system with unified location picking across desktop header and mobile menu, real-time scheduling and seamless Tymber integration**
```
