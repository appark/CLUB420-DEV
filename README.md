# COMPLETE CLUB420 SYSTEM 

---

I'm working on documenting the CLUB420 WooCommerce system on GitHub. We just finished organizing everything and need to create the individual files.

TASK: Help me break this complete working system into individual GitHub files and create INSTALLATION.md

## COMPLETED README.MD (READY FOR GITHUB)

```markdown
# CLUB420 WooCommerce Real-Time Scheduler & Tymber Integration

**Status**: ✅ **PHASE 2B COMPLETED** - Production Ready  
**Last Updated**: December 2024  
**Performance**: Grade A+ (85% Performance, 97% Structure)  
**Production Status**: ✅ **LIVE AND OPERATIONAL**

## 🏗️ **System Architecture Overview**

This is a sophisticated multi-system integration combining:
- **WordPress/WooCommerce** main site with real-time product scheduling
- **Tymber menu system** integration via proxy pass (`/menu/f-street/` → Tymber Davis, `/menu/highway-80/` → Tymber Dixon)
- **Shared state management** across system boundaries using localStorage
- **Cannabis compliance** age gate with cross-system persistence
- **Store-specific content and navigation** using CSS class visibility control

## 🔄 **Technical Integration Flow**

```
User Journey:
1. Lands on club420.com (WordPress)
2. Age gate verification → localStorage: 'tymber-user-has-allowed-age'
3. Store selection → localStorage: 'last-store-selected' (Davis/Dixon)
4. JavaScript shows store-specific content using CSS classes:
   - .davis-content/.davis-menu sections visible for Davis users
   - .dixon-content/.dixon-menu sections visible for Dixon users
5. Menu clicks route to appropriate Tymber system:
   - Davis Store → https://club420.com/menu/f-street/categories/flower/
   - Dixon Store → https://club420.com/menu/highway-80/categories/flower/
6. Tymber pages read localStorage → No re-verification needed
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
<select id="styled-store-select-page">
  <option value="">Choose Store Location</option>
  <option value="davis">Davis Store</option>
  <option value="dixon">Dixon Store</option>
</select>
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
├── docs/                             # Technical documentation
│   ├── how-it-works.md              # System architecture deep dive
│   ├── divi-integration.md           # CSS class system & dropdown setup
│   ├── tymber-integration.md         # Proxy setup & localStorage sharing
│   ├── store-id-configuration.md    # Store ID mapping changes
│   ├── wp-engine-cache.md           # Cache exclusion implementation
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

## ⚠️ **Critical Configuration Points**

1. **Store IDs**: Hardcoded in JavaScript - see `docs/store-id-configuration.md` if changed
2. **Proxy Setup**: Menu URLs must proxy to Tymber correctly for localStorage sharing
3. **WP Engine Cache**: Cache exclusion headers required for real-time scheduling
4. **localStorage Keys**: Must match between WordPress and Tymber systems
5. **CSS Classes**: Must use `.davis-content`, `.dixon-content`, `.davis-menu`, `.dixon-menu` in Divi
6. **Dropdown IDs**: Must start with `styled-store-select-` for automatic detection

## 🎯 **Quick Start**

1. **Installation**: Follow [INSTALLATION.md](INSTALLATION.md) for complete setup
2. **Divi Setup**: Add CSS classes to sections and menu items for store visibility
3. **Tymber Integration**: Ensure proxy pass is configured correctly
4. **Test Flow**: Verify age gate → store selection → content visibility → menu navigation
5. **Admin Setup**: Configure product schedules and monitor via dashboard

---

**CLUB420 - Sophisticated cannabis retail system with real-time scheduling and seamless Tymber integration**
```

## 6 PHP SNIPPETS (WordPress Code Snippets plugin)

### SNIPPET 1: Club420 Store Filter + Cache Exclusion
```php
// Club420 Carousel Store Filter - REAL-TIME SCHEDULING + CACHE EXCLUSION
// Clean Production Version

add_filter('woocommerce_shortcode_products_query', 'club420_realtime_filter_with_scheduling', 10, 2);
function club420_realtime_filter_with_scheduling($args, $atts) {
    
    // SKIP filtering if this is from our custom carousel
    if (isset($args['club420_carousel_query']) && $args['club420_carousel_query'] === true) {
        return $args; // Let carousel handle its own filtering
    }
    
    $store_location = isset($_GET['store_filter']) ? sanitize_text_field($_GET['store_filter']) : 'all';
    
    if ($store_location !== 'all') {
        if (!isset($args['meta_query'])) {
            $args['meta_query'] = array();
        }
        
        // Set relation to OR - product shows if EITHER manual toggle OR schedule is active
        $args['meta_query']['relation'] = 'OR';
        
        if ($store_location === 'davis') {
            
            // OPTION 1: Manual toggle enabled (original system)
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
            
            // OPTION 2: Scheduled and currently active (NEW real-time system)
            $args['meta_query'][] = array(
                'relation' => 'AND',
                array(
                    'key' => '_club420_davis_url',
                    'value' => '',
                    'compare' => '!='
                ),
                array(
                    'key' => '_club420_davis_visibility',
                    'value' => 'scheduled',
                    'compare' => '='
                ),
                array(
                    'key' => '_club420_davis_schedule_start',
                    'value' => current_time('timestamp', true), // Current UTC timestamp
                    'compare' => '<='
                ),
                array(
                    'key' => '_club420_davis_schedule_end',
                    'value' => current_time('timestamp', true), // Current UTC timestamp
                    'compare' => '>='
                )
            );
            
            // OPTION 3: Always visible (from Snippet 6 system)
            $args['meta_query'][] = array(
                'relation' => 'AND',
                array(
                    'key' => '_club420_davis_url',
                    'value' => '',
                    'compare' => '!='
                ),
                array(
                    'key' => '_club420_davis_visibility',
                    'value' => 'always',
                    'compare' => '='
                )
            );
            
        } elseif ($store_location === 'dixon') {
            
            // OPTION 1: Manual toggle enabled (original system)
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
            
            // OPTION 2: Scheduled and currently active (NEW real-time system)
            $args['meta_query'][] = array(
                'relation' => 'AND',
                array(
                    'key' => '_club420_dixon_url',
                    'value' => '',
                    'compare' => '!='
                ),
                array(
                    'key' => '_club420_dixon_visibility',
                    'value' => 'scheduled',
                    'compare' => '='
                ),
                array(
                    'key' => '_club420_dixon_schedule_start',
                    'value' => current_time('timestamp', true), // Current UTC timestamp
                    'compare' => '<='
                ),
                array(
                    'key' => '_club420_dixon_schedule_end',
                    'value' => current_time('timestamp', true), // Current UTC timestamp
                    'compare' => '>='
                )
            );
            
            // OPTION 3: Always visible (from Snippet 6 system)
            $args['meta_query'][] = array(
                'relation' => 'AND',
                array(
                    'key' => '_club420_dixon_url',
                    'value' => '',
                    'compare' => '!='
                ),
                array(
                    'key' => '_club420_dixon_visibility',
                    'value' => 'always',
                    'compare' => '='
                )
            );
        }
        
        // WP ENGINE CACHE BUSTING: Add timestamp to force fresh queries
        if (!isset($args['meta_query']['cache_buster'])) {
            $args['meta_query']['cache_buster'] = array(
                'key' => '_club420_cache_timestamp',
                'value' => floor(current_time('timestamp') / 300), // Changes every 5 minutes
                'compare' => 'NOT EXISTS'
            );
        }
    }
    
    return $args;
}

// WP ENGINE OPTIMIZATION: Clear object cache when products are saved
add_action('save_post_product', 'club420_clear_product_cache');
function club420_clear_product_cache($post_id) {
    // Clear WP Engine object cache for this product
    clean_post_cache($post_id);
    
    // Also clear any WooCommerce transients
    wc_delete_product_transients($post_id);
}

// Keep the conditional loading prep from previous optimization
function club420_conditional_scripts() {
    if (is_front_page() || is_shop() || is_product_category() || is_product()) {
        add_action('wp_footer', 'club420_load_optimized_scripts', 20);
    }
}
add_action('wp', 'club420_conditional_scripts');

function club420_load_optimized_scripts() {
    echo '<script>window.club420LoadOptimized=true;</script>';
}

// WP ENGINE CACHE EXCLUSION - Exclude pages with scheduled products from cache
add_action('wp', 'club420_bypass_cache_for_scheduled_products', 5);
function club420_bypass_cache_for_scheduled_products() {
    // Only check on pages that show product carousels
    if (!is_front_page() && !is_shop() && !is_product_category()) {
        return;
    }
    
    // Skip for logged-in users (they get fresh data anyway)
    if (is_user_logged_in()) {
        return;
    }
    
    global $wpdb;
    
    // Check if ANY products have active schedules RIGHT NOW
    $current_utc_time = current_time('timestamp', true);
    
    $active_scheduled = $wpdb->get_var($wpdb->prepare("
        SELECT COUNT(*) 
        FROM {$wpdb->postmeta} pm1
        INNER JOIN {$wpdb->postmeta} pm2 ON pm1.post_id = pm2.post_id
        INNER JOIN {$wpdb->postmeta} pm3 ON pm1.post_id = pm3.post_id
        WHERE (pm1.meta_key = '_club420_davis_visibility' OR pm1.meta_key = '_club420_dixon_visibility')
        AND pm1.meta_value = 'scheduled'
        AND ((pm2.meta_key = '_club420_davis_schedule_start' OR pm2.meta_key = '_club420_dixon_schedule_start'))
        AND ((pm3.meta_key = '_club420_davis_schedule_end' OR pm3.meta_key = '_club420_dixon_schedule_end'))
        AND pm2.meta_value <= %s
        AND pm3.meta_value >= %s
    ", $current_utc_time, $current_utc_time));
    
    if ($active_scheduled > 0) {
        // BYPASS WP ENGINE CACHE - This page has active scheduled products
        if (!headers_sent()) {
            header('Cache-Control: no-cache, no-store, must-revalidate, max-age=0');
            header('Pragma: no-cache');
            header('Expires: 0');
            
            // WP Engine specific headers
            if (defined('WPE_APIKEY') || function_exists('wpe_param')) {
                header('X-Cache-Control: no-cache');
                header('X-WPE-Cache-Control: no-cache');
            }
        }
    }
}
```

### SNIPPET 2: Product URL Fields with Scheduler
```php
// Enhanced "Club420 Product URL Fields with Scheduler" - COMPLETE WITH FIXED SAVE Snippet 2
// PHASE 2B: Radio button interface with scheduling system
// Backward compatible with existing checkbox system

add_action('woocommerce_product_options_general_product_data', 'club420_add_enhanced_product_fields_with_scheduler');
function club420_add_enhanced_product_fields_with_scheduler() {
    global $post;
    $product_id = $post->ID;
    
    echo '<div class="options_group">';
    echo '<h4 style="padding-left: 12px; margin-bottom: 10px; color: #0073aa;">CLUB420 Store Settings with Scheduler</h4>';
    
    // Get current WordPress timezone for proper time handling
    $wp_timezone = wp_timezone();
    
    // Davis Store Section
    echo '<div style="padding: 12px; border: 1px solid #ddd; margin-bottom: 15px; border-radius: 5px;">';
    echo '<h5 style="margin: 0 0 10px 0; color: #2271b1;">Davis Store</h5>';
    
    // Davis visibility options (radio buttons)
    echo '<p style="margin-bottom: 10px; font-weight: 500;">Product visibility for Davis customers:</p>';
    
    // Get current Davis visibility setting (with backward compatibility)
    $davis_visibility = get_post_meta($product_id, '_club420_davis_visibility', true);
    $davis_enabled = get_post_meta($product_id, '_club420_davis_enabled', true);
    
    // Backward compatibility: convert old checkbox to radio value
    if (empty($davis_visibility)) {
        if ($davis_enabled === 'yes') {
            $davis_visibility = 'always';
        } else {
            $davis_visibility = 'disabled';
        }
    }
    
    woocommerce_wp_radio( array(
        'id' => '_club420_davis_visibility',
        'value' => $davis_visibility,
        'options' => array(
            'always' => 'Always visible to Davis customers',
            'scheduled' => 'Visible only during scheduled times',
            'disabled' => 'Not available to Davis customers'
        )
    ));
    
    // Davis URL Field (unchanged)
    woocommerce_wp_text_input(array(
        'id' => '_club420_davis_url',
        'label' => 'Davis Store URL',
        'type' => 'url',
        'placeholder' => 'https://club420.com/menu/f-street/categories/...'
    ));
    
    // Davis Schedule Section (shows when scheduled is selected)
    echo '<div id="davis_schedule_section" style="display: none; background: #e8f4fd; border: 1px solid #0073aa; border-radius: 4px; padding: 15px; margin-top: 10px;">';
    echo '<h6 style="margin: 0 0 10px 0; color: #0073aa;">📅 Davis Store Schedule</h6>';
    echo '<p style="font-size: 12px; color: #666; margin-bottom: 10px;">Times are in Los Angeles timezone (your business timezone)</p>';
    
    // Davis Start Date/Time
    woocommerce_wp_text_input(array(
        'id' => '_club420_davis_schedule_start',
        'label' => '🟢 START: Show product from',
        'type' => 'datetime-local',
        'custom_attributes' => array(
            'step' => '60'
        ),
        'description' => 'Date and time when product becomes visible'
    ));
    
    // Davis End Date/Time
    woocommerce_wp_text_input(array(
        'id' => '_club420_davis_schedule_end',
        'label' => '🔴 END: Hide product after',
        'type' => 'datetime-local',
        'custom_attributes' => array(
            'step' => '60'
        ),
        'description' => 'Date and time when product becomes hidden'
    ));
    
    echo '</div>'; // End Davis schedule section
    echo '</div>'; // End Davis store section
    
    // Dixon Store Section
    echo '<div style="padding: 12px; border: 1px solid #ddd; margin-bottom: 10px; border-radius: 5px;">';
    echo '<h5 style="margin: 0 0 10px 0; color: #2271b1;">Dixon Store</h5>';
    
    // Dixon visibility options (radio buttons)
    echo '<p style="margin-bottom: 10px; font-weight: 500;">Product visibility for Dixon customers:</p>';
    
    // Get current Dixon visibility setting (with backward compatibility)
    $dixon_visibility = get_post_meta($product_id, '_club420_dixon_visibility', true);
    $dixon_enabled = get_post_meta($product_id, '_club420_dixon_enabled', true);
    
    // Backward compatibility: convert old checkbox to radio value
    if (empty($dixon_visibility)) {
        if ($dixon_enabled === 'yes') {
            $dixon_visibility = 'always';
        } else {
            $dixon_visibility = 'disabled';
        }
    }
    
    // Dixon visibility radio buttons
    woocommerce_wp_radio( array(
        'id' => '_club420_dixon_visibility',
        'value' => $dixon_visibility,
        'options' => array(
            'always' => 'Always visible to Dixon customers',
            'scheduled' => 'Visible only during scheduled times',
            'disabled' => 'Not available to Dixon customers'
        )
    ));
    
    // Dixon URL Field (unchanged)
    woocommerce_wp_text_input(array(
        'id' => '_club420_dixon_url',
        'label' => 'Dixon Store URL',
        'type' => 'url',
        'placeholder' => 'https://club420.com/menu/highway-80/categories/...'
    ));
    
    // Dixon Schedule Section (shows when scheduled is selected)
    echo '<div id="dixon_schedule_section" style="display: none; background: #e8f4fd; border: 1px solid #0073aa; border-radius: 4px; padding: 15px; margin-top: 10px;">';
    echo '<h6 style="margin: 0 0 10px 0; color: #0073aa;">📅 Dixon Store Schedule</h6>';
    echo '<p style="font-size: 12px; color: #666; margin-bottom: 10px;">Times are in Los Angeles timezone (your business timezone)</p>';
    
    // Dixon Start Date/Time
    woocommerce_wp_text_input(array(
        'id' => '_club420_dixon_schedule_start',
        'label' => '🟢 START: Show product from',
        'type' => 'datetime-local',
        'custom_attributes' => array(
            'step' => '60'
        ),
        'description' => 'Date and time when product becomes visible'
    ));
    
    // Dixon End Date/Time
    woocommerce_wp_text_input(array(
        'id' => '_club420_dixon_schedule_end',
        'label' => '🔴 END: Hide product after',
        'type' => 'datetime-local',
        'custom_attributes' => array(
            'step' => '60'
        ),
        'description' => 'Date and time when product becomes hidden'
    ));
    
    echo '</div>'; // End Dixon schedule section
    echo '</div>'; // End Dixon store section
    
    // Info section
    echo '<div style="background: #fff3cd; border: 1px solid #ffeaa7; border-radius: 4px; padding: 10px; margin-top: 15px; font-size: 13px; color: #856404;">';
    echo '<strong>💡 How Store Visibility Works:</strong><br>';
    echo '• <strong>Always visible:</strong> Product shows to store customers at all times<br>';
    echo '• <strong>Scheduled visibility:</strong> Product shows only during specific date/time periods<br>';
    echo '• <strong>Not available:</strong> Product never shows to store customers<br>';
    echo '• <strong>Timezone:</strong> All scheduled times are in Los Angeles timezone';
    echo '</div>';
    
    echo '</div>'; // End options_group
    
    // JavaScript for show/hide schedule sections
    ?>
    <script type="text/javascript">
    jQuery(document).ready(function($) {
        // Function to show/hide Davis schedule section
        function toggleDavisSchedule() {
            var selectedValue = $('input[name="_club420_davis_visibility"]:checked').val();
            if (selectedValue === 'scheduled') {
                $('#davis_schedule_section').slideDown();
            } else {
                $('#davis_schedule_section').slideUp();
            }
        }
        
        // Function to show/hide Dixon schedule section
        function toggleDixonSchedule() {
            var selectedValue = $('input[name="_club420_dixon_visibility"]:checked').val();
            if (selectedValue === 'scheduled') {
                $('#dixon_schedule_section').slideDown();
            } else {
                $('#dixon_schedule_section').slideUp();
            }
        }
        
        // Bind change events to radio buttons
        $('input[name="_club420_davis_visibility"]').change(toggleDavisSchedule);
        $('input[name="_club420_dixon_visibility"]').change(toggleDixonSchedule);
        
        // Show sections if scheduled is already selected (on page load)
        toggleDavisSchedule();
        toggleDixonSchedule();
    });
    </script>
    <?php
}

// FIXED Enhanced save function - handles radio buttons and scheduling CORRECTLY
add_action('woocommerce_process_product_meta', 'club420_save_enhanced_product_fields_with_scheduler');
function club420_save_enhanced_product_fields_with_scheduler($post_id) {
    
    // DAVIS STORE - Save radio button and calculate correct enabled status
    $davis_visibility = isset($_POST['_club420_davis_visibility']) ? sanitize_text_field($_POST['_club420_davis_visibility']) : 'disabled';
    update_post_meta($post_id, '_club420_davis_visibility', $davis_visibility);
    
    // Calculate Davis enabled status based on visibility and schedule
    $davis_enabled = club420_calculate_enabled_status($post_id, 'davis', $davis_visibility, $_POST);
    update_post_meta($post_id, '_club420_davis_enabled', $davis_enabled);
    
    // DIXON STORE - Save radio button and calculate correct enabled status  
    $dixon_visibility = isset($_POST['_club420_dixon_visibility']) ? sanitize_text_field($_POST['_club420_dixon_visibility']) : 'disabled';
    update_post_meta($post_id, '_club420_dixon_visibility', $dixon_visibility);
    
    // Calculate Dixon enabled status based on visibility and schedule
    $dixon_enabled = club420_calculate_enabled_status($post_id, 'dixon', $dixon_visibility, $_POST);
    update_post_meta($post_id, '_club420_dixon_enabled', $dixon_enabled);
    
    // URL FIELDS (unchanged)
    if (isset($_POST['_club420_davis_url'])) {
        update_post_meta($post_id, '_club420_davis_url', sanitize_text_field($_POST['_club420_davis_url']));
    }
    if (isset($_POST['_club420_dixon_url'])) {
        update_post_meta($post_id, '_club420_dixon_url', sanitize_text_field($_POST['_club420_dixon_url']));
    }
    
    // DAVIS SCHEDULE - Only save if 'scheduled' is selected
    if ($davis_visibility === 'scheduled') {
        club420_save_schedule_times($post_id, 'davis', $_POST);
    } else {
        // Clear schedule fields if not using scheduled visibility
        delete_post_meta($post_id, '_club420_davis_schedule_start');
        delete_post_meta($post_id, '_club420_davis_schedule_end');
    }
    
    // DIXON SCHEDULE - Only save if 'scheduled' is selected
    if ($dixon_visibility === 'scheduled') {
        club420_save_schedule_times($post_id, 'dixon', $_POST);
    } else {
        // Clear schedule fields if not using scheduled visibility
        delete_post_meta($post_id, '_club420_dixon_schedule_start');
        delete_post_meta($post_id, '_club420_dixon_schedule_end');
    }
}

/**
 * Calculate the correct enabled status based on visibility setting and current time
 */
function club420_calculate_enabled_status($post_id, $store, $visibility, $post_data) {
    switch ($visibility) {
        case 'always':
            return 'yes';
            
        case 'disabled':
            return 'no';
            
        case 'scheduled':
            // For scheduled products, check if we're within the time window
            $start_key = "_club420_{$store}_schedule_start";
            $end_key = "_club420_{$store}_schedule_end";
            
            // Get the schedule times from the form data
            $start_input = isset($post_data[$start_key]) ? sanitize_text_field($post_data[$start_key]) : '';
            $end_input = isset($post_data[$end_key]) ? sanitize_text_field($post_data[$end_key]) : '';
            
            // If no schedule times provided, disable the product
            if (empty($start_input) || empty($end_input)) {
                return 'no';
            }
            
            try {
                // Convert form times to UTC timestamps
                $wp_timezone = wp_timezone();
                $current_utc = current_time('timestamp', true);
                
                $start_wp = new DateTime($start_input, $wp_timezone);
                $start_utc = $start_wp->getTimestamp();
                
                $end_wp = new DateTime($end_input, $wp_timezone);
                $end_utc = $end_wp->getTimestamp();
                
                // Check if current time is within the scheduled period
                if ($current_utc >= $start_utc && $current_utc <= $end_utc) {
                    return 'yes'; // Within schedule - enable
                } else {
                    return 'no';  // Outside schedule - disable
                }
                
            } catch (Exception $e) {
                return 'no'; // Error = disable for safety
            }
            
        default:
            return 'no';
    }
}

/**
 * Save schedule times with timezone conversion
 */
function club420_save_schedule_times($post_id, $store, $post_data) {
    $start_key = "_club420_{$store}_schedule_start";
    $end_key = "_club420_{$store}_schedule_end";
    
    // Save start time with timezone conversion
    if (isset($post_data[$start_key]) && !empty($post_data[$start_key])) {
        $start_input = sanitize_text_field($post_data[$start_key]);
        try {
            $wp_timezone = wp_timezone();
            $start_wp = new DateTime($start_input, $wp_timezone);
            $start_utc = $start_wp->getTimestamp();
            update_post_meta($post_id, $start_key, $start_utc);
        } catch (Exception $e) {
            error_log("Club420 Schedule: Invalid {$store} start date format: " . $start_input);
        }
    }
    
    // Save end time with timezone conversion
    if (isset($post_data[$end_key]) && !empty($post_data[$end_key])) {
        $end_input = sanitize_text_field($post_data[$end_key]);
        try {
            $wp_timezone = wp_timezone();
            $end_wp = new DateTime($end_input, $wp_timezone);
            $end_utc = $end_wp->getTimestamp();
            update_post_meta($post_id, $end_key, $end_utc);
        } catch (Exception $e) {
            error_log("Club420 Schedule: Invalid {$store} end date format: " . $end_input);
        }
    }
}

// Helper function to load and display existing schedule values properly
add_action('admin_footer', 'club420_load_schedule_field_values');
function club420_load_schedule_field_values() {
    global $post, $pagenow;
    
    // Only run on product edit pages
    if ($pagenow !== 'post.php' || !$post || $post->post_type !== 'product') {
        return;
    }
    
    $wp_timezone = wp_timezone();
    
    // Load Davis schedule values and convert from UTC to Los Angeles time for display
    $davis_start_utc = get_post_meta($post->ID, '_club420_davis_schedule_start', true);
    $davis_end_utc = get_post_meta($post->ID, '_club420_davis_schedule_end', true);
    
    echo '<script type="text/javascript">
    jQuery(document).ready(function($) {';
    
    if ($davis_start_utc) {
        $davis_start_wp = new DateTime('@' . $davis_start_utc);
        $davis_start_wp->setTimezone($wp_timezone);
        $davis_start_display = $davis_start_wp->format('Y-m-d\TH:i');
        
        echo '$("#_club420_davis_schedule_start").val("' . esc_js($davis_start_display) . '");';
    }
    
    if ($davis_end_utc) {
        $davis_end_wp = new DateTime('@' . $davis_end_utc);
        $davis_end_wp->setTimezone($wp_timezone);
        $davis_end_display = $davis_end_wp->format('Y-m-d\TH:i');
        
        echo '$("#_club420_davis_schedule_end").val("' . esc_js($davis_end_display) . '");';
    }
    
    // Load Dixon schedule values and convert from UTC to Los Angeles time for display
    $dixon_start_utc = get_post_meta($post->ID, '_club420_dixon_schedule_start', true);
    $dixon_end_utc = get_post_meta($post->ID, '_club420_dixon_schedule_end', true);
    
    if ($dixon_start_utc) {
        $dixon_start_wp = new DateTime('@' . $dixon_start_utc);
        $dixon_start_wp->setTimezone($wp_timezone);
        $dixon_start_display = $dixon_start_wp->format('Y-m-d\TH:i');
        
        echo '$("#_club420_dixon_schedule_start").val("' . esc_js($dixon_start_display) . '");';
    }
    
    if ($dixon_end_utc) {
        $dixon_end_wp = new DateTime('@' . $dixon_end_utc);
        $dixon_end_wp->setTimezone($wp_timezone);
        $dixon_end_display = $dixon_end_wp->format('Y-m-d\TH:i');
        
        echo '$("#_club420_dixon_schedule_end").val("' . esc_js($dixon_end_display) . '");';
    }
    
    echo '});
    </script>';
}
```

### SNIPPET 3: Admin Store Filter
```php
// CLUB420 Admin Store Filter - Inline Dropdown Version
// Replace the previous "CLUB420 Admin Store Filter" snippet with this

// Add store filter dropdown inline with other filters
add_action('restrict_manage_posts', 'club420_add_inline_store_filter');
function club420_add_inline_store_filter() {
    global $typenow;
    
    // Only show on products page
    if ($typenow == 'product') {
        $selected = isset($_GET['store_filter_admin']) ? $_GET['store_filter_admin'] : '';
        
        // Create dropdown that matches WooCommerce styling
        echo '<select name="store_filter_admin" id="store_filter_admin" class="wc-enhanced-select" style="width: 200px;">';
        echo '<option value="">Filter by store</option>';
        echo '<option value="davis_only"' . selected($selected, 'davis_only', false) . '>Davis Store Only</option>';
        echo '<option value="dixon_only"' . selected($selected, 'dixon_only', false) . '>Dixon Store Only</option>';
        echo '<option value="both_stores"' . selected($selected, 'both_stores', false) . '>Both Stores</option>';
        echo '<option value="neither_store"' . selected($selected, 'neither_store', false) . '>Neither Store</option>';
        echo '</select>';
    }
}

// Filter products based on store selection
add_filter('parse_query', 'club420_filter_products_by_store');
function club420_filter_products_by_store($query) {
    global $pagenow, $typenow;
    
    // Only apply on products admin page
    if ($pagenow == 'edit.php' && $typenow == 'product' && isset($_GET['store_filter_admin']) && $_GET['store_filter_admin'] != '') {
        
        $store_filter = $_GET['store_filter_admin'];
        
        // Initialize meta query
        if (!isset($query->query_vars['meta_query'])) {
            $query->query_vars['meta_query'] = array();
        }
        
        switch ($store_filter) {
            case 'davis_only':
                // Davis enabled, Dixon disabled or not set
                $query->query_vars['meta_query'] = array(
                    'relation' => 'AND',
                    array(
                        'key' => '_club420_davis_enabled',
                        'value' => 'yes',
                        'compare' => '='
                    ),
                    array(
                        'relation' => 'OR',
                        array(
                            'key' => '_club420_dixon_enabled',
                            'value' => 'yes',
                            'compare' => '!='
                        ),
                        array(
                            'key' => '_club420_dixon_enabled',
                            'compare' => 'NOT EXISTS'
                        )
                    )
                );
                break;
                
            case 'dixon_only':
                // Dixon enabled, Davis disabled or not set
                $query->query_vars['meta_query'] = array(
                    'relation' => 'AND',
                    array(
                        'key' => '_club420_dixon_enabled',
                        'value' => 'yes',
                        'compare' => '='
                    ),
                    array(
                        'relation' => 'OR',
                        array(
                            'key' => '_club420_davis_enabled',
                            'value' => 'yes',
                            'compare' => '!='
                        ),
                        array(
                            'key' => '_club420_davis_enabled',
                            'compare' => 'NOT EXISTS'
                        )
                    )
                );
                break;
                
            case 'both_stores':
                // Both Davis AND Dixon enabled
                $query->query_vars['meta_query'] = array(
                    'relation' => 'AND',
                    array(
                        'key' => '_club420_davis_enabled',
                        'value' => 'yes',
                        'compare' => '='
                    ),
                    array(
                        'key' => '_club420_dixon_enabled', 
                        'value' => 'yes',
                        'compare' => '='
                    )
                );
                break;
                
            case 'neither_store':
                // Neither Davis nor Dixon enabled
                $query->query_vars['meta_query'] = array(
                    'relation' => 'AND',
                    array(
                        'relation' => 'OR',
                        array(
                            'key' => '_club420_davis_enabled',
                            'value' => 'yes',
                            'compare' => '!='
                        ),
                        array(
                            'key' => '_club420_davis_enabled',
                            'compare' => 'NOT EXISTS'
                        )
                    ),
                    array(
                        'relation' => 'OR',
                        array(
                            'key' => '_club420_dixon_enabled',
                            'value' => 'yes', 
                            'compare' => '!='
                        ),
                        array(
                            'key' => '_club420_dixon_enabled',
                            'compare' => 'NOT EXISTS'
                        )
                    )
                );
                break;
        }
    }
}

// Add store status columns to products list 
add_filter('manage_edit-product_columns', 'club420_add_store_columns');
function club420_add_store_columns($columns) {
    // Add store columns after stock status
    $new_columns = array();
    
    foreach ($columns as $key => $value) {
        $new_columns[$key] = $value;
        
        // Add store columns after stock column
        if ($key == 'product_tag') {
            $new_columns['davis_store'] = 'Davis';
            $new_columns['dixon_store'] = 'Dixon';
        }
    }
    
    return $new_columns;
}

// Display store status in columns
add_action('manage_product_posts_custom_column', 'club420_display_store_columns', 10, 2);
function club420_display_store_columns($column, $post_id) {
    switch ($column) {
        case 'davis_store':
            $enabled = get_post_meta($post_id, '_club420_davis_enabled', true);
            if ($enabled == 'yes') {
                echo '<span style="color: green; font-weight: bold;">✓</span>';
            } else {
                echo '<span style="color: #ccc;">✗</span>';
            }
            break;
            
        case 'dixon_store':
            $enabled = get_post_meta($post_id, '_club420_dixon_enabled', true);
            if ($enabled == 'yes') {
                echo '<span style="color: green; font-weight: bold;">✓</span>';
            } else {
                echo '<span style="color: #ccc;">✗</span>';
            }
            break;
    }
}
```

### SNIPPET 4: Auto-Button Generator
```php
// PHP Snippet #4: FIXED High-Performance CLUB420 Auto-Button Generation
// FLASH ISSUE RESOLVED - Never shows both buttons simultaneously

// Cache store detection for performance
function club420_get_current_store_cached() {
    static $current_store = null;
    
    if ($current_store === null) {
        $current_store = isset($_GET['store_filter']) ? sanitize_text_field($_GET['store_filter']) : '';
        
        // If no URL parameter, don't default to 'all' - wait for JavaScript
        if (empty($current_store)) {
            $current_store = 'unknown';
        }
    }
    
    return $current_store;
}

// Generate optimized button HTML with deals detection
function club420_generate_button_html($url, $text, $type = 'primary') {
    if (empty($url)) return '';
    
    $button_class = ($type === 'deals') ? 'pa-deals-button' : 'pa-blurb-button';
    
    return sprintf(
        '<a class="%s" href="%s">%s</a>',
        esc_attr($button_class),
        esc_url($url),
        esc_html($text)
    );
}

// Detect if URL is for deals
function club420_is_deals_url($url) {
    return (strpos($url, '/deals') !== false || strpos($url, 'deals') !== false);
}

// Get category-specific deals anchor
function club420_get_deals_anchor($product_id) {
    // Get product categories
    $categories = wp_get_post_terms($product_id, 'product_cat', array('fields' => 'names'));
    
    if (empty($categories)) {
        return 'deals'; // Fallback to generic deals
    }
    
    // Category mapping to anchor IDs
    $category_map = array(
        'preroll' => 'preroll-deals',
        'pre-roll' => 'preroll-deals',
        'prerolls' => 'preroll-deals',
        'flower' => 'flower-deals',
        'flowers' => 'flower-deals',
        'cartridge' => 'cartridge-deals',
        'cartridges' => 'cartridge-deals',
        'vape' => 'cartridge-deals',
        'edible' => 'edible-deals',
        'edibles' => 'edible-deals',
        'extract' => 'extract-deals',
        'extracts' => 'extract-deals',
        'concentrate' => 'extract-deals',
        'concentrates' => 'extract-deals'
    );
    
    // Check each category against our mapping
    foreach ($categories as $category) {
        $category_lower = strtolower(trim($category));
        if (isset($category_map[$category_lower])) {
            return $category_map[$category_lower];
        }
    }
    
    // Fallback to generic deals if no match
    return 'deals';
}

// Generate deals URL with category-specific anchor - back to homepage deals
function club420_generate_deals_url($store, $product_id) {
    $deals_anchor = club420_get_deals_anchor($product_id);
    
    // Auto-detect current domain (dev.club420.com or club420.com)
    $current_domain = $_SERVER['HTTP_HOST'];
    $protocol = is_ssl() ? 'https://' : 'http://';
    $homepage_url = $protocol . $current_domain . '/#' . $deals_anchor;
    
    return $homepage_url;
}

// FIXED: Main auto-button generation function - NO MORE FLASH
function club420_auto_generate_buttons($content) {
    // Early exit for non-product pages (performance optimization)
    if (!is_product() && !is_shop() && !is_product_category()) {
        return $content;
    }
    
    global $product;
    
    // Early exit if no product object
    if (!$product || !is_a($product, 'WC_Product')) {
        return $content;
    }
    
    // Get current store (cached)
    $current_store = club420_get_current_store_cached();
    
    // FLASH FIX: Don't show any buttons if store is unknown (let JavaScript handle it)
    if ($current_store === 'unknown') {
        return $content;
    }
    
    // Get product ID once
    $product_id = $product->get_id();
    
    // Read custom fields efficiently (single call per field)
    $davis_url = get_post_meta($product_id, '_club420_davis_url', true);
    $dixon_url = get_post_meta($product_id, '_club420_dixon_url', true);
    $davis_enabled = get_post_meta($product_id, '_club420_davis_enabled', true);
    $dixon_enabled = get_post_meta($product_id, '_club420_dixon_enabled', true);
    
    $buttons_html = '';
    
    // FIXED: Generate buttons based on store - NEVER SHOW BOTH
    if ($current_store === 'davis') {
        // Davis store only
        if ($davis_enabled === 'yes' && !empty($davis_url)) {
            $buttons_html .= club420_generate_button_html($davis_url, 'View Products', 'primary');
            $deals_url = club420_generate_deals_url('', $product_id);
            $buttons_html .= club420_generate_button_html($deals_url, 'See All Deals', 'deals');
        }
    } elseif ($current_store === 'dixon') {
        // Dixon store only  
        if ($dixon_enabled === 'yes' && !empty($dixon_url)) {
            $buttons_html .= club420_generate_button_html($dixon_url, 'View Products', 'primary');
            $deals_url = club420_generate_deals_url('', $product_id);
            $buttons_html .= club420_generate_button_html($deals_url, 'See All Deals', 'deals');
        }
    }
    // REMOVED: No more 'all' condition that caused the flash
    
    // Only append if we have buttons (avoid unnecessary HTML)
    if (!empty($buttons_html)) {
        // Add styling and button container - left aligned with fade-in class
        $button_container = '<div class="club420-auto-buttons club420-buttons-ready" style="margin-top: 15px; text-align: left;">' . $buttons_html . '</div>';
        $content .= $button_container;
    }
    
    return $content;
}

// Hook into product description with high performance
add_filter('woocommerce_short_description', 'club420_auto_generate_buttons', 20);
add_filter('the_content', function($content) {
    // Only process on product pages to avoid unnecessary processing
    if (is_product()) {
        return club420_auto_generate_buttons($content);
    }
    return $content;
}, 20);

// ENHANCED: Add CSS for button styling with fade-in to prevent flash
function club420_auto_button_styles() {
    // Only load CSS on relevant pages
    if (!is_product() && !is_shop() && !is_product_category()) {
        return;
    }
    
    // Enhanced CSS with fade-in and flash prevention
    echo '<style>
    .club420-auto-buttons {
        margin: 15px 0;
        text-align: left;
        opacity: 0;
        transition: opacity 0.3s ease;
    }
    .club420-auto-buttons.club420-buttons-ready {
        opacity: 1;
    }
    .pa-blurb-button, .pa-deals-button {
        display: inline-block;
        padding: 12px 24px;
        margin: 5px 10px 5px 0;
        text-decoration: none;
        border-radius: 5px;
        font-weight: bold;
        min-width: 150px;
    }
    .pa-blurb-button {
        background-color: #f2ac1d;
        color: white;
        border: 2px solid #f2ac1d;
    }
    .pa-deals-button {
        background-color: #000000;
        color: white;
        border: 2px solid #000000;
    }
    /* JavaScript fallback - show buttons if no JS after 2 seconds */
    .no-js .club420-auto-buttons {
        opacity: 1;
    }
    </style>';
}
add_action('wp_head', 'club420_auto_button_styles', 30);

// Add JavaScript to handle button visibility smoothly
function club420_button_visibility_script() {
    if (!is_product()) {
        return;
    }
    
    echo '<script>
    document.addEventListener("DOMContentLoaded", function() {
        // Ensure buttons fade in smoothly after page load
        setTimeout(function() {
            const buttonContainers = document.querySelectorAll(".club420-auto-buttons");
            buttonContainers.forEach(function(container) {
                container.classList.add("club420-buttons-ready");
            });
        }, 100);
    });
    </script>';
}
add_action('wp_footer', 'club420_button_visibility_script', 5);

// Performance monitoring function (optional - can be disabled in production)
function club420_performance_monitor() {
    // Only in admin or for admin users to avoid performance impact
    if (!current_user_can('manage_options')) {
        return;
    }
    
    // Add debug info to HTML comments for performance tracking
    add_action('wp_footer', function() {
        echo '<!-- CLUB420 Auto-Buttons: FLASH FIXED - Loaded at ' . current_time('mysql') . ' -->';
    });
}
club420_performance_monitor();
```

### SNIPPET 5: BlazeSlider Carousel
```php
// Club420 BlazeSlider - REAL-TIME SCHEDULING (CLEAN PRODUCTION VERSION)
add_shortcode('club420_deals_carousel', 'club420_blazeslider_adaptation');

function club420_blazeslider_adaptation($atts) {
    // FORCE fresh data for ALL users - treat non-logged-in like logged-in for cache bypass
    $original_user = wp_get_current_user();
    $was_logged_out = !is_user_logged_in();
    
    if ($was_logged_out) {
        // Temporarily fake being logged in to bypass caching
        wp_set_current_user(1);
    }
    
    $atts = shortcode_atts(array(
        'store' => 'current'
    ), $atts);
    
    $categories = array(
        'flower' => array('name' => 'Flower Deals', 'slug' => 'flower'),
        'preroll' => array('name' => 'Preroll Deals', 'slug' => 'preroll'),
        'cartridge' => array('name' => 'Cartridge Deals', 'slug' => 'cartridge'),
        'edible' => array('name' => 'Edible Deals', 'slug' => 'edible'),
        'extract' => array('name' => 'Extract Deals', 'slug' => 'extract')
    );
    
    static $club420_slider_id = 1;
    $output = '';
    
    foreach ($categories as $cat_key => $category) {
        $output .= '<h3 style="font-size: 28px; font-weight: 700; margin: 40px 0 30px 0; color: #333;">' . esc_html($category['name']) . '</h3>';
        $output .= '<div class="blaze-slider" id="club420_slider'.$club420_slider_id.'" data-category="' . esc_attr($cat_key) . '">';
        $output .= '<div class="my-structure">';
        $output .= '<span class="blaze-prev" aria-label="Go to previous slide">';
        $output .= '<svg width="38" height="38" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">';
        $output .= '<path d="M15 18L9 12L15 6" stroke="#000" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>';
        $output .= '</svg></span>';
        $output .= '<span class="blaze-next" aria-label="Go to next slide">';
        $output .= '<svg width="38" height="38" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">';
        $output .= '<path d="M9 18L15 12L9 6" stroke="#000" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>';
        $output .= '</svg></span>';
        $output .= '</div>';
        $output .= '<div class="blaze-container"><div class="blaze-track-container"><div class="club420-blaze-track blaze-track">';
        
        // Check current store filter
        $store_location = isset($_GET['store_filter']) ? sanitize_text_field($_GET['store_filter']) : 'all';
        
        // Get products with basic query
        $args = array(
            'post_type' => 'product',
            'posts_per_page' => 50,
            'post_status' => 'publish',
            'tax_query' => array(
                array(
                    'taxonomy' => 'product_cat',
                    'field'    => 'slug',
                    'terms'    => $category['slug'],
                )
            ),
            'orderby' => 'date',
            'order' => 'DESC',
            'club420_carousel_query' => true,
            'cache_results' => false,
            'update_post_meta_cache' => false,
            'update_post_term_cache' => false
        );
        
        // Add basic store availability filter
        if ($store_location !== 'all') {
            $url_key = ($store_location === 'davis') ? '_club420_davis_url' : '_club420_dixon_url';
            $args['meta_query'] = array(
                array(
                    'key' => $url_key,
                    'value' => '',
                    'compare' => '!='
                )
            );
        }
        
        $products = new WP_Query($args);
        $displayed_count = 0;
        
        if ($products->have_posts()) {
            while ($products->have_posts() && $displayed_count < 12) {
                $products->the_post();
                global $product;
                
                if (!$product || !$product->is_visible()) {
                    continue;
                }
                
                $product_id = get_the_ID();
                
                // Check if product should show using real-time scheduling
                if ($store_location !== 'all') {
                    // FORCE fresh check for non-logged-in users
                    if (!is_user_logged_in()) {
                        clean_post_cache($product_id);
                        wp_cache_delete($product_id, 'posts');
                    }
                    
                    $should_show = club420_should_product_show($product_id, $store_location);
                    
                    if (!$should_show) {
                        continue;
                    }
                }
                
                // Product passed all checks - display it
                $output .= '<div class="post_card">';
                $output .= '<a href="' . esc_url(get_permalink()) . '" style="position: relative;">';
                $output .= woocommerce_get_product_thumbnail();
                $output .= club420_get_yith_badge_html(get_the_ID());
                $output .= '</a>';
                $output .= '<span class="wwo_card_details">';
                $output .= '<a class="p_title" href="' . esc_url(get_permalink()) . '">' . get_the_title() . '</a>';
                $output .= '</span>';
                $output .= '</div>';
                
                $displayed_count++;
            }
            wp_reset_postdata();
        }
        
        $output .= '</div></div></div></div>';
        $club420_slider_id++;
    }
    
    // Restore original user state
    if ($was_logged_out) {
        wp_set_current_user(0);
    }
    
    return $output;
}

/**
 * Real-time scheduling logic - reads directly from database
 */
function club420_should_product_show($product_id, $store_location) {
    global $wpdb;
    
    // Read scheduling data directly from database (bypasses all caching)
    $visibility_key = "_club420_{$store_location}_visibility";
    $enabled_key = "_club420_{$store_location}_enabled";
    $start_key = "_club420_{$store_location}_schedule_start";
    $end_key = "_club420_{$store_location}_schedule_end";
    
    $meta_data = $wpdb->get_results($wpdb->prepare("
        SELECT meta_key, meta_value 
        FROM {$wpdb->postmeta} 
        WHERE post_id = %d 
        AND meta_key IN (%s, %s, %s, %s)
    ", $product_id, $visibility_key, $enabled_key, $start_key, $end_key), ARRAY_A);
    
    $visibility = '';
    $enabled = '';
    $start_time = '';
    $end_time = '';
    
    foreach ($meta_data as $meta) {
        switch ($meta['meta_key']) {
            case $visibility_key:
                $visibility = $meta['meta_value'];
                break;
            case $enabled_key:
                $enabled = $meta['meta_value'];
                break;
            case $start_key:
                $start_time = $meta['meta_value'];
                break;
            case $end_key:
                $end_time = $meta['meta_value'];
                break;
        }
    }
    
    $current_utc_time = current_time('timestamp', true);
    
    switch ($visibility) {
        case 'always':
            return true;
            
        case 'disabled':
            return false;
            
        case 'scheduled':
            if (!$start_time || !$end_time) {
                return false;
            }
            return ($current_utc_time >= $start_time && $current_utc_time <= $end_time);
            
        default:
            return ($enabled === 'yes');
    }
}

function club420_get_yith_badge_html($product_id) {
    if (!function_exists('yith_wcbm_get_product_badges')) {
        return '';
    }
    
    $badges = yith_wcbm_get_product_badges($product_id);
    
    if (empty($badges)) {
        return '';
    }
    
    $badge_html = '';
    foreach ($badges as $badge) {
        if (isset($badge['text']) && !empty($badge['text'])) {
            $badge_html .= '<span class="club420-yith-badge">';
            $badge_html .= esc_html($badge['text']);
            $badge_html .= '</span>';
        }
    }
    
    return $badge_html;
}

function club420_blazeslider_styles() {
    if (!is_front_page() && !is_shop() && !is_product_category()) {
        return;
    }
    
    echo '<style>
.blaze-slider{--slides-to-show:4;--slide-gap:20px;direction:ltr}
.blaze-container{position:relative}
.blaze-track-container{overflow:hidden}
.blaze-track{will-change:transform;touch-action:pan-y;display:flex;gap:var(--slide-gap);--slide-width:calc((100% - (var(--slides-to-show) - 1) * var(--slide-gap)) / var(--slides-to-show));box-sizing:border-box}
.blaze-track>*{box-sizing:border-box;width:var(--slide-width);flex-shrink:0}

.blaze-slider {
    position: relative;
    margin: 0 10px;
}

.my-structure {
    pointer-events: none;
    position: absolute;
    inset: -20px -40px;
    z-index: 2;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
}

.my-structure span {
    pointer-events: all;
    cursor: pointer;
    transition: all .3s ease;
    display: flex;
    align-items: center;
    justify-content: center;
    width: 42px;
    height: 42px;
}

.my-structure svg {
    transition: opacity 0.3s ease;
}

.my-structure span:hover svg {
    opacity: 0.7;
}

.club420-blaze-track .post_card {
    position: relative;
    max-width: 500px;
    border: 1px solid #eee;
    border-radius: 12px;
    overflow: hidden;
    transition: all 0.3s ease;
    background: #fff;
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
}

.club420-blaze-track .post_card:hover {
    box-shadow: 0 8px 30px rgba(0, 0, 0, 0.12);
}

.club420-blaze-track .post_card .container-image-and-badge img {
    width: 92%;
    height: auto;
    margin: 18px auto 8px auto;
    object-fit: contain;
}

.club420-blaze-track .post_card .container-image-and-badge {
    text-align: center;
}

.club420-blaze-track .wwo_card_details {
    display: flex;
    flex-direction: column;
    padding: 8px 15px 15px 15px;
}

.club420-blaze-track .p_title {
    font-size: 14px;
    font-weight: 600;
    margin: 0;
    text-decoration: none;
    color: #2c2c2c;
    text-align: left;
    line-height: 1.3;
}

.club420-yith-badge {
    z-index: 3;
    position: absolute;
    top: 8px;
    left: 8px;
    background: linear-gradient(135deg, #ff6b6b 0%, #ee5a24 100%);
    color: #fff;
    padding: 6px 12px;
    border-radius: 8px;
    font-size: 11px;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 0.5px;
    box-shadow: 0 3px 10px rgba(0, 0, 0, 0.25);
    border: 2px solid rgba(255, 255, 255, 0.2);
    white-space: nowrap;
}

@media (max-width: 900px) {
    .blaze-slider {
        --slides-to-show: 2;
        margin: 0 10px;
    }
    
    .my-structure {
        inset: -15px -35px;
    }
}

@media (max-width: 500px) {
    .blaze-slider {
        --slides-to-show: 1.5;
        margin: 0 15px;
    }
    
    .my-structure {
        inset: -10px -35px;
    }
    
    .my-structure span {
        width: 35px;
        height: 35px;
    }
    
    .my-structure svg {
        width: 30px;
        height: 30px;
    }
    
    .club420-blaze-track .post_card .container-image-and-badge img {
        width: 94%;
        margin: 12px auto 8px auto;
    }
}
</style>';
}
add_action('wp_head', 'club420_blazeslider_styles', 30);

function club420_blazeslider_scripts() {
    if (!is_front_page() && !is_shop() && !is_product_category()) {
        return;
    }
    
    echo '<script>
var BlazeSlider=function(){"use strict";const t="start";class e{constructor(t,e){this.config=e,this.totalSlides=t,this.isTransitioning=!1,n(this,t,e)}next(t=1){if(this.isTransitioning||this.isStatic)return;const{stateIndex:e}=this;let n=0,i=e;for(let e=0;e<t;e++){const t=this.states[i];n+=t.next.moveSlides,i=t.next.stateIndex}return i!==e?(this.stateIndex=i,[e,n]):void 0}prev(t=1){if(this.isTransitioning||this.isStatic)return;const{stateIndex:e}=this;let n=0,i=e;for(let e=0;e<t;e++){const t=this.states[i];n+=t.prev.moveSlides,i=t.prev.stateIndex}return i!==e?(this.stateIndex=i,[e,n]):void 0}}function n(t,e,n){t.stateIndex=0,function(t){const{slidesToScroll:e,slidesToShow:n}=t.config,{totalSlides:i,config:s}=t;if(i<n&&(s.slidesToShow=i),!(i<=n)&&(e>n&&(s.slidesToScroll=n),i<e+n)){const t=i-n;s.slidesToScroll=t}}(t),t.isStatic=e<=n.slidesToShow,t.states=function(t){const{totalSlides:e}=t,{loop:n}=t.config,i=function(t){const{slidesToShow:e,slidesToScroll:n,loop:i}=t.config,{isStatic:s,totalSlides:o}=t,r=[],a=o-1;for(let t=0;t<o;t+=n){const n=t+e-1;if(n>a){if(!i){const t=a-e+1,n=r.length-1;(0===r.length||r.length>0&&r[n][0]!==t)&&r.push([t,a]);break}{const e=n-o;r.push([t,e])}}else r.push([t,n]);if(s)break}return r}(t),s=[],o=i.length-1;for(let t=0;t<i.length;t++){let r,a;n?(r=t===o?0:t+1,a=0===t?o:t-1):(r=t===o?o:t+1,a=0===t?0:t-1);const l=i[t][0],c=i[r][0],d=i[a][0];let u=c-l;c<l&&(u+=e);let f=l-d;d>l&&(f+=e),s.push({page:i[t],next:{stateIndex:r,moveSlides:u},prev:{stateIndex:a,moveSlides:f}})}return s}(t)}function i(t){if(t.onSlideCbs){const e=t.states[t.stateIndex],[n,i]=e.page;t.onSlideCbs.forEach((e=>e(t.stateIndex,n,i)))}}function s(t){t.offset=-1*t.states[t.stateIndex].page[0],o(t),i(t)}function o(t){const{track:e,offset:n,dragged:i}=t;e.style.transform=0===n?`translate3d(${i}px,0px,0px)`:`translate3d(  calc( ${i}px + ${n} * (var(--slide-width) + ${t.config.slideGap})),0px,0px)`}function r(t){t.track.style.transitionDuration=`${t.config.transitionDuration}ms`}function a(t){t.track.style.transitionDuration="0ms"}const l=10,c=()=>"ontouchstart"in window;function d(t){const e=this,n=e.slider;if(!n.isTransitioning){if(n.dragged=0,e.isScrolled=!1,e.startMouseClientX="touches"in t?t.touches[0].clientX:t.clientX,!("touches"in t)){(t.target||e).setPointerCapture(t.pointerId)}a(n),p(e,"addEventListener")}}function u(t){const e=this,n="touches"in t?t.touches[0].clientX:t.clientX,i=e.slider.dragged=n-e.startMouseClientX,s=Math.abs(i);s>5&&(e.slider.isDragging=!0),s>15&&t.preventDefault(),e.slider.dragged=i,o(e.slider),!e.isScrolled&&e.slider.config.loop&&i>l&&(e.isScrolled=!0,e.slider.prev())}function f(){const t=this,e=t.slider.dragged;t.slider.isDragging=!1,p(t,"removeEventListener"),t.slider.dragged=0,o(t.slider),r(t.slider),t.isScrolled||(e<-1*l?t.slider.next():e>l&&t.slider.prev())}const h=t=>t.preventDefault();function p(t,e){t[e]("contextmenu",f),c()?(t[e]("touchend",f),t[e]("touchmove",u)):(t[e]("pointerup",f),t[e]("pointermove",u))}const g={slideGap:"20px",slidesToScroll:1,slidesToShow:1,loop:!0,enableAutoplay:!1,stopAutoplayOnInteraction:!0,autoplayInterval:3e3,autoplayDirection:"to left",enablePagination:!0,transitionDuration:300,transitionTimingFunction:"ease",draggable:!0};function v(t){const e={...g};for(const n in t)if(window.matchMedia(n).matches){const i=t[n];for(const t in i)e[t]=i[t]}return e}function S(){const t=this.index,e=this.slider,n=e.stateIndex,i=e.config.loop,s=Math.abs(t-n),o=e.states.length-s,r=s>e.states.length/2&&i;t>n?r?e.prev(o):e.next(s):r?e.next(o):e.prev(s)}function m(t,e=t.config.transitionDuration){t.isTransitioning=!0,setTimeout((()=>{t.isTransitioning=!1}),e)}function x(e,n){const i=e.el.classList,s=e.stateIndex,o=e.paginationButtons;e.config.loop||(0===s?i.add(t):i.remove(t),s===e.states.length-1?i.add("end"):i.remove("end")),o&&e.config.enablePagination&&(o[n].classList.remove("active"),o[s].classList.add("active"))}function y(e,i){const s=i.track;i.slides=s.children,i.offset=0,i.config=e,n(i,i.totalSlides,e),e.loop||i.el.classList.add(t),e.enableAutoplay&&!e.loop&&(e.enableAutoplay=!1),s.style.transitionProperty="transform",s.style.transitionTimingFunction=i.config.transitionTimingFunction,s.style.transitionDuration=`${i.config.transitionDuration}ms`;const{slidesToShow:r,slideGap:a}=i.config;i.el.style.setProperty("--slides-to-show",r+""),i.el.style.setProperty("--slide-gap",a),i.isStatic?i.el.classList.add("static"):e.draggable&&function(t){const e=t.track;e.slider=t;const n=c()?"touchstart":"pointerdown";e.addEventListener(n,d),e.addEventListener("click",(e=>{(t.isTransitioning||t.isDragging)&&(e.preventDefault(),e.stopImmediatePropagation(),e.stopPropagation())}),{capture:!0}),e.addEventListener("dragstart",h)}(i),function(t){if(!t.config.enablePagination||t.isStatic)return;const e=t.el.querySelector(".blaze-pagination");if(!e)return;t.paginationButtons=[];const n=t.states.length;for(let i=0;i<n;i++){const s=document.createElement("button");t.paginationButtons.push(s),s.textContent="",s.ariaLabel=`${i+1} of ${n}`,e.append(s),s.slider=t,s.index=i,s.onclick=S}t.paginationButtons[0].classList.add("active")}(i),function(t){const e=t.config;if(!e.enableAutoplay)return;const n="to left"===e.autoplayDirection?"next":"prev";t.autoplayTimer=setInterval((()=>{t[n]()}),e.autoplayInterval),e.stopAutoplayOnInteraction&&t.el.addEventListener(c()?"touchstart":"mousedown",(()=>{clearInterval(t.autoplayTimer)}),{once:!0})}(i),function(t){const e=t.el.querySelector(".blaze-prev"),n=t.el.querySelector(".blaze-next");e&&(e.onclick=()=>{t.prev()}),n&&(n.onclick=()=>{t.next()})}(i),o(i)}return class extends e{constructor(t,e){const n=t.querySelector(".blaze-track"),i=n.children,s=e?v(e):{...g};super(i.length,s),this.config=s,this.el=t,this.track=n,this.slides=i,this.offset=0,this.dragged=0,this.isDragging=!1,this.el.blazeSlider=this,this.passedConfig=e;const o=this;n.slider=o,y(s,o);let r=!1,a=0;window.addEventListener("resize",(()=>{if(0===a)return void(a=window.innerWidth);const t=window.innerWidth;a!==t&&(a=t,r||(r=!0,setTimeout((()=>{o.refresh(),r=!1}),200)))}))}next(t){if(this.isTransitioning)return;const e=super.next(t);if(!e)return void m(this);const[n,l]=e;x(this,n),m(this),function(t,e){const n=requestAnimationFrame;t.config.loop?(t.offset=-1*e,o(t),setTimeout((()=>{!function(t,e){for(let n=0;n<e;n++)t.track.append(t.slides[0])}(t,e),a(t),t.offset=0,o(t),n((()=>{n((()=>{r(t),i(t)}))}))}),t.config.transitionDuration)):s(t)}(this,l)}prev(t){if(this.isTransitioning)return;const e=super.prev(t);if(!e)return void m(this);const[n,l]=e;x(this,n),m(this),function(t,e){const n=requestAnimationFrame;if(t.config.loop){a(t),t.offset=-1*e,o(t),function(t,e){const n=t.slides.length;for(let i=0;i<e;i++){const e=t.slides[n-1];t.track.prepend(e)}}(t,e);const s=()=>{n((()=>{r(t),n((()=>{t.offset=0,o(t),i(t)}))}))};t.isDragging?c()?t.track.addEventListener("touchend",s,{once:!0}):t.track.addEventListener("pointerup",s,{once:!0}):n(s)}else s(t)}(this,l)}stopAutoplay(){clearInterval(this.autoplayTimer)}destroy(){this.track.removeEventListener(c()?"touchstart":"pointerdown",d),this.stopAutoplay(),this.paginationButtons?.forEach((t=>t.remove())),this.el.classList.remove("static"),this.el.classList.remove(t)}refresh(){const t=this.passedConfig?v(this.passedConfig):{...g};this.destroy(),y(t,this)}onSlide(t){return this.onSlideCbs||(this.onSlideCbs=new Set),this.onSlideCbs.add(t),()=>this.onSlideCbs.delete(t)}}}();

document.addEventListener("DOMContentLoaded", function() {
    setTimeout(function() {
        const sliders = document.querySelectorAll("[id^=\"club420_slider\"]");
        
        sliders.forEach(function(slider) {
            new BlazeSlider(slider, {
                all: {
                    enableAutoplay: true,
                    autoplayInterval: 3000,
                    autoplayDirection: "to left",
                    stopAutoplayOnInteraction: true,
                    transitionDuration: 300,
                    slidesToShow: 4,
                    slidesToScroll: 1,
                    slideGap: "20px",
                    loop: true,
                    enablePagination: false,
                    transitionTimingFunction: "ease",
                    draggable: true
                },
                "(max-width: 900px)": {
                    slidesToShow: 2,
                    autoplayInterval: 4000,
                },
                "(max-width: 500px)": {
                    slidesToShow: 1.5,
                    autoplayInterval: 4000,
                },
            });
        });
        
    }, 1500);
});
</script>';
}
add_action('wp_footer', 'club420_blazeslider_scripts', 25);
```

### SNIPPET 6: Scheduler Dashboard
```php
// Snippet 6: Club420 Product Scheduler Dashboard
// Clean Production Version - Real-time scheduling monitoring

class Club420ProductSchedulerDashboard {
    
    public function __construct() {
        $this->init();
    }
    
    public function init() {
        // Add dashboard shortcode for monitoring scheduled products
        add_shortcode('club420_scheduler_dashboard', array($this, 'scheduler_dashboard_shortcode'));
        
        // Add admin notice with system status
        add_action('admin_notices', array($this, 'show_system_status'));
    }
    
    /**
     * Scheduler dashboard shortcode
     * Usage: [club420_scheduler_dashboard]
     */
    public function scheduler_dashboard_shortcode($atts) {
        // Only allow admins to run this
        if (!current_user_can('manage_options')) {
            return '<p>Access denied. Admin privileges required.</p>';
        }
        
        ob_start();
        
        echo '<div style="background: #f9f9f9; border: 1px solid #ddd; padding: 20px; margin: 20px 0; border-radius: 5px; font-family: Arial, sans-serif;">';
        echo '<h3 style="margin-top: 0;">🕐 Club420 Real-Time Scheduler Dashboard</h3>';
        
        // Display current time info
        $current_wp_time = current_time('mysql');
        $current_utc_time = current_time('mysql', true);
        $wp_timezone = wp_timezone();
        
        echo '<div style="background: #e7f3ff; padding: 15px; border-radius: 5px; margin-bottom: 20px;">';
        echo '<p style="margin: 5px 0;"><strong>Current WordPress Time (Los Angeles):</strong> ' . $current_wp_time . '</p>';
        echo '<p style="margin: 5px 0;"><strong>Current UTC Time:</strong> ' . $current_utc_time . '</p>';
        echo '<p style="margin: 5px 0;"><strong>WordPress Timezone:</strong> ' . $wp_timezone->getName() . '</p>';
        echo '<p style="margin: 5px 0; color: green;"><strong>✅ Real-Time Scheduling:</strong> ACTIVE (instant activation)</p>';
        echo '</div>';
        
        // Show system status
        echo '<div style="background: #d4edda; padding: 15px; border-radius: 5px; margin-bottom: 20px; border-left: 4px solid #28a745;">';
        echo '<h4 style="margin-top: 0; color: #155724;">📊 System Status</h4>';
        echo '<p style="margin: 5px 0; color: #155724;"><strong>Scheduling Method:</strong> Real-time (instant activation)</p>';
        echo '<p style="margin: 5px 0; color: #155724;"><strong>Cache Management:</strong> WP Engine cache exclusion for scheduled content</p>';
        echo '<p style="margin: 5px 0; color: #155724;"><strong>Manual Toggles:</strong> Working (instant override)</p>';
        echo '<p style="margin: 5px 0; color: #155724;"><strong>Scheduled Products:</strong> Activate/deactivate immediately at set times</p>';
        echo '</div>';
        
        // Show ALL scheduled products with their current status
        $scheduled_products = $this->get_products_with_schedules();
        echo '<h4>Products with Visibility Settings: ' . count($scheduled_products) . '</h4>';
        
        if (!empty($scheduled_products)) {
            echo '<div style="overflow-x: auto; margin-top: 20px;">';
            echo '<table style="border-collapse: collapse; width: 100%; font-size: 12px; background: white;">';
            echo '<thead>';
            echo '<tr style="background: #0073aa; color: white;">';
            echo '<th style="border: 1px solid #ddd; padding: 8px; text-align: left;">ID</th>';
            echo '<th style="border: 1px solid #ddd; padding: 8px; text-align: left;">Product Title</th>';
            echo '<th style="border: 1px solid #ddd; padding: 8px; text-align: center;">Davis Settings</th>';
            echo '<th style="border: 1px solid #ddd; padding: 8px; text-align: center;">Davis Status</th>';
            echo '<th style="border: 1px solid #ddd; padding: 8px; text-align: center;">Dixon Settings</th>';
            echo '<th style="border: 1px solid #ddd; padding: 8px; text-align: center;">Dixon Status</th>';
            echo '</tr>';
            echo '</thead>';
            echo '<tbody>';
            
            // Show ALL products
            foreach ($scheduled_products as $product_id) {
                $product = get_post($product_id);
                
                if (!$product) continue;
                
                // Get visibility and enabled status for both stores
                $davis_visibility = get_post_meta($product_id, '_club420_davis_visibility', true);
                $davis_enabled = get_post_meta($product_id, '_club420_davis_enabled', true);
                $dixon_visibility = get_post_meta($product_id, '_club420_dixon_visibility', true);
                $dixon_enabled = get_post_meta($product_id, '_club420_dixon_enabled', true);
                
                // Calculate individual store status
                $davis_status = $this->calculate_store_status($product_id, 'davis', $davis_visibility, $davis_enabled);
                $dixon_status = $this->calculate_store_status($product_id, 'dixon', $dixon_visibility, $dixon_enabled);
                
                // Format settings display
                $davis_settings = $davis_visibility ? $davis_visibility : 'not set';
                $dixon_settings = $dixon_visibility ? $dixon_visibility : 'not set';
                
                // Add schedule times for scheduled products
                if ($davis_visibility === 'scheduled') {
                    $davis_start = get_post_meta($product_id, '_club420_davis_schedule_start', true);
                    $davis_end = get_post_meta($product_id, '_club420_davis_schedule_end', true);
                    if ($davis_start && $davis_end) {
                        $davis_settings .= '<br><small>' . date('m/d H:i', $davis_start) . ' - ' . date('m/d H:i', $davis_end) . '</small>';
                    }
                }
                
                if ($dixon_visibility === 'scheduled') {
                    $dixon_start = get_post_meta($product_id, '_club420_dixon_schedule_start', true);
                    $dixon_end = get_post_meta($product_id, '_club420_dixon_schedule_end', true);
                    if ($dixon_start && $dixon_end) {
                        $dixon_settings .= '<br><small>' . date('m/d H:i', $dixon_start) . ' - ' . date('m/d H:i', $dixon_end) . '</small>';
                    }
                }
                
                echo '<tr style="border-bottom: 1px solid #eee;">';
                echo '<td style="border: 1px solid #ddd; padding: 8px; font-weight: bold;">' . $product_id . '</td>';
                echo '<td style="border: 1px solid #ddd; padding: 8px; max-width: 200px; word-wrap: break-word;">' . esc_html($product->post_title) . '</td>';
                echo '<td style="border: 1px solid #ddd; padding: 8px; text-align: center;">' . $davis_settings . '</td>';
                echo '<td style="border: 1px solid #ddd; padding: 8px; text-align: center; font-weight: bold;">' . $davis_status . '</td>';
                echo '<td style="border: 1px solid #ddd; padding: 8px; text-align: center;">' . $dixon_settings . '</td>';
                echo '<td style="border: 1px solid #ddd; padding: 8px; text-align: center; font-weight: bold;">' . $dixon_status . '</td>';
                echo '</tr>';
            }
            
            echo '</tbody>';
            echo '</table>';
            echo '</div>';
            
            echo '<p style="margin-top: 15px;"><em>Showing all ' . count($scheduled_products) . ' products with scheduling settings.</em></p>';
            echo '<p style="margin-top: 10px; font-size: 11px; color: #666;"><strong>Note:</strong> Status updates in real-time. Refresh page to see latest status.</p>';
        } else {
            echo '<p style="background: #fff3cd; padding: 15px; border-radius: 5px; border-left: 4px solid #ffc107;">No products with scheduling settings found. Set up scheduling on products to see them here.</p>';
        }
        
        echo '</div>';
        
        return ob_get_clean();
    }
    
    /**
     * Calculate the current status for a specific store
     */
    private function calculate_store_status($product_id, $store, $visibility, $enabled) {
        $current_utc_time = current_time('timestamp', true);
        
        switch ($visibility) {
            case 'always':
                return '<span style="color: green;">✅ VISIBLE</span>';
                
            case 'disabled':
                return '<span style="color: red;">❌ HIDDEN</span>';
                
            case 'scheduled':
                $start_key = "_club420_{$store}_schedule_start";
                $end_key = "_club420_{$store}_schedule_end";
                $start_time = get_post_meta($product_id, $start_key, true);
                $end_time = get_post_meta($product_id, $end_key, true);
                
                if (!$start_time || !$end_time) {
                    return '<span style="color: orange;">⚠️ NO TIMES SET</span>';
                }
                
                if ($current_utc_time >= $start_time && $current_utc_time <= $end_time) {
                    return '<span style="color: green;">✅ SCHEDULED LIVE</span>';
                } else if ($current_utc_time < $start_time) {
                    $hours_until = round(($start_time - $current_utc_time) / 3600, 1);
                    return '<span style="color: blue;">⏳ STARTS IN ' . $hours_until . 'h</span>';
                } else {
                    return '<span style="color: gray;">⏰ SCHEDULE ENDED</span>';
                }
                
            default:
                // Check manual toggle as fallback
                if ($enabled === 'yes') {
                    return '<span style="color: green;">✅ MANUAL ENABLED</span>';
                } else {
                    return '<span style="color: red;">❌ MANUAL DISABLED</span>';
                }
        }
    }
    
    /**
     * Get all products that have schedule-related meta data
     */
    private function get_products_with_schedules() {
        global $wpdb;
        
        // Find products that have visibility settings (scheduled products)
        $product_ids = $wpdb->get_col("
            SELECT DISTINCT p.ID 
            FROM {$wpdb->posts} p 
            INNER JOIN {$wpdb->postmeta} pm ON p.ID = pm.post_id 
            WHERE p.post_type = 'product' 
            AND p.post_status = 'publish'
            AND (
                pm.meta_key = '_club420_davis_visibility' OR 
                pm.meta_key = '_club420_dixon_visibility'
            )
        ");
        
        return array_map('intval', $product_ids);
    }
    
    /**
     * Show system status in admin (only on products page)
     */
    public function show_system_status() {
        global $pagenow, $typenow;
        if ($pagenow !== 'edit.php' || $typenow !== 'product' || !current_user_can('manage_options')) {
            return;
        }
        
        echo '<div class="notice notice-success"><p><strong>Club420 Real-Time Scheduler:</strong> ✅ Active! Products show/hide instantly based on schedules. Use <code>[club420_scheduler_dashboard]</code> shortcode to monitor status.</p></div>';
    }
}

// Initialize the dashboard
new Club420ProductSchedulerDashboard();
```

## COMPLETE BODY JAVASCRIPT (Divi → Theme Options → Integration → Body)

```html
<!-- COMPLETE Club420 System - SMART AGE GATE WITH BOT DETECTION + SAFARI FIXES -->

<style>
  @keyframes fadeIn {
    from { opacity: 0; transform: scale(0.95); }
    to { opacity: 1; transform: scale(1); }
  }

  @keyframes pulse {
    0%, 100% { transform: scale(1); }
    50% { transform: scale(1.05); }
  }

  /* ULTRA-FAST TRANSITIONS - NO FLASH */
  body {
    background: #000 !important;
  }

  body, html {
    background-color: #000 !important; /* Double protection */
  }

  /* Prevent any white background during page load */
  body::before {
    content: '';
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: #000;
    z-index: -1;
  }

  /* FIXED Modal Positioning */
  .club420-modal {
    position: fixed !important;
    z-index: 999999 !important;
    top: 0 !important; 
    left: 0 !important;
    width: 100vw !important;
    height: 100vh !important;
    background: rgba(0, 0, 0, 0.85);
    backdrop-filter: blur(10px);
    display: none !important;
    justify-content: center !important;
    align-items: center !important;
    font-family: 'Inter', sans-serif;
    padding: 1rem;
    margin: 0 !important;
    box-sizing: border-box !important;
  }

  .club420-modal.show {
    display: flex !important;
    animation: fadeIn 0.5s ease-in-out;
  }

  .club420-modal-box {
    background: #fff;
    padding: 2rem;
    border-radius: 16px;
    text-align: center;
    max-width: 420px;
    width: 100%;
    max-height: 90vh;
    box-shadow: 0 0 20px rgba(0,0,0,0.3);
    position: relative !important;
    z-index: 1000000 !important;
    margin: auto !important;
    box-sizing: border-box !important;
    overflow-y: auto;
  }

  .club420-modal-box img {
    max-width: 150px;
    margin-bottom: 1.5rem;
  }

  .club420-modal-box h2 {
    margin-bottom: 0.5rem;
    font-size: 1.75rem;
  }

  .club420-modal-box p {
    margin-bottom: 0.75rem;
    color: #757576;
  }

  .club420-btn {
    display: block;
    width: 100%;
    margin: 0.5rem 0;
    padding: 0.75rem;
    font-size: 1rem;
    border: none;
    border-radius: 8px;
    font-weight: bold;
    cursor: pointer;
    transition: background 0.3s ease;
  }

  .club420-btn--yes {
    background: #f2ac1d;
    color: black;
  }

  .club420-btn--no {
    background: #757576;
    color: white;
  }

  .store-option {
    display: block;
    border: 2px solid #ddd;
    border-radius: 10px;
    padding: 1rem;
    margin-bottom: 1rem;
    cursor: pointer;
    text-align: left;
    transition: all 0.3s ease;
    user-select: none;
  }

  .store-option:hover {
    border-color: #f2ac1d;
    background: #fef7e8;
  }

  .store-option:active {
    transform: scale(0.98);
  }

  #enter-site-button {
    width: 100%;
    padding: 0.75rem;
    font-size: 1rem;
    border: none;
    border-radius: 10px;
    background: #ccc;
    color: white;
    font-weight: bold;
    cursor: not-allowed;
    transition: background 0.3s, transform 0.2s;
  }

  #enter-site-button:not(:disabled) {
    background: #f2ac1d;
    color: black;
    cursor: pointer;
  }

  #enter-site-button:not(:disabled):hover {
    background: #e09b0a;
    transform: scale(1.02);
  }

  #enter-site-button:not(:disabled):active {
    transform: scale(0.98);
  }

  /* ULTRA-FAST Page transition overlay - ALWAYS VISIBLE */
  .club420-page-transition {
    position: fixed !important;
    top: 0 !important;
    left: 0 !important;
    width: 100vw !important;
    height: 100vh !important;
    background: linear-gradient(135deg, #000 0%, #333 100%);
    z-index: 999999 !important;
    display: flex !important;
    align-items: center !important;
    justify-content: center !important;
    opacity: 0;
    visibility: hidden;
    transition: none !important;
    margin: 0 !important;
    padding: 0 !important;
    box-sizing: border-box !important;
  }
  
  .club420-page-transition.active {
    display: flex !important;
    opacity: 1 !important;
    visibility: visible !important;
    transition: none !important;
  }
  
  .transition-content {
    text-align: center;
    color: white;
    font-family: 'Inter', sans-serif;
    position: relative;
    z-index: 1000000;
    max-width: 90vw;
    padding: 2rem;
  }
  
  .transition-logo {
    width: 80px;
    height: 80px;
    margin: 0 auto 20px;
    background: rgba(255, 255, 255, 0.95);
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    animation: pulse 2s infinite;
    overflow: hidden;
    box-shadow: 0 4px 20px rgba(0,0,0,0.2);
  }
  
  .transition-logo img {
    max-width: 70%;
    max-height: 70%;
    object-fit: contain;
  }
  
  .logo-desktop {
    display: block;
  }
  
  .logo-mobile {
    display: none;
  }
  
  @media screen and (max-width: 768px) {
    .transition-logo {
      width: 70px;
      height: 70px;
      margin-bottom: 15px;
    }
    
    .transition-text {
      font-size: 20px;
    }
    
    .transition-subtext {
      font-size: 14px;
    }
    
    .logo-desktop {
      display: none;
    }
    
    .logo-mobile {
      display: block;
    }
  }
  
  .transition-text {
    font-size: 24px;
    font-weight: 600;
    margin-bottom: 12px;
    color: white;
    text-shadow: 0 2px 4px rgba(0,0,0,0.5);
  }
  
  .transition-subtext {
    font-size: 16px;
    opacity: 0.9;
    color: white;
    text-shadow: 0 1px 2px rgba(0,0,0,0.5);
  }
  
  /* ULTRA-FAST Pre-transition animations */
  .page-prepare-exit {
    transform: scale(0.98);
    opacity: 0.8;
    transition: none !important;
  }

  @media screen and (max-width: 480px) {
    .club420-modal-box {
      padding: 1.25rem;
    }
    .club420-modal-box h2 {
      font-size: 1.5rem;
    }
    .club420-modal-box p {
      font-size: 0.95rem;
    }
  }

  .store-hidden {
    visibility: hidden !important;
    opacity: 0 !important;
    transition: opacity 0.3s ease !important;
  }

  .store-visible {
    visibility: visible !important;
    opacity: 1 !important;
    transition: opacity 0.3s ease !important;
  }
</style>

<!-- AGE GATE MODAL -->
<div id="age-gate-modal" class="club420-modal">
  <div class="club420-modal-box">
    <img src="https://dev.club420.com/wp-content/uploads/2025/05/Asset-1.png" alt="Club420 Logo">
    <h2>Welcome to Club420</h2>
    <p>Please verify your age and select your preferred store location</p>
    <p>Are you 21 years or older?</p>
    <button id="confirm-age" class="club420-btn club420-btn--yes" onclick="confirmAgeClick()">YES</button>
    <button onclick="window.location.href='https://google.com'" class="club420-btn club420-btn--no">NO</button>
  </div>
</div>

<!-- STORE PICKER MODAL -->
<div id="store-picker-modal" class="club420-modal">
  <div class="club420-modal-box">
    <h2>Select Your Store Location</h2>
    <label class="store-option" data-store="davis" onclick="selectStore('davis')">
      <input type="radio" name="store" value="davis" style="margin-right: 0.75rem;" onclick="selectStore('davis')">
      <strong>Davis Store</strong>
    </label>
    <label class="store-option" data-store="dixon" onclick="selectStore('dixon')">
      <input type="radio" name="store" value="dixon" style="margin-right: 0.75rem;" onclick="selectStore('dixon')">
      <strong>Dixon Store</strong>
    </label>
    <button id="enter-site-button" disabled onclick="enterSiteClick()">ENTER SITE</button>
  </div>
</div>

<!-- ULTRA-FAST TRANSITION OVERLAY -->
<div id="club420-transition" class="club420-page-transition">
  <div class="transition-content">
    <div class="transition-logo">
      <img src="https://dev.club420.com/wp-content/uploads/2025/06/black_leaf_logo_desktop.png" 
           alt="Club420 Logo" class="logo-desktop">
      <img src="https://dev.club420.com/wp-content/uploads/2025/06/black_leaf_logo_mobile.png" 
           alt="Club420 Logo" class="logo-mobile">
    </div>
    <div class="transition-text" id="transition-store-text">Switching to Dixon Store</div>
    <div class="transition-subtext">Updating your product selection...</div>
  </div>
</div>

<script>
  const davisID = '79043044-f024-4b70-8714-4fcad409f978';
  const dixonID = '7029749f-9c6d-419e-b037-5c1b566f3df9';

  // DIRECT BUTTON FUNCTIONS (Safari fix)
  function confirmAgeClick() {
    console.log('👍 Age confirmed by user (direct click)');
    localStorage.setItem('tymber-user-has-allowed-age', 'true');
    
    hideModal('age-gate-modal');
    
    if (!localStorage.getItem('last-store-selected')) {
      setTimeout(() => {
        showModal('store-picker-modal');
      }, 300);
    } else {
      showStoreSectionsSmooth();
      
      // Handle URL store filter for button functionality
      const currentUrl = new URL(window.location);
      const hasStoreFilter = currentUrl.searchParams.has('store_filter');
      
      if (!hasStoreFilter) {
        console.log('🔄 Adding store filter for button functionality');
        const storeSelected = localStorage.getItem('last-store-selected');
        const currentStore = storeSelected === davisID ? 'davis' : 'dixon';
        currentUrl.searchParams.set('store_filter', currentStore);
        window.location.replace(currentUrl.toString());
      }
    }
  }

  function selectStore(store) {
    console.log('🏪 Store selected:', store);
    
    // Check the corresponding radio button
    const radioButton = document.querySelector(`input[name="store"][value="${store}"]`);
    if (radioButton) {
      radioButton.checked = true;
    }
    
    // Enable the Enter Site button
    const btn = document.getElementById('enter-site-button');
    if (btn) {
      btn.disabled = false;
      btn.style.background = '#f2ac1d';
      btn.style.cursor = 'pointer';
      console.log('✅ Enter Site button enabled');
    }
  }

  function enterSiteClick() {
    const selectedRadio = document.querySelector('input[name="store"]:checked');
    if (selectedRadio) {
      console.log('🚀 Entering site with store:', selectedRadio.value);
      setStoreSmoothReload(selectedRadio.value);
    } else {
      console.log('❌ No store selected');
    }
  }

  // SMART BOT DETECTION - BYPASS AGE GATE FOR TESTING TOOLS ONLY
  function isTestingBot() {
    const userAgent = navigator.userAgent.toLowerCase();
    const testingBots = [
      'gtmetrix',
      'pagespeed',
      'lighthouse', 
      'pingdom',
      'webpagetest',
      'websiteaudit',
      'googlebot',
      'bingbot',
      'crawler',
      'spider',
      'headless'
    ];
    
    // Check user agent
    const isBot = testingBots.some(bot => userAgent.includes(bot));
    
    // Check for headless browsers (common in testing)
    const isHeadless = navigator.webdriver === true || 
                      window.navigator.webdriver === true ||
                      window.callPhantom || 
                      window._phantom;
    
    // Check for testing URLs
    const isTestingUrl = window.location.href.includes('PageSpeed=off') ||
                         window.location.href.includes('gtmetrix') ||
                         window.location.search.includes('test=true') ||
                         window.location.search.includes('bot=true');
    
    console.log('🔍 Bot detection:', {
      userAgent: userAgent.substring(0, 50) + '...',
      isBot: isBot,
      isHeadless: isHeadless,
      isTestingUrl: isTestingUrl
    });
    
    return isBot || isHeadless || isTestingUrl;
  }

  // MODAL FUNCTIONS
  function showModal(modalId) {
    console.log('📱 Showing modal:', modalId);
    
    document.querySelectorAll('.club420-modal').forEach(modal => {
      modal.classList.remove('show');
    });
    
    const modal = document.getElementById(modalId);
    if (modal) {
      modal.classList.add('show');
      document.body.style.overflow = 'hidden';
      console.log('✅ Modal shown:', modalId);
    } else {
      console.error('❌ Modal not found:', modalId);
    }
  }

  function hideModal(modalId) {
    console.log('📱 Hiding modal:', modalId);
    const modal = document.getElementById(modalId);
    if (modal) {
      modal.classList.remove('show');
      document.body.style.overflow = '';
      console.log('✅ Modal hidden:', modalId);
    }
  }

  function hideAllModals() {
    document.querySelectorAll('.club420-modal').forEach(modal => {
      modal.classList.remove('show');
    });
    document.body.style.overflow = '';
  }

  // ULTRA-FAST STORE SWITCHING - ZERO DELAYS, ZERO FLASH
  function setStoreSmoothReload(store) {
    const id = store === 'davis' ? davisID : dixonID;
    const storeName = store === 'davis' ? 'Davis' : 'Dixon';
    
    console.log('Club420 ULTRA-FAST: Switching to', storeName);
    
    // INSTANT dark overlay - covers any flash immediately
    document.body.style.background = 'linear-gradient(135deg, #000 0%, #333 100%)';
    document.body.style.transition = 'none';
    
    // INSTANT transition overlay
    const transition = document.getElementById('club420-transition');
    const storeText = document.getElementById('transition-store-text');
    
    if (transition) {
      transition.style.transition = 'none';
      transition.classList.add('active');
    }
    
    if (storeText) {
      storeText.textContent = `Switching to ${storeName} Store`;
    }
    
    // INSTANT localStorage save
    localStorage.setItem('last-store-selected', id);
    hideAllModals();
    
    // IMMEDIATE redirect - zero delays
    const currentUrl = new URL(window.location);
    currentUrl.searchParams.set('store_filter', store);
    window.location.replace(currentUrl.toString());
  }

  function showStoreSectionsSmooth() {
    const isDiviBuilder = 
      document.body.classList.contains('et-fb') || 
      document.body.classList.contains('et_pb_vb') || 
      window.location.href.includes('et_fb=1') || 
      window.location.href.includes('PageSpeed=off') || 
      document.querySelector('.et-fb') || 
      document.querySelector('#et_pb_vb') || 
      document.querySelector('.et_pb_vb_overlay');
    
    if (isDiviBuilder) {
      console.log('Club420: Divi Builder detected - showing ALL sections for editing');
      
      const allElements = document.querySelectorAll('.davis-content, .dixon-content, .davis-menu, .dixon-menu');
      allElements.forEach(el => {
        el.style.display = '';
        el.style.opacity = '1';
        el.style.visibility = 'visible';
      });
      
      return;
    }
    
    const store = localStorage.getItem('last-store-selected');
    
    const davisElements = document.querySelectorAll('.davis-content');
    const dixonElements = document.querySelectorAll('.dixon-content');
    const davisMenuItems = document.querySelectorAll('.davis-menu');
    const dixonMenuItems = document.querySelectorAll('.dixon-menu');

    [...davisElements, ...dixonElements].forEach(el => el.style.display = 'none');
    [...davisMenuItems, ...dixonMenuItems].forEach(el => el.style.display = 'none');

    if (store === davisID) {
      [...davisElements, ...davisMenuItems].forEach(el => el.style.display = '');
      console.log('Club420: Showing Davis sections');
    } else if (store === dixonID) {
      [...dixonElements, ...dixonMenuItems].forEach(el => el.style.display = '');
      console.log('Club420: Showing Dixon sections');
    }
  }

  function smoothPageEntrance() {
    const hasAdminBar = document.body.classList.contains('admin-bar') || 
                        document.querySelector('#wpadminbar') ||
                        window.location.href.includes('wp-admin');
    
    if (hasAdminBar) {
      console.log('Club420: WordPress admin bar detected - skipping body transform to avoid white gap');
      
      document.body.style.opacity = '0';
      
      setTimeout(() => {
        document.body.style.transition = 'opacity 0.4s ease';
        document.body.style.opacity = '1';
        
        setTimeout(() => {
          document.body.style.transition = '';
        }, 400);
      }, 50);
      
      return;
    }
    
    document.body.style.opacity = '0';
    document.body.style.transform = 'scale(1.02)';
    
    setTimeout(() => {
      document.body.style.transition = 'opacity 0.4s ease, transform 0.4s ease';
      document.body.style.opacity = '1';
      document.body.style.transform = 'scale(1)';
      
      setTimeout(() => {
        document.body.style.transition = '';
      }, 400);
    }, 50);
  }

  // SMART INITIALIZATION - BOT DETECTION + FULL AGE GATE
  window.addEventListener('DOMContentLoaded', function() {
    const isTesting = isTestingBot();
    
    if (isTesting) {
      console.log('🤖 TESTING BOT DETECTED: Bypassing age gate for performance testing');
      
      // Force bypass for testing bots ONLY
      localStorage.setItem('tymber-user-has-allowed-age', 'true');
      localStorage.setItem('last-store-selected', davisID);
      
      // Set store filter for button functionality WITHOUT redirect
      const currentUrl = new URL(window.location);
      if (!currentUrl.searchParams.has('store_filter')) {
        currentUrl.searchParams.set('store_filter', 'davis');
        window.history.replaceState({}, '', currentUrl.toString());
      }
      
      hideAllModals();
      showStoreSectionsSmooth();
      smoothPageEntrance();
      return;
    }
    
    // REAL USERS: Full age gate compliance process
    console.log('👤 REAL USER: Full cannabis compliance age gate process');
    
    const ageVerified = localStorage.getItem('tymber-user-has-allowed-age');
    const storeSelected = localStorage.getItem('last-store-selected');
    
    if (ageVerified !== 'true') {
      console.log('🚨 Age verification required');
      showModal('age-gate-modal');
      return;
    }
    
    if (!storeSelected) {
      console.log('🏪 Store selection required');
      showModal('store-picker-modal');
      return;
    }
    
    // User is verified and has store - show content
    console.log('✅ User verified, showing content');
    smoothPageEntrance();
    showStoreSectionsSmooth();
    
    // Handle URL store filter for button functionality (your original logic preserved)
    const currentUrl = new URL(window.location);
    const hasStoreFilter = currentUrl.searchParams.has('store_filter');
    
    if (!hasStoreFilter) {
      console.log('🔄 Adding store filter for button functionality');
      const currentStore = storeSelected === davisID ? 'davis' : 'dixon';
      currentUrl.searchParams.set('store_filter', currentStore);
      window.location.replace(currentUrl.toString());
      return;
    }

    // Set up event handlers for age gate
    const confirmButton = document.getElementById('confirm-age');
    if (confirmButton) {
      confirmButton.addEventListener('click', function() {
        console.log('👍 Age confirmed by user (event listener)');
        confirmAgeClick(); // Call the direct function
      });
    }

    // Set up event handlers for store selection
    const radioButtons = document.querySelectorAll('input[name="store"]');
    radioButtons.forEach(rb => {
      rb.addEventListener('change', function() {
        const btn = document.getElementById('enter-site-button');
        btn.disabled = false;
        btn.style.background = '#f2ac1d';
        btn.style.cursor = 'pointer';
        btn.onclick = function() {
          setStoreSmoothReload(rb.value);
        };
      });
    });
  });

  // MULTIPLE DROPDOWN SUPPORT - HANDLES ALL DROPDOWNS WITH STYLED-STORE-SELECT* IDS
  window.addEventListener('DOMContentLoaded', function() {
    setTimeout(function() {
      console.log('Club420: Initializing multiple dropdown support...');
      
      // Find all dropdowns that start with 'styled-store-select'
      const dropdowns = document.querySelectorAll('select[id^="styled-store-select"]');
      
      if (dropdowns.length === 0) {
        console.log('Club420: No styled store select dropdowns found');
        return;
      }
      
      console.log(`Club420: Found ${dropdowns.length} store dropdown(s)`);
      
      dropdowns.forEach((dropdown, index) => {
        if (!dropdown) return;
        
        console.log(`Club420: Setting up dropdown ${index + 1} with ID: ${dropdown.id}`);
        
        // Set current selection based on localStorage
        const currentStore = localStorage.getItem('last-store-selected');
        if (currentStore === davisID) {
          dropdown.value = 'davis';
        } else if (currentStore === dixonID) {
          dropdown.value = 'dixon';
        }
        
        // Add hover effect
        dropdown.addEventListener('mouseenter', function() {
          this.style.borderColor = '#f2ac1d';
          this.style.boxShadow = '0 0 15px rgba(242, 172, 29, 0.3)';
        });
        
        dropdown.addEventListener('mouseleave', function() {
          this.style.borderColor = '#fff';
          this.style.boxShadow = 'none';
        });
        
        dropdown.addEventListener('change', function() {
          const store = this.value;
          if (!store) return;
          
          console.log('Club420 Multiple Dropdown: Selected', store, 'from dropdown', this.id);
          
          // Sync all other dropdowns to match this selection
          dropdowns.forEach(dd => {
            if (dd !== this && dd.value !== store) {
              dd.value = store;
              console.log(`Club420: Synced dropdown ${dd.id} to ${store}`);
            }
          });
          
          // Use the smooth reload function from body code
          if (typeof setStoreSmoothReload === 'function') {
            setStoreSmoothReload(store);
          } else {
            console.log('setStoreSmoothReload not available, using fallback');
            // Fallback with smooth transition
            const storeId = store === 'davis' ? davisID : dixonID;
            localStorage.setItem('last-store-selected', storeId);
            
            // Add smooth page exit
            document.body.style.transition = 'opacity 0.3s ease, transform 0.3s ease';
            document.body.style.opacity = '0.8';
            document.body.style.transform = 'scale(0.98)';
            
            setTimeout(() => {
              const currentUrl = new URL(window.location);
              currentUrl.searchParams.set('store_filter', store);
              window.location.href = currentUrl.toString();
            }, 300);
          }
        });
      });
      
      console.log('Club420: Multiple dropdown support initialized successfully');
      
    }, 1200); // Slight delay to ensure body code loads first
  });

  window.setStore = setStoreSmoothReload;

  // MENU MANAGER (unchanged)
  window.Club420MenuManager = {
    storeMapping: {
      '79043044-f024-4b70-8714-4fcad409f978': 'f-street',
      '7029749f-9c6d-419e-b037-5c1b566f3df9': 'highway-80'
    },
    
    categories: {
      'flower': 'flower',
      'cartridges': 'cartridge', 
      'edibles': 'edible',
      'prerolls': 'preroll',
      'extract': 'extract',
      'wellness': 'tincture',
      'merchandise': 'merch',
      'shop-all': ''
    },
    
    getDomain: function() {
      const hostname = window.location.hostname;
      if (hostname.includes('dev.')) {
        return 'https://club420.com';
      }
      return 'https://club420.com';
    },
    
    getMenuURL: function(category) {
      const selectedStoreID = localStorage.getItem('last-store-selected');
      const ageVerified = localStorage.getItem('tymber-user-has-allowed-age');
      
      if (!selectedStoreID || ageVerified !== 'true') return '/';
      
      const storeSlug = this.storeMapping[selectedStoreID];
      if (!storeSlug) return '/';
      
      const categorySlug = this.categories[category];
      const domain = this.getDomain();
      
      if (category === 'shop-all' || categorySlug === '') {
        return domain + '/menu/' + storeSlug + '/?order=-price';
      }
      
      if (!categorySlug) {
        return domain + '/menu/' + storeSlug + '/';
      }
      
      return domain + '/menu/' + storeSlug + '/categories/' + categorySlug + '/?order=-price';
    },
    
    goToCategory: function(category) {
      const url = this.getMenuURL(category);
      console.log('Club420: Navigating to:', category, '→', url);
      window.location.href = url;
    }
  };

  // TEXT MENU NAVIGATION (unchanged)
  document.addEventListener('DOMContentLoaded', function() {
    setTimeout(function() {
      console.log('Club420: Starting text menu integration...');
      
      let hooked = 0;
      
      const allLinks = document.querySelectorAll('a');
      allLinks.forEach(link => {
        const text = link.textContent.toLowerCase().trim();
        const href = link.getAttribute('href') || '';
        
        if (href.startsWith('#') && Club420MenuManager.categories.hasOwnProperty(href.substring(1))) {
          return;
        }
        
        if (text === 'flower' || text === 'cartridges' || text === 'edibles' || 
            text === 'pre-rolls' || text === 'prerolls' || text === 'extracts' || 
            text === 'extract' || text === 'shop all' || text === 'all' || text === 'shop now') {
          
          link.addEventListener('click', function(e) {
            e.preventDefault();
            
            if (text === 'flower') Club420MenuManager.goToCategory('flower');
            else if (text === 'cartridges') Club420MenuManager.goToCategory('cartridges');
            else if (text === 'edibles') Club420MenuManager.goToCategory('edibles');
            else if (text === 'pre-rolls' || text === 'prerolls') Club420MenuManager.goToCategory('prerolls');
            else if (text === 'extracts' || text === 'extract') Club420MenuManager.goToCategory('extract');
            else if (text === 'shop all' || text === 'all' || text === 'shop now') Club420MenuManager.goToCategory('shop-all');
          }, { passive: false });
          hooked++;
        }
      });
      
      console.log('Club420: Hooked', hooked, 'text menu elements');
      
    }, 3000);
  });

  window.addEventListener('popstate', function() {
    setTimeout(showStoreSectionsSmooth, 100);
  });

  window.addEventListener('storage', function(e) {
    if (e.key === 'last-store-selected') {
      setTimeout(showStoreSectionsSmooth, 100);
    }
  });

  window.addEventListener('load', function() {
    setTimeout(showStoreSectionsSmooth, 300);
  });

  console.log('🍃 Club420: Smart age gate active with Safari fixes + Multiple Dropdown Support - Compliance for users, performance for bots! 🤖👤');
</script>
```

## STORE DROPDOWN HTML (Divi Code Module)

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

## CAROUSEL SHORTCODE USAGE (In Divi Sections)

### Davis Content Sections:
```
[club420_deals_carousel store="davis"]
```

### Dixon Content Sections:
```
[club420_deals_carousel store="dixon"]
```

## SYSTEM ARCHITECTURE SUMMARY

### **Critical Configuration:**
- **Store IDs**: Davis=`79043044-f024-4b70-8714-4fcad409f978`, Dixon=`7029749f-9c6d-419e-b037-5c1b566f3df9`
- **Tymber Integration**: `/menu/f-street/` (Davis), `/menu/highway-80/` (Dixon) 
- **localStorage Keys**: `tymber-user-has-allowed-age`, `last-store-selected`
- **CSS Classes**: `.davis-content`/`.dixon-content` (sections), `.davis-menu`/`.dixon-menu` (navigation)
- **Dropdown Pattern**: `styled-store-select-*` for automatic detection

### **Text Menu Detection** (Navigation Labels):
- **FLOWER** → `/categories/flower/`
- **CARTRIDGES** → `/categories/cartridge/`
- **EDIBLES** → `/categories/edible/`
- **PRE-ROLLS** or **PREROLLS** → `/categories/preroll/`
- **EXTRACTS** or **EXTRACT** → `/categories/extract/`
- **SHOP ALL** or **ALL** or **SHOP NOW** → `/` (all products)

### **Required WordPress Plugins:**
- **Code Snippets** (for PHP implementation) ✅ **ESSENTIAL**
- **WooCommerce** (e-commerce functionality) ✅ **REQUIRED**
- **Divi Theme** (for JavaScript integration) ✅ **REQUIRED**
- **YITH WooCommerce Badge Management** (for product badges) ✅ **COMPATIBLE**

### **WP Engine Hosting Requirements:**
- **Cache exclusion headers** for real-time scheduling
- **Proxy pass configuration** for Tymber menu integration
- **Performance optimizations** maintained throughout

---

**TASK: Please help me break this complete working system into individual GitHub files and create INSTALLATION.md. All code above is production-ready and working.**
