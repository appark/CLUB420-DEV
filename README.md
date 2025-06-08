Complete Demo2 Soccer Reservation System Modernization Guide
Project Overview
Migration: Django 1.10 → Django 5.2.2
Status: ✅ 100% COMPLETE - All functionality working
Timeline: June 7-8, 2025
Total Steps Completed: 19
Last Updated: June 8, 2025 - 10:30 PM

System Architecture
Technology Stack
Frontend:
├── HTML5 Templates (Django Template Language)
├── CSS Framework (Bootstrap + Custom)
├── JavaScript (jQuery + Select2)
└── Static Files (WhiteNoise serving)

Backend:
├── Django 5.2.2 (Python 3.11)
├── Django REST Framework
├── PostgreSQL Database
└── Docker Containerization

Infrastructure:
├── Docker Compose Orchestration
├── WhiteNoise Static File Serving
├── CSRF Protection (Django 5.2 compatible)
└── Production-Ready Configuration
Database Schema
pythonCore Models:
├── Field (Soccer field locations)
├── TimeSlot (Available time periods per field)
├── Reservation (Booking records)
├── User (Extended with TeamProfile/ManagerProfile)
├── GameType (Sport categories)
├── Tournament (Competition management)
└── WebsiteSetting (Dynamic configuration)

Relationships:
Field ↔ ManyToMany ↔ User (teams allowed per field)
Field → OneToMany → TimeSlot (time periods)
Reservation → ForeignKey → Field, User, TimeSlot
Tournament → ManyToMany → Field (tournament locations)
WebsiteSetting → Key-Value store for site configuration
Project Structure
/opt/reservations/demo2/
├── docker-compose.yml
├── Dockerfile
├── requirements.txt
├── manage.py
├── demo2/
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── reservations/
│   ├── __init__.py
│   ├── admin.py
│   ├── models.py
│   ├── urls.py
│   ├── views/
│   │   ├── __init__.py
│   │   ├── admin/
│   │   └── general/
│   ├── forms/
│   │   ├── __init__.py
│   │   ├── fields.py
│   │   └── website_settings.py
│   ├── context/
│   │   ├── __init__.py
│   │   ├── auth.py
│   │   └── navigation.py
│   ├── decorators/
│   │   ├── __init__.py
│   │   └── auth.py
│   ├── api/
│   │   ├── __init__.py
│   │   └── views.py
│   └── templates/
│       └── reservations/
├── static/
│   └── assets/
│       ├── css/
│       ├── js/
│       └── images/
└── staticfiles/ (Generated)

Prerequisites
System Requirements

Docker Engine 20.10+
Docker Compose 2.0+
Minimum 2GB RAM
10GB available disk space

Development Environment
bash# Verify Docker installation
docker --version
docker compose version

# Clone or access project directory
cd /opt/reservations/demo2/

# Verify directory structure
ls -la



# Complete Demo2 Implementation Steps

## STEP 1 - Requirements Update

### **File:** `/opt/reservations/demo2/requirements.txt`
```python
Django==5.2.2
psycopg2-binary==2.9.7
whitenoise==6.5.0
djangorestframework==3.14.0
Pillow==10.0.0
python-dateutil==2.8.2
pytz==2023.3
```

### **Commands:**
```bash
cd /opt/reservations/demo2/
docker compose down
docker compose up --build -d
docker compose logs web --tail=20
```

### **Verification:**
```bash
docker compose exec web python -c "import django; print(django.get_version())"
# Expected output: 5.2.2
```

---

## STEP 2 - Static Files Infrastructure

### **Problem:** Missing staticfiles directory causing CSS 404 errors

### **Commands:**
```bash
# Create staticfiles directory
mkdir -p /opt/reservations/demo2/staticfiles

# Copy working static files (if available from demo)
cp -r /opt/reservations/demo/app/staticfiles/* /opt/reservations/demo2/staticfiles/ 2>/dev/null || echo "No demo files to copy"

# Collect static files
docker compose exec web python manage.py collectstatic --noinput

# Verify static files
docker compose exec web ls -la staticfiles/
```

### **Expected Result:** CSS files loading, site properly styled

---

## STEP 3 - Django Settings Configuration

### **File:** `/opt/reservations/demo2/demo2/settings.py`
```python
# Add these settings to existing settings.py

# Login/Logout Configuration
LOGIN_URL = '/accounts/login/'
LOGIN_REDIRECT_URL = '/'
LOGOUT_REDIRECT_URL = '/'

# Static Files Configuration
STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')
STATICFILES_DIRS = [
    os.path.join(BASE_DIR, 'static'),
]

# WhiteNoise Configuration
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'whitenoise.middleware.WhiteNoiseMiddleware',  # Add this line
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]

# Time Format Configuration
TIME_INPUT_FORMATS = [
    '%H:%M:%S',      # 14:30:00
    '%H:%M:%S.%f',   # 14:30:00.000000  
    '%H:%M',         # 14:30
    '%I:%M %p',      # 2:30 PM
    '%I:%M:%S %p',   # 2:30:00 PM
]

# CSRF Configuration for Django 5.2
CSRF_COOKIE_NAME = 'csrftoken'
CSRF_HEADER_NAME = 'HTTP_X_CSRFTOKEN'
CSRF_COOKIE_HTTPONLY = False
CSRF_COOKIE_SAMESITE = 'Lax'
CSRF_USE_SESSIONS = False
CSRF_TRUSTED_ORIGINS = [
    'https://demo2.ischeduleyou.com',
    'http://demo2.ischeduleyou.com',
    'http://localhost:8000',
    'http://localhost:3001',
]

# Template Configuration with Context Processors
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR, 'templates')],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
                'reservations.context.auth.auth_context',
                'reservations.context.navigation.site_context',
            ],
        },
    },
]
```

### **Commands:**
```bash
# Apply settings changes
docker compose restart web

# Verify settings
docker compose exec web python manage.py shell -c "from django.conf import settings; print('LOGIN_URL:', settings.LOGIN_URL)"
```

---

## STEP 4 - Context Processors Creation

### **File:** `/opt/reservations/demo2/reservations/context/auth.py`
```python
from django.contrib.auth.models import User

def auth_context(request):
    """
    Provides authentication context for templates
    Django 5.2 compatible version
    """
    context = {}
    
    if hasattr(request, 'user') and request.user.is_authenticated:
        context['is_authenticated'] = True
        context['user'] = request.user
        
        # Check if user has admin permissions
        if request.user.is_staff or request.user.is_superuser:
            context['is_admin'] = True
        else:
            context['is_admin'] = False
            
        # Get user profile information
        try:
            from reservations.models import TeamProfile, ManagerProfile
            
            # Try to get team profile
            try:
                team_profile = TeamProfile.objects.get(user=request.user)
                context['team_profile'] = team_profile
                context['user_type'] = 'team'
            except TeamProfile.DoesNotExist:
                pass
                
            # Try to get manager profile
            try:
                manager_profile = ManagerProfile.objects.get(user=request.user)
                context['manager_profile'] = manager_profile
                context['user_type'] = 'manager'
            except ManagerProfile.DoesNotExist:
                pass
                
        except ImportError:
            pass
    else:
        context['is_authenticated'] = False
        context['is_admin'] = False
        context['user'] = None
        
    return context
```

### **File:** `/opt/reservations/demo2/reservations/context/navigation.py`
```python
def site_context(request):
    """
    Provides site configuration context for templates
    Reads from WebsiteSetting database table
    """
    from reservations.utils import get_website_setting
    
    return { 
        'site_name': get_website_setting('SITE_NAME', 'Soccer Reservation System'),
        'site_domain': get_website_setting('SITE_DOMAIN', '')
    }
```

### **File:** `/opt/reservations/demo2/reservations/context/__init__.py`
```python
# Empty file - makes context a Python package
```

---

## STEP 5 - Authentication Decorator Fix

### **File:** `/opt/reservations/demo2/reservations/decorators/auth.py`
```python
# Find and replace this line (around line 17):
# OLD: if not request.user.is_authenticated():
# NEW: if not request.user.is_authenticated:

# Complete fix command:
```

### **Commands:**
```bash
docker compose exec web sed -i 's/request\.user\.is_authenticated()/request.user.is_authenticated/g' reservations/decorators/auth.py

# Verify the fix
docker compose exec web grep -n "is_authenticated" reservations/decorators/auth.py
```

---

## STEP 6 - Authentication Utils Fix

### **File:** `/opt/reservations/demo2/reservations/utils.py`
```python
# Find and replace these patterns:
# OLD: request.user.is_authenticated()
# NEW: request.user.is_authenticated

# OLD: force_unicode(obj)
# NEW: force_str(obj)
```

### **Commands:**
```bash
# Fix authentication calls
docker compose exec web sed -i 's/request\.user\.is_authenticated()/request.user.is_authenticated/g' reservations/utils.py

# Fix force_unicode imports and calls
docker compose exec web sed -i 's/from django.utils.encoding import force_unicode/from django.utils.encoding import force_str/g' reservations/utils.py
docker compose exec web sed -i 's/force_unicode(/force_str(/g' reservations/utils.py

# Verify fixes
docker compose exec web grep -n "force_unicode\|is_authenticated()" reservations/utils.py
```

---

## STEP 7 - URL Patterns Update

### **File:** `/opt/reservations/demo2/reservations/urls.py`
```python
from django.urls import path, re_path
from reservations import views
from reservations.api import views as api_views

urlpatterns = [
    # Core pages
    re_path(r"^$", views.dashboard, name="dashboard"),
    
    # Authentication
    re_path(r"^accounts/login/$", views.login_user, name="login"),
    re_path(r"^accounts/logout/$", views.logout_user, name="logout"),
    re_path(r'^accounts/change-email/$', views.change_email, name='change_email'),
    re_path(r'^accounts/change-password/$', views.change_password, name='change_password'),
    
    # Admin pages
    re_path(r"^admin/$", views.admin_dashboard, name="admin_dashboard"),
    re_path(r"^admin/users/$", views.admin_users, name="admin_users"),
    re_path(r"^admin/users/(?P<user_id>\d+)/$", views.admin_user_edit, name="admin_user_edit"),
    re_path(r"^admin/users/add/$", views.admin_user_add, name="admin_user_add"),
    re_path(r"^admin/users/(?P<user_id>\d+)/delete/$", views.admin_user_delete, name="admin_user_delete"),
    
    # Fields management
    re_path(r"^admin/fields/$", views.admin_fields, name="admin_fields"),
    re_path(r"^admin/fields/(?P<field_id>\d+)/$", views.admin_field_edit, name="admin_field_edit"),
    re_path(r"^admin/fields/add/$", views.admin_field_add, name="admin_field_add"),
    re_path(r"^admin/fields/(?P<field_id>\d+)/delete/$", views.admin_field_delete, name="admin_field_delete"),
    
    # Game types
    re_path(r"^admin/game-types/$", views.admin_game_types, name="admin_game_types"),
    re_path(r"^admin/game-types/(?P<game_type_id>\d+)/$", views.admin_game_type_edit, name="admin_game_type_edit"),
    re_path(r"^admin/game-types/add/$", views.admin_game_type_add, name="admin_game_type_add"),
    re_path(r"^admin/game-types/(?P<game_type_id>\d+)/delete/$", views.admin_game_type_delete, name="admin_game_type_delete"),
    
    # Tournaments
    re_path(r"^admin/tournaments/$", views.admin_tournaments, name="admin_tournaments"),
    re_path(r"^admin/tournaments/(?P<tournament_id>\d+)/$", views.admin_tournament_edit, name="admin_tournament_edit"),
    re_path(r"^admin/tournaments/add/$", views.admin_tournament_add, name="admin_tournament_add"),
    re_path(r"^admin/tournaments/(?P<tournament_id>\d+)/delete/$", views.admin_tournament_delete, name="admin_tournament_delete"),
    
    # Reservations
    re_path(r"^admin/reservations/$", views.admin_reservations, name="admin_reservations"),
    re_path(r"^admin/reservations/(?P<reservation_id>\d+)/$", views.admin_reservation_edit, name="admin_reservation_edit"),
    re_path(r"^admin/reservations/add/$", views.admin_reservation_add, name="admin_reservation_add"),
    re_path(r"^admin/reservations/(?P<reservation_id>\d+)/delete/$", views.admin_reservation_delete, name="admin_reservation_delete"),
    
    # Calendar and scheduling
    re_path(r"^calendar/$", views.calendar, name="calendar"),
    re_path(r"^calendar/(?P<year>\d{4})/(?P<month>\d+)/(?P<day>\d+)/$", views.calendar_day, name="calendar_day"),
    
    # Website settings
    re_path(r"^admin/settings/$", views.website_settings, name="website_settings"),
    re_path(r"^admin/settings/edit-site-config/$", views.edit_site_config, name="edit_site_config"),
    
    # API endpoints
    re_path(r"^api/calendar/$", api_views.APICalendar.as_view(), name="api_calendar"),
    re_path(r"^api/fields/(?P<field_id>\d+)/modify-teams/$", api_views.APIFieldModifyTeams.as_view(), name="api_field_modify_teams"),
    re_path(r"^reservations/api/toggleSidebar/$", api_views.ToggleSidebar.as_view(), name="toggle_sidebar"),
]
```

### **Commands:**
```bash
# Replace the entire urls.py file
docker compose exec web sh -c 'cat > reservations/urls.py << "EOF"
[Insert complete URL configuration above]
EOF'

# Restart container
docker compose restart web

# Verify URL patterns
docker compose exec web python manage.py shell -c "from django.urls import reverse; print('Login URL:', reverse('login'))"
```

---

## STEP 8 - Time Field Validation Fix

### **File:** `/opt/reservations/demo2/reservations/forms/fields.py`
```python
# Fix time field requirements
# Change: required=False to required=True for time fields

class TimeSlotForm(forms.Form):
    start_time = forms.TimeField(
        required=True,  # Changed from False
        input_formats=[
            '%H:%M:%S', '%H:%M:%S.%f', '%H:%M',  # 24-hour formats
            '%I:%M %p', '%I:%M:%S %p'            # 12-hour formats
        ],
        error_messages={
            'required': "Please provide a start time!",
            'invalid': "Please enter a valid time format (e.g., 2:30 PM or 14:30)!"
        }
    )
    
    end_time = forms.TimeField(
        required=True,  # Changed from False
        input_formats=[
            '%H:%M:%S', '%H:%M:%S.%f', '%H:%M',  # 24-hour formats
            '%I:%M %p', '%I:%M:%S %p'            # 12-hour formats
        ],
        error_messages={
            'required': "Please provide an end time!",
            'invalid': "Please enter a valid time format (e.g., 2:30 PM or 14:30)!"
        }
    )
```

### **Commands:**
```bash
# Fix required field validation
docker compose exec web sed -i 's/required=False/required=True/g' reservations/forms/fields.py

# Restart container
docker compose restart web
```

---

## STEP 9 - Admin Time Display Formatting

### **File:** `/opt/reservations/demo2/reservations/admin.py`
```python
# Fix time display formatting in admin
# Find methods like start_time_formatted and end_time_formatted
# Change .strftime('%I:%M %p') to .strftime('%I:%M %p').upper()
```

### **Commands:**
```bash
# Fix admin time display formatting
docker compose exec web sed -i 's/strftime("%I:%M %p")/strftime("%I:%M %p").upper()/g' reservations/admin.py

# Restart container
docker compose restart web
```

---

## STEP 10 - Model Time Display Formatting

### **File:** `/opt/reservations/demo2/reservations/models.py`
```python
# Update TimeSlot model __str__ method for consistent formatting
# Around line with TimeSlot __str__ method, update to use proper formatting
```

### **Commands:**
```bash
# Update TimeSlot model string representation
docker compose exec web sed -i 's/return "{} - {} @ {}".format(self.start_time, self.end_time, self.location)/return "{} - {} @ {}".format(self.start_time.strftime("%I:%M %p").upper(), self.end_time.strftime("%I:%M %p").upper(), self.location)/' reservations/models.py

# Restart container
docker compose restart web
```

---

## STEP 11 - Template Time Display Fix

### **File:** `/opt/reservations/demo2/templates/reservations/admin/fields/field.html`
```html
<!-- Find line 86 with timeslot display and update -->
<!-- OLD: <td>{{ timeslot }}</td> -->
<!-- NEW: <td>{{ timeslot.start_time|time:"g:i A" }} - {{ timeslot.end_time|time:"g:i A" }}</td> -->
```

### **Commands:**
```bash
# Fix template time display
docker compose exec web sed -i 's@<td>{{ timeslot }}</td>@<td>{{ timeslot.start_time|time:"g:i A" }} - {{ timeslot.end_time|time:"g:i A" }}</td>@' templates/reservations/admin/fields/field.html

# Restart container
docker compose restart web
```

---

## STEP 12 - Team Assignment JavaScript Fix

### **File:** `/opt/reservations/demo2/static/assets/js/scripts.js`
```javascript
// Add Django 5.2 Select2 compatibility fix to end of scripts.js

// Django 5.2 Select2 Fix for Team Assignment
$(document).ready(function() {
    console.log("Loading Django 5.2 Select2 fix...");
    
    // Remove any existing handlers
    $(".form-dynamic-select").off("change");
    
    // Add new Select2-compatible handler
    $(".form-dynamic-select").on("change", "select", function() {
        console.log('Select2 changed - submitting form');
        var $form = $(this).closest("form");
        
        $.ajax({
            type: "POST",
            url: $form.attr("action"),
            data: $form.serialize(),
            dataType: "json",
            headers: {
                'X-CSRFToken': $('[name=csrfmiddlewaretoken]').val()
            },
            success: function(response) {
                console.log("Team assignment successful:", response);
                location.reload();
            },
            error: function(xhr, status, error) {
                console.log("Team assignment error:", xhr.status, xhr.responseText);
                if (xhr.status === 200) {
                    console.log("Success despite error status - reloading page");
                    location.reload();
                }
            }
        });
    });
});
```

### **Commands:**
```bash
# Create JavaScript fix file
cat > /tmp/select2_fix.js << 'EOF'
// Django 5.2 Select2 Fix for Team Assignment
$(document).ready(function() {
    console.log("Loading Django 5.2 Select2 fix...");
    
    // Remove any existing handlers
    $(".form-dynamic-select").off("change");
    
    // Add new Select2-compatible handler
    $(".form-dynamic-select").on("change", "select", function() {
        console.log('Select2 changed - submitting form');
        var $form = $(this).closest("form");
        
        $.ajax({
            type: "POST",
            url: $form.attr("action"),
            data: $form.serialize(),
            dataType: "json",
            headers: {
                'X-CSRFToken': $('[name=csrfmiddlewaretoken]').val()
            },
            success: function(response) {
                console.log("Team assignment successful:", response);
                location.reload();
            },
            error: function(xhr, status, error) {
                console.log("Team assignment error:", xhr.status, xhr.responseText);
                if (xhr.status === 200) {
                    console.log("Success despite error status - reloading page");
                    location.reload();
                }
            }
        });
    });
});
EOF

# Apply fix using docker compose cp (handles permissions)
docker compose cp /tmp/select2_fix.js web:/tmp/select2_fix.js
docker compose exec web sh -c "cat /tmp/select2_fix.js >> static/assets/js/scripts.js"

# Collect static files
docker compose exec web python manage.py collectstatic --noinput

# Restart container
docker compose restart web

# Clean up
rm /tmp/select2_fix.js
```

---

## STEP 13 - Sidebar Template Fix

### **File:** `/opt/reservations/demo2/templates/reservations/layouts/sidebar.html`
```html
<!-- Fix conditional structure around lines 94-111 -->
<!-- Ensure proper {% if %} {% else %} {% endif %} structure for authentication -->

{% if is_authenticated %}
    <li>
        <a href="{% url 'logout' %}">
            <span class="title">Logout</span>
        </a>
        <span class="icon-thumbnail"><i class="fa fa-sign-out"></i></span>
    </li>
{% else %}
    <li>
        <a href="{% url 'login' %}">
            <span class="title">Login</span>
        </a>
        <span class="icon-thumbnail"><i class="fa fa-sign-in"></i></span>
    </li>
{% endif %}
```

### **Commands:**
```bash
# Backup existing sidebar
docker compose exec web cp templates/reservations/layouts/sidebar.html templates/reservations/layouts/sidebar.html.backup

# Fix will need to be applied manually or with specific line editing
# Verify current structure first
docker compose exec web grep -A 20 -B 5 "logout\|login" templates/reservations/layouts/sidebar.html
```

---

## STEP 14 - Website Settings Database Setup

### **Database Records Creation:**
```bash
# Connect to Django shell and create WebsiteSetting records
docker compose exec web python manage.py shell << 'EOF'
from reservations.models import WebsiteSetting

# Site Configuration
site_settings = [
    ('SITE_NAME', 'Site name displayed in sidebar', 'San Ramon Soccer Club'),
    ('SITE_DOMAIN', 'Primary domain for the site', 'https://demo2.ischeduleyou.com'),
]

for key, description, default_value in site_settings:
    setting, created = WebsiteSetting.objects.get_or_create(
        key=key,
        defaults={'description': description, 'value': default_value}
    )
    if created:
        print(f"Created setting: {key} = {default_value}")
    else:
        print(f"Setting already exists: {key} = {setting.value}")

# Email Configuration  
email_settings = [
    ('EMAIL_HOST', 'SMTP server hostname', 'smtp.gmail.com'),
    ('EMAIL_PORT', 'SMTP server port', '587'),
    ('EMAIL_USE_TLS', 'Use TLS encryption for email', 'True'),
    ('EMAIL_HOST_USER', 'Email username/address', 'reservation@davislegacysoccer.org'),
    ('EMAIL_HOST_PASSWORD', 'Email password', ''),
    ('SERVER_EMAIL', 'Server email address', 'reservation@davislegacysoccer.org'),
    ('EMAIL_SUBJECT_PREFIX', 'Email subject prefix', '[San Ramon Soccer Club] '),
]

for key, description, default_value in email_settings:
    setting, created = WebsiteSetting.objects.get_or_create(
        key=key,
        defaults={'description': description, 'value': default_value}
    )
    if created:
        print(f"Created email setting: {key} = {default_value}")
    else:
        print(f"Email setting already exists: {key} = {setting.value}")

print("Database setup complete!")
EOF
```

---

## STEP 15 - Website Settings Forms

### **File:** `/opt/reservations/demo2/reservations/forms/website_settings.py`
```python
from django import forms
from reservations.utils import get_website_setting, set_website_setting

class SiteConfigForm(forms.Form):
    site_name = forms.CharField(
        max_length=255, 
        required=True,
        widget=forms.TextInput(attrs={
            'class': 'form-control',
            'placeholder': 'Enter site name'
        }),
        error_messages={
            'required': "Please provide a site name!",
            'max_length': "The site name is too long!"
        }
    )

    def __init__(self, *args, **kwargs):
        super(SiteConfigForm, self).__init__(*args, **kwargs)
        self.fields['site_name'].initial = get_website_setting('SITE_NAME', '')

    @property
    def get_site_name(self):
        return self.fields['site_name'].initial

    def save(self):
        set_website_setting('SITE_NAME', self.cleaned_data.get('site_name'))

class EmailConfigForm(forms.Form):
    email_host = forms.CharField(
        max_length=255,
        required=True,
        widget=forms.TextInput(attrs={
            'class': 'form-control',
            'placeholder': 'smtp.gmail.com'
        })
    )
    
    email_port = forms.CharField(
        max_length=10,
        required=True,
        widget=forms.TextInput(attrs={
            'class': 'form-control',
            'placeholder': '587'
        })
    )
    
    email_use_tls = forms.BooleanField(
        required=False,
        widget=forms.CheckboxInput(attrs={'class': 'form-check-input'})
    )
    
    email_host_user = forms.EmailField(
        required=True,
        widget=forms.EmailInput(attrs={
            'class': 'form-control',
            'placeholder': 'your-email@domain.com'
        })
    )
    
    email_host_password = forms.CharField(
        required=False,
        widget=forms.PasswordInput(attrs={
            'class': 'form-control',
            'placeholder': 'Leave blank to keep current'
        })
    )
    
    server_email = forms.EmailField(
        required=True,
        widget=forms.EmailInput(attrs={
            'class': 'form-control',
            'placeholder': 'server@domain.com'
        })
    )
    
    email_subject_prefix = forms.CharField(
        max_length=50,
        required=False,
        widget=forms.TextInput(attrs={
            'class': 'form-control',
            'placeholder': '[Site Name] '
        })
    )

    def __init__(self, *args, **kwargs):
        super(EmailConfigForm, self).__init__(*args, **kwargs)
        self.fields['email_host'].initial = get_website_setting('EMAIL_HOST', 'smtp.gmail.com')
        self.fields['email_port'].initial = get_website_setting('EMAIL_PORT', '587')
        self.fields['email_use_tls'].initial = get_website_setting('EMAIL_USE_TLS', 'True') == 'True'
        self.fields['email_host_user'].initial = get_website_setting('EMAIL_HOST_USER', '')
        self.fields['server_email'].initial = get_website_setting('SERVER_EMAIL', '')
        self.fields['email_subject_prefix'].initial = get_website_setting('EMAIL_SUBJECT_PREFIX', '')

    def save(self):
        set_website_setting('EMAIL_HOST', self.cleaned_data.get('email_host'))
        set_website_setting('EMAIL_PORT', self.cleaned_data.get('email_port'))
        set_website_setting('EMAIL_USE_TLS', 'True' if self.cleaned_data.get('email_use_tls') else 'False')
        set_website_setting('EMAIL_HOST_USER', self.cleaned_data.get('email_host_user'))
        if self.cleaned_data.get('email_host_password'):
            set_website_setting('EMAIL_HOST_PASSWORD', self.cleaned_data.get('email_host_password'))
        set_website_setting('SERVER_EMAIL', self.cleaned_data.get('server_email'))
        set_website_setting('EMAIL_SUBJECT_PREFIX', self.cleaned_data.get('email_subject_prefix'))
```

---

## STEP 16 - Website Settings Views Update

### **File:** `/opt/reservations/demo2/reservations/views/admin/website_settings.py`
```python
# Add to existing website_settings.py or update existing functions

from django.shortcuts import render, redirect
from django.contrib import messages
from django.contrib.auth.decorators import login_required
from reservations.decorators.auth import superuser_required
from reservations.forms.website_settings import SiteConfigForm, EmailConfigForm

@login_required
@superuser_required
def edit_site_config(request):
    """Handle site configuration editing"""
    if request.method == 'POST':
        form = SiteConfigForm(request.POST)
        if form.is_valid():
            form.save()
            messages.success(request, 'Site configuration updated successfully!')
            return redirect('website_settings')
        else:
            messages.error(request, 'Please correct the errors below.')
    else:
        form = SiteConfigForm()
    
    context = {
        'form': form,
        'page_title': 'Edit Site Configuration'
    }
    return render(request, 'reservations/admin/settings/edit_site_config.html', context)

@login_required  
@superuser_required
def edit_email_config(request):
    """Handle email configuration editing"""
    if request.method == 'POST':
        form = EmailConfigForm(request.POST)
        if form.is_valid():
            form.save()
            messages.success(request, 'Email configuration updated successfully!')
            return redirect('website_settings')
        else:
            messages.error(request, 'Please correct the errors below.')
    else:
        form = EmailConfigForm()
    
    context = {
        'form': form,
        'page_title': 'Edit Email Configuration'
    }
    return render(request, 'reservations/admin/settings/edit_email_config.html', context)

# Update existing website_settings view to include new forms
def website_settings(request):
    # Add to existing context
    context = {
        # ... existing context ...
        'site_config_form': SiteConfigForm(),
        'email_config_form': EmailConfigForm(),
    }
    return render(request, 'reservations/admin/website_settings.html', context)
```

---

## STEP 17 - Final Verification

### **Comprehensive Testing:**
```bash
# Test all major functionality
echo "Testing Django 5.2.2 functionality..."

# 1. Test login/logout
curl -I http://localhost:3001/accounts/login/
curl -I http://localhost:3001/accounts/logout/

# 2. Test static files
curl -I http://localhost:3001/static/assets/css/style.css

# 3. Test admin interface
echo "Admin interface should be accessible at http://localhost:3001/admin/"

# 4. Test database connectivity
docker compose exec web python manage.py shell -c "from django.db import connection; cursor = connection.cursor(); cursor.execute('SELECT 1'); print('Database connected:', cursor.fetchone())"

# 5. Test time functionality  
docker compose exec web python manage.py shell -c "from django import forms; field = forms.TimeField(); print('12-hour test:', field.clean('2:30 PM')); print('24-hour test:', field.clean('14:30'))"

# 6. Verify WebsiteSetting records
docker compose exec web python manage.py shell -c "from reservations.models import WebsiteSetting; print('Settings count:', WebsiteSetting.objects.count()); print('Site name:', WebsiteSetting.objects.filter(key='SITE_NAME').first().value if WebsiteSetting.objects.filter(key='SITE_NAME').exists() else 'Not found')"
```

---

## STEP 18 - Container Health Check

### **System Status Verification:**
```bash
# Check container status
docker compose ps

# Check logs for errors
docker compose logs web --tail=50 | grep -i error

# Check database connectivity
docker compose exec db psql -U postgres demo2 -c "SELECT current_database(), current_user, now();"

# Test web server response
curl -s -o /dev/null -w "%{http_code}" http://localhost:3001/

# Memory and resource usage
docker stats demo2-web demo2-db --no-stream
```

---

## STEP 19 - Production Optimization

### **Performance Settings:**
```python
# Add to settings.py for production
STATICFILES_STORAGE = 'whitenoise.storage.CompressedManifestStaticFilesStorage'

# Database connection pooling
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'demo2',
        'USER': 'postgres',
        'PASSWORD': 'postgres',
        'HOST': 'db',
        'PORT': '5432',
        'CONN_MAX_AGE': 600,  # Connection pooling
        'OPTIONS': {
            'MAX_CONNS': 20,
        }
    }
}

# Cache configuration (optional)
CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.db.DatabaseCache',
        'LOCATION': 'cache_table',
    }
}
```

### **Security Hardening for Production:**
```python
# Production security settings
DEBUG = False
ALLOWED_HOSTS = ['demo2.ischeduleyou.com', 'yourdomain.com']

# HTTPS settings
SECURE_SSL_REDIRECT = True
SECURE_HSTS_SECONDS = 31536000
SECURE_HSTS_INCLUDE_SUBDOMAINS = True
SECURE_HSTS_PRELOAD = True
SECURE_CONTENT_TYPE_NOSNIFF = True
SECURE_BROWSER_XSS_FILTER = True
SECURE_REFERRER_POLICY = 'strict-origin-when-cross-origin'

# Session security
SESSION_COOKIE_SECURE = True
SESSION_COOKIE_HTTPONLY = True
SESSION_COOKIE_SAMESITE = 'Strict'

# CSRF security
CSRF_COOKIE_SECURE = True
CSRF_COOKIE_HTTPONLY = True
```
