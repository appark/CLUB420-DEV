# Club420 Location Picker System - Complete Implementation Guide

## Overview

This document provides the complete, tested implementation for the **Desktop Header Location Picker** and **Mobile Menu Location Picker** system integrated with the Club420 WooCommerce multi-store platform.

**System Status**: ✅ **Production Ready & Tested**  
**Components**: Desktop Divi Header + Mobile Menu Transformation + Backend Integration

## Table of Contents

1. [System Architecture](#system-architecture)
2. [Desktop Header Location Picker (Divi Code Module)](#desktop-header-location-picker-divi-code-module)
3. [Mobile Menu Location Picker (WordPress Menu Transformation)](#mobile-menu-location-picker-wordpress-menu-transformation)
4. [Complete Body JavaScript](#complete-body-javascript)
5. [Backend Integration (PHP Store Filtering)](#backend-integration-php-store-filtering)
6. [Implementation Guide](#implementation-guide)
7. [Technical Flow](#technical-flow)
8. [Troubleshooting](#troubleshooting)

## System Architecture

### Multi-Store Integration

The Club420 location picker system combines:
- **WordPress/WooCommerce** - Main site with store-specific product filtering
- **Tymber Integration** - External menu system via proxy pass
- **Desktop Location Picker** - Custom Divi header component
- **Mobile Location Picker** - WordPress menu transformation
- **Shared State Management** - Cross-system localStorage
- **Store-Specific Content** - Dynamic content visibility

### Store Configuration

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

### Proxy Architecture

```
club420.com/menu/f-street/ → Proxy pass to Tymber (Davis location)
club420.com/menu/highway-80/ → Proxy pass to Tymber (Dixon location)
```

### localStorage State Management

```javascript
// Store selection persistence
localStorage.setItem('last-store-selected', storeID);

// Cannabis compliance verification
localStorage.setItem('tymber-user-has-allowed-age', 'true');
```

## Desktop Header Location Picker (Divi Code Module)

### Implementation Method
**Location**: Divi Header → Add Code Module  
**Content**: Single code module containing HTML + CSS + JavaScript

### Complete Divi Code Module

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

<style>
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
  text-decoration-color: #f2ac1d;
  text-underline-offset: 6px;
  font-weight: 400;
}

.location-arrow {
  color: #fff;
  font-size: 18px;
  transition: transform 0.3s ease, font-size 0.3s ease;
}

.location-button.active .location-arrow {
  transform: rotate(90deg);
  font-size: 22px;
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
</style>

<script>
document.addEventListener('DOMContentLoaded', function () {
  const button = document.getElementById('locationButton');
  const dropdown = document.getElementById('locationDropdown');
  const currentLocation = document.getElementById('currentLocation');

  // Click handler for button
  button.addEventListener('click', function () {
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
    option.addEventListener('click', function () {
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
        console.log('Club420: Desktop switching to', store);
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
</script>
```

### Desktop Functionality

- **Black button design** with "Davis Change Location" text and yellow underline
- **Elegant dropdown** with 2-column grid (Davis | Dixon)
- **Hover effects** and smooth animations
- **State persistence** via localStorage
- **Automatic hiding** on mobile devices

## Mobile Menu Location Picker (WordPress Menu Transformation)

### Concept Overview

The mobile location picker **transforms a standard WordPress menu item** into a sophisticated black button dropdown that matches the desktop design.

### WordPress Menu Structure Required

```
📋 WordPress Admin → Appearance → Menus

LOCATIONS (Parent Menu Item)
├── Navigation Label: "Locations"
├── URL: Empty (critical - leave blank, not #)
├── CSS Classes: dt-hide-on-desktop
└── Sub-items:
    ├── Davis
    │   ├── Navigation Label: "Davis"
    │   ├── URL: #
    │   └── CSS Classes: dt-hide-on-desktop
    └── Dixon
        ├── Navigation Label: "Dixon"
        ├── URL: #
        └── CSS Classes: dt-hide-on-desktop
```

### Complete Mobile CSS

**Location**: Divi → Appearance → Customize → Additional CSS

```css
/* Hide WooCommerce Cart Icon */
.et-cart-info { display: none !important; }

/* Hide Menu item on Desktop */
@media (min-width: 981px) {
  .dt-hide-on-desktop {
    display: none !important;
  }
}

@media only screen and (max-width: 980px) {
  .mobile_menu_bar:before {
    color: #f2ac1d !important;
    font-size: 35px !important;
  }
}

/* ========================================= */
/* MOBILE MENU - ONLY TARGET LOCATIONS */
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

/* All menu items */
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

/* Menu item hover */
ul.et_mobile_menu .menu-item a:hover {
    background: #f8f9fa !important;
    color: #f2ac1d !important;
    padding-left: 35px !important;
    border-left: 4px solid #f2ac1d !important;
}

@media (max-width: 980px) {
  
  /* ========================================= */
  /* ONLY HIDE LOCATIONS SUBMENU BY DEFAULT */
  /* ========================================= */
  ul.et_mobile_menu .menu-item-has-children.dt-hide-on-desktop .sub-menu {
    display: none !important;
    opacity: 0 !important;
    height: 0 !important;
    overflow: hidden !important;
    margin: 0 !important;
    padding: 0 !important;
    border: none !important;
  }
  
  /* ========================================= */
  /* LOCATIONS BUTTON STYLING - DESKTOP STYLE */
  /* ========================================= */
  ul.et_mobile_menu .menu-item-has-children.dt-hide-on-desktop > a {
    background: #000 !important;
    color: #fff !important;
    border: 2px solid #fff !important;
    border-radius: 12px !important;
    margin: 10px 15px !important;
    font-weight: 600 !important;
    text-transform: none !important;
    position: relative !important;
    transition: all 0.3s ease !important;
    display: flex !important;
    align-items: center !important;
    justify-content: space-between !important;
    padding: 15px 25px !important;
    min-height: 50px !important;
  }
  
  /* Store name styling - Multiple fallback methods */
  ul.et_mobile_menu .menu-item-has-children.dt-hide-on-desktop > a .store-name,
  ul.et_mobile_menu .menu-item-has-children.dt-hide-on-desktop > a span:first-child {
    color: #fff !important;
    font-weight: 600 !important;
    margin-right: 10px !important;
    font-size: 16px !important;
    text-decoration: none !important;
    display: inline-block !important;
  }
  
  /* "Change Location" with yellow underline - Multiple fallback methods */
  ul.et_mobile_menu .menu-item-has-children.dt-hide-on-desktop > a .change-location,
  ul.et_mobile_menu .menu-item-has-children.dt-hide-on-desktop > a span:last-child {
    color: #fff !important;
    font-weight: 400 !important;
    font-size: 16px !important;
    text-decoration: underline !important;
    text-decoration-color: #f2ac1d !important;
    text-decoration-thickness: 2px !important;
    text-underline-offset: 4px !important;
    border-bottom: 2px solid #f2ac1d !important; /* Fallback method 1 */
    padding-bottom: 2px !important;
    position: relative !important;
    display: inline-block !important;
  }
  
  /* Additional yellow underline fallback using pseudo-element */
  ul.et_mobile_menu .menu-item-has-children.dt-hide-on-desktop > a .change-location:after,
  ul.et_mobile_menu .menu-item-has-children.dt-hide-on-desktop > a span:last-child:after {
    content: "" !important;
    position: absolute !important;
    bottom: -2px !important;
    left: 0 !important;
    right: 0 !important;
    height: 2px !important;
    background: #f2ac1d !important; /* Fallback method 2 */
  }
  
  /* Arrow styling and animation */
  ul.et_mobile_menu .menu-item-has-children.dt-hide-on-desktop > a:after {
    content: "›" !important;
    font-size: 18px !important;
    transition: transform 0.3s ease !important;
    margin-left: auto !important;
    color: #fff !important;
    order: 3 !important;
  }
  
  ul.et_mobile_menu .menu-item-has-children.dt-hide-on-desktop.open > a:after {
    transform: rotate(90deg) !important;
  }
  
  /* ========================================= */
  /* LOCATIONS DROPDOWN - 2-COLUMN GRID ONLY */
  /* ========================================= */
  ul.et_mobile_menu .menu-item-has-children.dt-hide-on-desktop.open .sub-menu {
    display: flex !important;
    opacity: 1 !important;
    height: auto !important;
    background: #fff !important;
    border-radius: 15px !important;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2) !important;
    margin: 10px 15px 20px 15px !important;
    padding: 0 !important;
    overflow: hidden !important;
    border: none !important;
  }
  
  /* Davis and Dixon - Equal width columns */
  ul.et_mobile_menu .menu-item-has-children.dt-hide-on-desktop .sub-menu .menu-item {
    flex: 1 !important;
    width: 50% !important;
    display: block !important;
  }
  
  ul.et_mobile_menu .menu-item-has-children.dt-hide-on-desktop .sub-menu .menu-item a {
    padding: 20px 25px !important;
    font-size: 16px !important;
    font-weight: 500 !important;
    color: #333 !important;
    text-transform: none !important;
    background: #fff !important;
    border: none !important;
    text-align: center !important;
    transition: all 0.3s ease !important;
    display: block !important;
    width: 100% !important;
    box-sizing: border-box !important;
  }
  
  /* Border between Davis and Dixon */
  ul.et_mobile_menu .menu-item-has-children.dt-hide-on-desktop .sub-menu .menu-item:nth-child(2) a {
    border-left: 1px solid #f0f0f0 !important;
  }
  
  /* Hover effects */
  ul.et_mobile_menu .menu-item-has-children.dt-hide-on-desktop .sub-menu .menu-item a:hover {
    background: #f8f9fa !important;
    color: #f2ac1d !important;
    padding-left: 25px !important;
    border-left: 1px solid #f0f0f0 !important;
  }
  
  /* Second column hover - maintain border */
  ul.et_mobile_menu .menu-item-has-children.dt-hide-on-desktop .sub-menu .menu-item:nth-child(2) a:hover {
    border-left: 1px solid #f0f0f0 !important;
  }
  
  /* Selected state - Yellow background */
  ul.et_mobile_menu .menu-item-has-children.dt-hide-on-desktop .sub-menu .menu-item.selected a {
    background: #f2ac1d !important;
    color: #000 !important;
    font-weight: 600 !important;
  }
  
  /* Selected state second column - maintain border */
  ul.et_mobile_menu .menu-item-has-children.dt-hide-on-desktop .sub-menu .menu-item:nth-child(2).selected a {
    border-left: 1px solid #f0f0f0 !important;
  }
}

/* Remove border from last menu item */
ul.et_mobile_menu > .menu-item:last-child > a {
    border-bottom: none !important;
}
```

### Mobile Functionality

- **LOCATIONS menu item** transformed into black button
- **"Davis Change Location"** text with yellow underline 
- **2-column dropdown** (Davis | Dixon side by side)
- **Yellow selection state** for current store
- **Nuclear JavaScript approach** to override Divi behavior

## Complete Body JavaScript

**Location**: Divi → Theme Options → Integration → Body

```javascript
<!-- COMPLETE Club420 System - ALL COMPONENTS INTEGRATED -->

<style>
  @keyframes fadeIn {
    from { opacity: 0; transform: scale(0.95); }
    to { opacity: 1; transform: scale(1); }
  }

  @keyframes pulse {
    0%, 100% { transform: scale(1); }
    50% { transform: scale(1.05); }
  }

  /* Age Gate Overlay */
  #club420-age-gate {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.95);
    z-index: 999999;
    display: flex;
    align-items: center;
    justify-content: center;
    animation: fadeIn 0.5s ease-in;
  }

  .age-gate-content {
    background: white;
    padding: 40px;
    border-radius: 20px;
    text-align: center;
    max-width: 500px;
    margin: 20px;
    box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
    animation: fadeIn 0.7s ease-in 0.2s both;
  }

  .age-gate-content h2 {
    color: #333;
    margin-bottom: 20px;
    font-size: 28px;
  }

  .age-gate-content p {
    color: #666;
    margin-bottom: 30px;
    font-size: 16px;
    line-height: 1.5;
  }

  .age-gate-buttons {
    display: flex;
    gap: 20px;
    justify-content: center;
    flex-wrap: wrap;
  }

  .age-gate-button {
    padding: 15px 30px;
    border: none;
    border-radius: 25px;
    font-size: 16px;
    font-weight: bold;
    cursor: pointer;
    transition: all 0.3s ease;
    min-width: 120px;
  }

  .age-gate-yes {
    background: #000;
    color: white;
    border: 2px solid #f2ac1d;
  }

  .age-gate-yes:hover {
    background: #f2ac1d;
    color: #000;
    animation: pulse 1s infinite;
  }

  .age-gate-no {
    background: #ccc;
    color: #666;
  }

  .age-gate-no:hover {
    background: #999;
    color: white;
  }

  /* Store Transition Overlay */
  #club420-transition {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.9);
    z-index: 999998;
    display: flex;
    align-items: center;
    justify-content: center;
    opacity: 0;
    visibility: hidden;
    transition: all 0.3s ease;
  }

  #club420-transition.active {
    opacity: 1;
    visibility: visible;
  }

  .transition-content {
    background: white;
    padding: 40px;
    border-radius: 20px;
    text-align: center;
    max-width: 400px;
    margin: 20px;
    box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
    animation: fadeIn 0.5s ease-in;
  }

  .transition-content h3 {
    color: #333;
    margin-bottom: 15px;
    font-size: 20px;
  }

  .transition-content p {
    color: #666;
    margin-bottom: 20px;
  }

  .transition-spinner {
    width: 40px;
    height: 40px;
    border: 3px solid #f3f3f3;
    border-top: 3px solid #f2ac1d;
    border-radius: 50%;
    animation: spin 1s linear infinite;
    margin: 0 auto;
  }

  @keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
  }

  /* Store dropdown styling */
  .styled-store-select {
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
    background-image: url('data:image/svg+xml;charset=US-ASCII,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 4 5"><path fill="%23ffffff" d="M2 0L0 2h4zm0 5L0 3h4z"/></svg>');
    background-repeat: no-repeat;
    background-position: right 1rem center;
    background-size: 12px;
    transition: all 0.3s ease;
  }

  .styled-store-select:hover {
    background: #333;
    border-color: #f2ac1d;
  }

  .styled-store-select option {
    color: #333;
    background: white;
  }
</style>

<script>
console.log('Club420: Complete system loading...');

// Store IDs and configuration
const davisID = '79043044-f024-4b70-8714-4fcad409f978';
const dixonID = '7029749f-9c6d-419e-b037-5c1b566f3df9';

// BOT DETECTION for performance testing tools
function isBot() {
  const botUserAgents = [
    'googlebot', 'bingbot', 'slurp', 'duckduckbot', 'baiduspider',
    'yandexbot', 'facebookexternalhit', 'twitterbot', 'rogerbot',
    'linkedinbot', 'embedly', 'quora link preview', 'showyoubot',
    'outbrain', 'pinterest/0.', 'developers.google.com/+/web/snippet',
    'slackbot', 'vkshare', 'w3c_validator', 'redditbot', 'applebot',
    'whatsapp', 'flipboard', 'tumblr', 'bitlybot', 'skypeuripreview',
    'nuzzel', 'discordbot', 'google page speed', 'qwantify',
    'pinterestbot', 'bitrix link preview', 'xing-contenttabreceiver',
    'chrome-lighthouse', 'telegrambot', 'pagespeed', 'gtmetrix',
    'pingdom', 'webpagetest', 'uptime', 'websitepulse', 'monitis'
  ];
  
  const userAgent = navigator.userAgent.toLowerCase();
  return botUserAgents.some(bot => userAgent.includes(bot));
}

// AGE GATE SYSTEM
function initializeAgeGate() {
  if (isBot()) {
    console.log('Club420: Bot detected - bypassing age gate for performance testing');
    localStorage.setItem('tymber-user-has-allowed-age', 'true');
    return;
  }
  
  const hasVerified = localStorage.getItem('tymber-user-has-allowed-age') === 'true';
  
  if (!hasVerified) {
    showAgeGate();
  }
}

function showAgeGate() {
  const overlay = document.createElement('div');
  overlay.id = 'club420-age-gate';
  overlay.innerHTML = `
    <div class="age-gate-content">
      <h2>🍃 Welcome to Club420</h2>
      <p>You must be 21 years or older to enter this site. Please verify your age to continue.</p>
      <div class="age-gate-buttons">
        <button class="age-gate-button age-gate-yes" onclick="verifyAge(true)">
          I'm 21 or Older
        </button>
        <button class="age-gate-button age-gate-no" onclick="verifyAge(false)">
          Under 21
        </button>
      </div>
    </div>
  `;
  
  document.body.appendChild(overlay);
  document.body.style.overflow = 'hidden';
}

function verifyAge(isOldEnough) {
  if (isOldEnough) {
    localStorage.setItem('tymber-user-has-allowed-age', 'true');
    document.getElementById('club420-age-gate').remove();
    document.body.style.overflow = '';
    console.log('Club420: Age verified - access granted');
  } else {
    window.location.href = 'https://www.google.com';
  }
}

// Make verifyAge globally available
window.verifyAge = verifyAge;

// ULTRA-FAST STORE SWITCHING with smooth transition
function setStoreSmoothReload(store) {
  const id = store === 'davis' ? davisID : dixonID;
  const storeName = store === 'davis' ? 'Davis' : 'Dixon';
  
  console.log('Club420 ULTRA-FAST: Switching to', storeName);
  
  // Show transition overlay
  const transition = document.getElementById('club420-transition');
  const storeText = document.getElementById('transition-store-text');
  
  if (transition) {
    transition.classList.add('active');
  }
  
  if (storeText) {
    storeText.textContent = `Switching to ${storeName} Store`;
  }
  
  // Update localStorage
  localStorage.setItem('last-store-selected', id);
  
  // Redirect with store filter
  const currentUrl = new URL(window.location);
  currentUrl.searchParams.set('store_filter', store);
  window.location.replace(currentUrl.toString());
}

// Make store switching globally available
window.setStore = setStoreSmoothReload;

// CONTENT VISIBILITY SYSTEM
function showStoreSectionsSmooth() {
  const store = localStorage.getItem('last-store-selected');
  
  const davisElements = document.querySelectorAll('.davis-content');
  const dixonElements = document.querySelectorAll('.dixon-content');
  const davisMenuItems = document.querySelectorAll('.davis-menu');
  const dixonMenuItems = document.querySelectorAll('.dixon-menu');

  // Hide all store-specific content
  [...davisElements, ...dixonElements].forEach(el => el.style.display = 'none');
  [...davisMenuItems, ...dixonMenuItems].forEach(el => el.style.display = 'none');

  // Show content for selected store
  if (store === davisID) {
    [...davisElements, ...davisMenuItems].forEach(el => el.style.display = '');
    console.log('Club420: Showing Davis sections');
  } else if (store === dixonID) {
    [...dixonElements, ...dixonMenuItems].forEach(el => el.style.display = '');
    console.log('Club420: Showing Dixon sections');
  } else {
    // Default to Davis if no store selected
    localStorage.setItem('last-store-selected', davisID);
    [...davisElements, ...davisMenuItems].forEach(el => el.style.display = '');
    console.log('Club420: No store selected - defaulting to Davis');
  }
}

// MULTIPLE DROPDOWN SUPPORT for page store selectors
function initializeStoreDropdowns() {
  const dropdowns = document.querySelectorAll('[id^="styled-store-select"]');
  
  dropdowns.forEach(dropdown => {
    console.log('Club420: Found store dropdown:', dropdown.id);
    
    // Set initial value based on localStorage
    const currentStore = localStorage.getItem('last-store-selected');
    if (currentStore === dixonID) {
      dropdown.value = 'dixon';
    } else {
      dropdown.value = 'davis';
    }
    
    // Add change event listener
    dropdown.addEventListener('change', function() {
      const selectedStore = this.value;
      if (selectedStore && (selectedStore === 'davis' || selectedStore === 'dixon')) {
        console.log('Club420: Store dropdown changed to:', selectedStore);
        setStoreSmoothReload(selectedStore);
      }
    });
  });
}

// MENU MANAGER - Advanced text detection for Tymber navigation
function initializeMenuManager() {
  setTimeout(function() {
    const menuLinks = document.querySelectorAll('a[href="#"], .et_pb_menu a, .et_mobile_menu a, nav a');
    
    menuLinks.forEach(link => {
      const linkText = link.textContent.trim().toUpperCase();
      
      // Tymber navigation detection
      const navigationMap = {
        'FLOWER': '/categories/flower/',
        'CARTRIDGES': '/categories/cartridge/',
        'CARTRIDGE': '/categories/cartridge/',
        'EDIBLES': '/categories/edible/',
        'EDIBLE': '/categories/edible/',
        'PRE-ROLLS': '/categories/preroll/',
        'PREROLLS': '/categories/preroll/',
        'PRE-ROLL': '/categories/preroll/',
        'PREROLL': '/categories/preroll/',
        'EXTRACTS': '/categories/extract/',
        'EXTRACT': '/categories/extract/',
        'SHOP ALL': '/',
        'ALL': '/',
        'SHOP NOW': '/'
      };
      
      if (navigationMap[linkText]) {
        link.addEventListener('click', function(e) {
          e.preventDefault();
          
          const currentStore = localStorage.getItem('last-store-selected');
          let basePath = '/menu/f-street'; // Default to Davis
          
          if (currentStore === dixonID) {
            basePath = '/menu/highway-80';
          }
          
          const fullPath = basePath + navigationMap[linkText];
          console.log('Club420: Menu navigation to:', fullPath);
          
          window.location.href = fullPath;
        });
        
        console.log('Club420: Menu link configured:', linkText, '→', navigationMap[linkText]);
      }
    });
  }, 1000);
}

// NUCLEAR MOBILE LOCATIONS - COMPLETELY OVERRIDES DIVI
window.addEventListener('DOMContentLoaded', function() {
  setTimeout(function() {
    console.log('Club420: Setting up NUCLEAR mobile LOCATIONS integration...');
    
    const mobileLocations = document.querySelector('ul.et_mobile_menu .menu-item-has-children.dt-hide-on-desktop');
    
    if (!mobileLocations) {
      console.log('Club420: Mobile LOCATIONS menu item not found');
      return;
    }
    
    const locationsButton = mobileLocations.querySelector('a');
    const submenu = mobileLocations.querySelector('.sub-menu');
    
    // TEXT-BASED DETECTION (Critical Fix)
    let davisLink = null;
    let dixonLink = null;

    if (submenu) {
      const menuLinks = submenu.querySelectorAll('.menu-item a');
      menuLinks.forEach(link => {
        const linkText = link.textContent.trim().toLowerCase();
        if (linkText === 'davis') {
          davisLink = link;
          console.log('Club420: Found Davis link by text');
        } else if (linkText === 'dixon') {
          dixonLink = link;
          console.log('Club420: Found Dixon link by text');
        }
      });
    }
    
    if (!locationsButton || !submenu || !davisLink || !dixonLink) {
      console.log('Club420: Required mobile LOCATIONS elements not found');
      return;
    }
    
    // Set initial state
    updateMobileLocationState();
    
    // NUCLEAR: Completely override Divi's behavior
    locationsButton.addEventListener('click', function(e) {
      e.preventDefault();
      e.stopPropagation();
      e.stopImmediatePropagation();
      
      console.log('Club420: NUCLEAR - Completely blocking Divi');
      
      // Manually toggle the submenu
      mobileLocations.classList.toggle('open');
      
      return false;
    }, true); // Capture phase
    
    // Davis click handler
    davisLink.addEventListener('click', function(e) {
      e.preventDefault();
      console.log('Club420: Mobile Davis selected');
      selectMobileStore('davis', 'Davis');
    });
    
    // Dixon click handler
    dixonLink.addEventListener('click', function(e) {
      e.preventDefault();
      console.log('Club420: Mobile Dixon selected');
      selectMobileStore('dixon', 'Dixon');
    });
    
    function selectMobileStore(store, storeName) {
      // Update CSS classes for visual state
      const davisItem = davisLink.closest('.menu-item');
      const dixonItem = dixonLink.closest('.menu-item');
      
      davisItem.classList.remove('selected');
      dixonItem.classList.remove('selected');
      
      if (store === 'davis') {
        davisItem.classList.add('selected');
      } else {
        dixonItem.classList.add('selected');
      }
      
      // Close the dropdown
      mobileLocations.classList.remove('open');
      
      // Update button text to show newly selected store
      ensureButtonTextIsLocations();
      
      // Trigger store switching via global function
      console.log('Club420: Mobile store switching to:', store);
      setStoreSmoothReload(store);
    }
    
    function updateMobileLocationState() {
      const currentStore = localStorage.getItem('last-store-selected');
      const davisItem = davisLink.closest('.menu-item');
      const dixonItem = dixonLink.closest('.menu-item');
      
      davisItem.classList.remove('selected');
      dixonItem.classList.remove('selected');
      
      if (currentStore === dixonID) {
        dixonItem.classList.add('selected');
        console.log('Club420: Mobile initial state - Dixon selected');
      } else {
        davisItem.classList.add('selected');
        console.log('Club420: Mobile initial state - Davis selected');
      }
      
      ensureButtonTextIsLocations();
    }
    
    function ensureButtonTextIsLocations() {
      if (locationsButton) {
        // Get current store from localStorage
        const currentStore = localStorage.getItem('last-store-selected');
        const dixonID = '7029749f-9c6d-419e-b037-5c1b566f3df9';
        
        // Determine current store name
        const storeName = (currentStore === dixonID) ? 'Dixon' : 'Davis';
        
        // Clear existing content
        locationsButton.innerHTML = '';
        
        // Create container for the text content
        const textContainer = document.createElement('div');
        textContainer.style.display = 'flex';
        textContainer.style.alignItems = 'center';
        textContainer.style.gap = '10px';
        textContainer.style.width = '100%';
        textContainer.style.justifyContent = 'space-between';
        
        // Create left side container
        const leftContainer = document.createElement('div');
        leftContainer.style.display = 'flex';
        leftContainer.style.alignItems = 'center';
        leftContainer.style.gap = '10px';
        
        // Add store name span
        const storeSpan = document.createElement('span');
        storeSpan.className = 'store-name';
        storeSpan.textContent = storeName;
        storeSpan.style.color = '#fff';
        storeSpan.style.fontWeight = '600';
        storeSpan.style.fontSize = '16px';
        
        // Add "Change Location" span with robust yellow underline
        const changeSpan = document.createElement('span');
        changeSpan.className = 'change-location';
        changeSpan.textContent = 'Change Location';
        changeSpan.style.color = '#fff';
        changeSpan.style.fontWeight = '400';
        changeSpan.style.fontSize = '16px';
        changeSpan.style.textDecoration = 'underline';
        changeSpan.style.textDecorationColor = '#f2ac1d';
        changeSpan.style.textDecorationThickness = '2px';
        changeSpan.style.textUnderlineOffset = '4px';
        changeSpan.style.borderBottom = '2px solid #f2ac1d'; // Fallback method 1
        changeSpan.style.paddingBottom = '2px';
        changeSpan.style.position = 'relative';
        
        // Emergency fallback - inline background gradient
        changeSpan.style.backgroundImage = 'linear-gradient(to right, #f2ac1d 0%, #f2ac1d 100%)';
        changeSpan.style.backgroundSize = '100% 2px';
        changeSpan.style.backgroundRepeat = 'no-repeat';
        changeSpan.style.backgroundPosition = 'bottom left';
        
        // Add spans to left container
        leftContainer.appendChild(storeSpan);
        leftContainer.appendChild(changeSpan);
        
        // Add left container to main container
        textContainer.appendChild(leftContainer);
        
        // Add container to button
        locationsButton.appendChild(textContainer);
        
        console.log(`Club420: Updated mobile button: ${storeName} Change Location`);
        console.log('Club420: Yellow underline methods applied - CSS + border-bottom fallback + background gradient');
      }
    }
    
    console.log('Club420: NUCLEAR mobile LOCATIONS integration complete! 📱💣✅');
    
  }, 2000); // Delay to ensure mobile menu is loaded
});

// PAGE TRANSITIONS for smooth UX
function addPageTransitions() {
  const links = document.querySelectorAll('a[href]:not([target="_blank"]):not([href^="#"]):not([href^="mailto:"]):not([href^="tel:"])');
  
  links.forEach(link => {
    link.addEventListener('click', function(e) {
      const href = this.getAttribute('href');
      
      // Skip if it's an internal anchor or external link
      if (href.startsWith('#') || href.includes('://') && !href.includes(window.location.hostname)) {
        return;
      }
      
      // Add smooth transition
      document.body.style.opacity = '0.7';
      document.body.style.transform = 'scale(0.98)';
      document.body.style.transition = 'all 0.3s ease';
    });
  });
}

// INITIALIZE EVERYTHING
document.addEventListener('DOMContentLoaded', function() {
  console.log('Club420: DOM loaded - initializing complete system...');
  
  // Create transition overlay
  const transitionOverlay = document.createElement('div');
  transitionOverlay.id = 'club420-transition';
  transitionOverlay.innerHTML = `
    <div class="transition-content">
      <h3>🍃 Club420</h3>
      <p id="transition-store-text">Updating your selection...</p>
      <div class="transition-spinner"></div>
    </div>
  `;
  document.body.appendChild(transitionOverlay);
  
  // Initialize all systems
  initializeAgeGate();
  setTimeout(() => {
    showStoreSectionsSmooth();
    initializeStoreDropdowns();
    initializeMenuManager();
    addPageTransitions();
  }, 500);
  
  console.log('Club420: Complete system initialized! 🚀');
});

// Handle browser back/forward navigation
window.addEventListener('popstate', function() {
  showStoreSectionsSmooth();
  initializeStoreDropdowns();
});

// Handle page visibility changes (tab switching)
document.addEventListener('visibilitychange', function() {
  if (!document.hidden) {
    showStoreSectionsSmooth();
  }
});

console.log('Club420: Complete system loaded! All components ready. 🍃🚀');
</script>
```

## Backend Integration (PHP Store Filtering)

### PHP Snippet 1: Store Filter + Cache Exclusion

**Location**: WordPress Admin → Snippets → Add New

```php
<?php
// Club420 Store Filter with Real-Time Scheduling + WP Engine Cache Exclusion
// Snippet 1: Core store filtering with cache control

add_filter('woocommerce_shortcode_products_query', 'club420_realtime_filter_with_scheduling', 10, 2);
function club420_realtime_filter_with_scheduling($args, $atts) {
    
    // Get store from URL parameter (set by location pickers)
    $store_location = isset($_GET['store_filter']) ? sanitize_text_field($_GET['store_filter']) : 'all';
    
    if ($store_location !== 'all') {
        if (!isset($args['meta_query'])) {
            $args['meta_query'] = array();
        }
        
        $args['meta_query']['relation'] = 'OR';
        
        if ($store_location === 'davis') {
            // Show products enabled for Davis store
            $args['meta_query'][] = array(
                'relation' => 'AND',
                array(
                    'key' => '_club420_davis_url',
                    'value' => '',
                    'compare' => '!='
                ),
                array(
                    'key' => '_club420_davis_enabled',
                    'value' => 'yes',
                    'compare' => '='
                )
            );
        } elseif ($store_location === 'dixon') {
            // Show products enabled for Dixon store
            $args['meta_query'][] = array(
                'relation' => 'AND',
                array(
                    'key' => '_club420_dixon_url',
                    'value' => '',
                    'compare' => '!='
                ),
                array(
                    'key' => '_club420_dixon_enabled',
                    'value' => 'yes',
                    'compare' => '='
                )
            );
        }
    }
    
    // Add real-time scheduling filter
    $current_utc = gmdate('Y-m-d H:i:s');
    
    if (!isset($args['meta_query'])) {
        $args['meta_query'] = array();
    }
    
    $args['meta_query'][] = array(
        'relation' => 'OR',
        array(
            'key' => '_club420_schedule_start',
            'compare' => 'NOT EXISTS'
        ),
        array(
            'key' => '_club420_schedule_start',
            'value' => '',
            'compare' => '='
        ),
        array(
            'relation' => 'AND',
            array(
                'key' => '_club420_schedule_start',
                'value' => $current_utc,
                'compare' => '<='
            ),
            array(
                'relation' => 'OR',
                array(
                    'key' => '_club420_schedule_end',
                    'compare' => 'NOT EXISTS'
                ),
                array(
                    'key' => '_club420_schedule_end',
                    'value' => '',
                    'compare' => '='
                ),
                array(
                    'key' => '_club420_schedule_end',
                    'value' => $current_utc,
                    'compare' => '>='
                )
            )
        )
    );
    
    return $args;
}

// WP Engine Cache Exclusion for Real-Time Updates
add_action('init', 'club420_cache_exclusion_headers');
function club420_cache_exclusion_headers() {
    if (isset($_GET['store_filter']) || has_active_schedules()) {
        header('Cache-Control: no-cache, must-revalidate, max-age=0');
        header('Pragma: no-cache');
        header('Expires: Thu, 01 Jan 1970 00:00:00 GMT');
    }
}

function has_active_schedules() {
    $current_utc = gmdate('Y-m-d H:i:s');
    
    $query = new WP_Query(array(
        'post_type' => 'product',
        'posts_per_page' => 1,
        'meta_query' => array(
            array(
                'key' => '_club420_schedule_start',
                'value' => $current_utc,
                'compare' => '<='
            )
        )
    ));
    
    return $query->have_posts();
}
?>
```

### Store-Specific Content Classes

```css
/* Store-specific content classes for Divi modules */
.davis-content { /* Davis-only content sections */ }
.dixon-content { /* Dixon-only content sections */ }
.davis-menu { /* Davis-only menu items */ }
.dixon-menu { /* Dixon-only menu items */ }
```

## Implementation Guide

### Step 1: WordPress Menu Setup

1. **Go to**: WordPress Admin → Appearance → Menus
2. **Create menu structure**:
   ```
   LOCATIONS (Parent)
   ├── Navigation Label: "Locations"
   ├── URL: Leave empty (critical - not #)
   ├── CSS Classes: dt-hide-on-desktop
   ├── Davis (Sub-item)
   │   ├── Navigation Label: "Davis"
   │   ├── URL: #
   │   └── CSS Classes: dt-hide-on-desktop
   └── Dixon (Sub-item)
       ├── Navigation Label: "Dixon"
       ├── URL: #
       └── CSS Classes: dt-hide-on-desktop
   ```

### Step 2: Desktop Location Picker

1. **Go to**: Divi → Edit Header → Add Code Module
2. **Add content**: Copy the complete Divi Code Module from above
3. **Save**: Publish header changes

### Step 3: Mobile CSS Installation

1. **Go to**: Divi → Appearance → Customize → Additional CSS
2. **Add CSS**: Copy the complete Mobile CSS from above
3. **Save**: Publish changes

### Step 4: Body JavaScript Installation

1. **Go to**: Divi → Theme Options → Integration → Body
2. **Add JavaScript**: Copy the complete Body JavaScript from above
3. **Save**: Save options

### Step 5: Backend PHP Setup

1. **Go to**: WordPress Admin → Snippets → Add New
2. **Add PHP**: Copy the PHP Store Filter snippet from above
3. **Activate**: Enable the snippet

### Step 6: Testing & Verification

#### Desktop Testing
- ✅ Button shows "Davis Change Location" with yellow underline
- ✅ Dropdown opens with Davis | Dixon side-by-side
- ✅ Store switching works (localStorage + URL parameter)
- ✅ Button updates to show current store

#### Mobile Testing
- ✅ LOCATIONS appears as black button (not regular menu item)
- ✅ Button shows "Davis Change Location" with yellow underline
- ✅ Dropdown opens with 2-column layout
- ✅ Davis/Dixon selection works and closes dropdown
- ✅ Yellow background shows selected store

#### Backend Testing
- ✅ URL parameter `?store_filter=davis` filters products
- ✅ Content sections with `.davis-content` show/hide correctly
- ✅ localStorage persists across page loads

## Technical Flow

### User Interaction Flow

```
1. User clicks location picker (desktop or mobile)
   ↓
2. Dropdown opens showing Davis/Dixon options
   ↓
3. User selects store (Davis or Dixon)
   ↓
4. JavaScript updates:
   - Visual state (button text, selected styling)
   - localStorage ('last-store-selected')
   - URL parameter (?store_filter=davis/dixon)
   ↓
5. Page redirects with store filter
   ↓
6. PHP backend filters products by store
   ↓
7. Content visibility system shows store-specific sections
   ↓
8. User sees store-specific content and products
```

### Critical Technical Details

#### The Nuclear Mobile Approach
```javascript
// COMPLETELY override Divi's menu behavior
locationsButton.addEventListener('click', function(e) {
  e.preventDefault();
  e.stopPropagation();
  e.stopImmediatePropagation();
  
  // Manually toggle the submenu
  mobileLocations.classList.toggle('open');
  
  return false;
}, true); // Capture phase
```

#### Text-Based Detection Fix
```javascript
// CRITICAL FIX: Detect Davis/Dixon by text, not position
const menuLinks = submenu.querySelectorAll('.menu-item a');
menuLinks.forEach(link => {
  const linkText = link.textContent.trim().toLowerCase();
  if (linkText === 'davis') {
    davisLink = link;
  } else if (linkText === 'dixon') {
    dixonLink = link;
  }
});
```

#### Yellow Underline Multiple Fallbacks
```css
/* Method 1: Modern CSS */
text-decoration: underline !important;
text-decoration-color: #f2ac1d !important;

/* Method 2: Border fallback */
border-bottom: 2px solid #f2ac1d !important;

/* Method 3: Background gradient fallback */
background-image: linear-gradient(to right, #f2ac1d 0%, #f2ac1d 100%);
background-size: 100% 2px;
background-position: bottom left;
```

## Troubleshooting

### Common Issues & Solutions

#### Desktop Picker Not Showing
- **Check**: Divi header has Code Module with the complete code
- **Verify**: CSS is not being overridden by theme styles
- **Ensure**: JavaScript console shows no errors

#### Mobile LOCATIONS Shows as Regular Menu Item
- **Check**: WordPress menu structure has `dt-hide-on-desktop` CSS class
- **Verify**: Mobile CSS is installed in Divi Custom CSS
- **Confirm**: LOCATIONS parent URL is empty (not #)

#### Store Switching Not Working
- **Check**: Browser console for JavaScript errors
- **Verify**: `setStoreSmoothReload` function exists globally
- **Ensure**: localStorage is being updated with correct store IDs

#### Yellow Underline Not Visible
- **Check**: Browser developer tools for CSS overrides
- **Verify**: Multiple fallback methods are applied
- **Confirm**: Spans are created with correct classes

#### Davis/Dixon Detection Wrong
- **Issue**: Position-based detection failing
- **Solution**: Text-based detection is implemented
- **Check**: Console logs show "Found Davis link by text"

### Debug Console Messages

```javascript
// System initialization
"Club420: Complete system loading..."
"Club420: NUCLEAR mobile LOCATIONS integration complete!"

// Desktop picker
"Club420: Desktop switching to davis"

// Mobile picker  
"Club420: Found Davis link by text"
"Club420: Mobile Davis selected"
"Club420: Updated mobile button: Davis Change Location"

// Store switching
"Club420 ULTRA-FAST: Switching to Davis"
"Club420: Showing Davis sections"
```

### Performance Considerations

- **Mobile JavaScript**: 2-second delay to ensure menu loads
- **Bot Detection**: Bypasses age gate for performance testing tools
- **Cache Exclusion**: WP Engine cache bypassed for real-time updates
- **Event Efficiency**: Minimal event listeners with proper cleanup

---

## Conclusion

This Club420 Location Picker System provides a **production-ready, tested solution** for seamless store switching across desktop and mobile platforms. The system successfully transforms standard WordPress menus into sophisticated black button interfaces while maintaining deep integration with WooCommerce product filtering and external Tymber systems.

**Key Achievements:**
- ✅ **Desktop**: Custom Divi header component with elegant dropdown
- ✅ **Mobile**: WordPress menu transformation with nuclear override approach  
- ✅ **Backend**: PHP store filtering with cache exclusion
- ✅ **Cross-System**: localStorage state management and content visibility
- ✅ **Brand Consistency**: Matching design across all interfaces

**System Status**: 🚀 **Production Ready & Fully Tested**

---

**🍃 Club420: Complete Location Picker Implementation - Ready for Production Use! 📱💻**
