# Django GDPR Cookie Consent

## Description

As stated by [GDPR cookie law](https://gdpr.eu/cookies/), websites that serve content for people from European Union must get consent from website visitors before storing any cookies that are not strictly necessary for the website to function. Not complying with GDPR laws can result in a fine of up to €20 million or 4% of the company's annual revenue, whichever is greater.

Django GDPR Cookie Consent app allows you to set up a modal dialog for cookie explanations and preferences. When a specific cookie section is accepted, the widget loads or renders HTML snippets related to that section. For example, if a visitor approved Performance cookies, they would get Google Analytics loaded.

Using the Django GDPR Cookie Consent app, you store the following information about the cookies in Django project settings:

- What are the cookie sections (e.g. "Essential", "Functionality", "Performance", "Marketing")? Are they bounded with any conditional HTML snippets?
- What are the cookie providers within each section (e.g., "This website," "Google Analytics," "Facebook," "Youtube," etc.)?
- What are the cookies set by each of those providers?

Descriptions for sections, providers, or cookies are translatable. User preferences are saved in a cookie too. If a particular section is unselected later, cookies related to that section are attempted to get deleted.

## Examples

Django GDPR Cookie Consent is used at

- [1st things 1st](https://www.1st-things-1st.com)
- [DjangoTricks](https://www.djangotricks.com)

## How to Install

### 1. Download and install the package with pip

[Get Django GDPR Cookie Consent from Gumroad](https://websightful.gumroad.com/l/django-gdpr-cookie-consent).

Install it into your Django project's virtual environment:

```shell
(venv)$ pip install /path/to/django_gdpr_cookie_consent-0.3.1-py2.py3-none-any.whl
```

You might as well add the wheel file in your project's repository and link to it in your `requirements.txt`:

```
Django==3.2
private_wheels/django_gdpr_cookie_consent-0.3.1-py2.py3-none-any.whl
```

### 2. Add the app to INSTALLED_APPS of your project settings

```python
INSTALLED_APPS = [
    # …
    "gdpr_cookie_consent.apps.GdprCookieConsentConfig",
    # …
]
```

### 3. Add path to urlpatterns

```python
from django.urls import path, include

urlpatterns = [
    # …
    path(
        "cookies/",
        include("gdpr_cookie_consent.urls", namespace="cookie_consent"),
    ),
    # …
]
```

### 4. Include the widget to your template(s)

Load the CSS somewhere in the `<head>` section:

```html
<link href="{% static 'gdpr-cookie-consent/css/gdpr-cookie-consent.css' %}" rel="stylesheet" />
```

Include the widget just before the closing `</body>` tag:

```html
{% include "gdpr_cookie_consent/includes/cookie_consent.html" %}
```

Link to the cookie management view, for example, in the website's footer:

```html
{% url "cookie_consent:cookies_management" as cookie_management_url %}
<a href="{{ cookie_management_url }}" rel="nofollow">
    {% trans "Manage Cookies" %}
</a>
```


### 5. Optional: add a context processor to your project settings

If you want to access cookie configuration from any template, add a context processor to your project settings:

```python
TEMPLATES = [
    {
        # …
        "OPTIONS": {
            "context_processors": [
                # …
                "gdpr_cookie_consent.context_processors.gdpr_cookie_consent",
            ],
        },
    },
]

```

## How to Use

### 6. Copy, paste, and modify the cookie consent configuration

Add this to the end of your project settings, then modify it to match the cookie usage of your website:

```python
_ = lambda text: text

COOKIE_CONSENT_SETTINGS = {
    # Base template will be used for the cookie management view.
    # It should contain {% block content %}{% endblock %}
    "base_template_name": "base.html",
    
    # Description will be used in the modal dialog and cookie management view.
    # Provide either a translatable string or a HTML template name.
    "description": _(""),
    "description_template_name": "gdpr_cookie_consent/descriptions/what_are_cookies.html",
    
    # Extra information will be used at the end of cookie management view.
    # Provide either a translatable string or a HTML template name.
    "extra_information": _(""),
    "extra_information_template_name": "gdpr_cookie_consent/descriptions/extra.html",
    
    # All important elements will have CSS classes with `cc-` prefix,
    # by which you can target them and overwrite their styling.
    # But you can attach some CSS classes to certain elements too.
    "styling": {
        "primary_button_css_classes": "",
        "secondary_button_css_classes": "",
        "provider_list_css_classes": "",
        "provider_item_css_classes": "",
        "link_css_classes": "",
        "section_anchor_css_classes": "",
    },
    # Any variables that will be passed to conditional HTML snippets
    "extra_context": {
        "API_KEY_1": "abcdefg12345",
        "API_KEY_2": "12345abcdefg",
    },
    
    # Consent cookie max age say how many seconds to keep the cookie consent preferences.
    # For example, it can be approximately six months
    "consent_cookie_max_age": 60 * 60 * 24 * 30 * 6,
    
    # Sections define the purposes of cookie groups.
    # For example: Essential, Functionality, Performance, and Marketing
    "sections": [
        {
            # The slug will be used as a readable identifier of the section
            "slug": "essential",
            
            # Translatable title of the section
            "title": _("Essential Cookies"),
            
            # Required sections will be already selected and read-only
            "required": True,
            
            # Section summary will be shown in the modal dialog and preferences form.
            # Provide either a translatable string or a HTML template name.
            "summary": _("These cookies are always on, as they’re essential for making this website work, and making it safe. Without these cookies, services you’ve asked for can’t be provided."),
            "summary_template_name": "",
            
            # Section description will be used at the extended cookie explanation in the cookie management view.
            # Provide either a translatable string or a HTML template name.
            "description": _("These cookies are always on, as they’re essential for making this website work, and making it safe. Without these cookies, services you’ve asked for can’t be provided."),
            "description_template_name": "",

            # Cookie providers are websites that set the cookies on your website
            "providers": [
                {
                    # Translatable title of the provider
                    "title": _("This website"),

                    # Provider description will be used at the extended cookie explanation in the cookie management view.
                    # Provide either a translatable string or a HTML template name.
                    "description": "",
                    "description_template_name": "",
                    
                    # A list of cookies set by the provider
                    "cookies": [
                        {
                            # Cookie name can include wildcard syntax like "abc_*"
                            "cookie_name": "sessionid",
                            
                            # Human-readable translated duration of the cookie
                            "duration": _("2 Weeks"),
                            
                            # Cookie description will be used at the extended cookie explanation in the cookie management view.
                            # Provide either a translatable string or a HTML template name.
                            "description": _("Session ID used to authenticate you and give permissions to use the site."),
                            "description_template_name": "",
                            
                            # Cookie domain will be used to delete cookies if a section is unchecked
                            "domain": ".example.com",
                        },
                        {
                            "cookie_name": "csrftoken",
                            "duration": _("Session"),
                            "description": _("Security token used to ensure that no hackers are posting forms on your behalf."),
                            "description_template_name": "",
                            "domain": ".example.com",
                        },
                        {
                            "cookie_name": "cookie_consent",
                            "duration": _("6 Years"),
                            "description": _("Settings of Cookie Consent preferences."),
                            "description_template_name": "",
                            "domain": ".example.com",
                        },
                    ]
                },
            ],
        },
        {
            "slug": "functionality",
            "title": _("Functionality Cookies"),
            
            # Conditional HTML snippet will be loaded or rendered if this section is selected
            "conditional_html_template_name": "conditional_html/functionality.html",
            "required": False,
            "summary": _("These cookies help us provide enhanced functionality and personalisation, and remember your settings. They may be set by us or by third party providers."),
            "summary_template_name": "",
            "description": _("These cookies help us provide enhanced functionality and personalisation, and remember your settings. They may be set by us or by third party providers."),
            "description_template_name": "",
            "providers": [
                {
                    "title": _("Youtube"),
                    "description": _("Youtube is a video platform from which we are embedding video tutorials and other informational videos."),
                    "description_template_name": "",
                    "cookies": [
                        {
                            "cookie_name": "CONSENT",
                            "duration": _("2 Years"),
                            "description": "",
                            "description_template_name": "",
                            "domain": ".youtube.com",
                        },
                        {
                            "cookie_name": "VISITOR_INFO1_LIVE",
                            "duration": _("6 Months"),
                            "description": "",
                            "description_template_name": "",
                        },
                        {
                            "cookie_name": "YSC",
                            "duration": _("Session"),
                            "description": "",
                            "description_template_name": "",
                        },
                        {
                            "cookie_name": "1P_JAR",
                            "duration": _("1 Month"),
                            "description": "",
                            "description_template_name": "",
                            "domain": ".google.com",
                        },
                        {
                            "cookie_name": "NID",
                            "duration": _("6 Months"),
                            "description": "",
                            "description_template_name": "",
                            "domain": ".google.com",
                        },
                    ]
                },
            ],
        },
        {
            "slug": "performance",
            "title": _("Performance Cookies"),
            "conditional_html_template_name": "conditional_html/performance.html",
            "required": False,
            "summary": _("These cookies help us analyse how many people are using this website, where they come from and how they're using it. If you opt out of these cookies, we can’t get feedback to make this website better for you and all our users."),
            "summary_template_name": "",
            "description": _("These cookies help us analyse how many people are using this website, where they come from and how they're using it. If you opt out of these cookies, we can’t get feedback to make this website better for you and all our users."),
            "description_template_name": "",
            "providers": [
                {
                    "title": _("Google Analytics"),
                    "description": _("Google Analytics is used to track website usage statistics."),
                    "description_template_name": "",
                    "cookies": [
                        {
                            "cookie_name": "_ga",
                            "duration": _("2 Years"),
                            "description": _("Used to distinguish users."),
                            "description_template_name": "",
                            "domain": ".example.com",
                        },
                        {
                            "cookie_name": "_gid",
                            "duration": _("24 Hours"),
                            "description": _("Used to distinguish users."),
                            "description_template_name": "",
                            "domain": ".example.com",
                        },
                        {
                            "cookie_name": "_ga_*",
                            "duration": _("2 Years"),
                            "description": _("Used to persist session state."),
                            "description_template_name": "",
                            "domain": ".example.com",
                        },
                        {
                            "cookie_name": "_gac_gb_*",
                            "duration": _("90 Days"),
                            "description": _("Contains campaign related information. If you have linked your Google Analytics and Google Ads accounts, Google Ads website conversion tags will read this cookie unless you opt-out."),
                            "description_template_name": "",
                            "domain": ".example.com",
                        },
                        {
                            "cookie_name": "_gat_gtag_UA_*",
                            "duration": _("1 Minute"),
                            "description": _("Stores unique user ID."),
                            "description_template_name": "",
                            "domain": ".example.com",
                        },
                    ]
                },
            ],
        },
        {
            "slug": "marketing",
            "title": _("Marketing Cookies"),
            "conditional_html_template_name": "conditional_html/marketing.html",
            "required": False,
            "summary": _("These cookies are set by our advertising partners to track your activity and show you relevant ads on other sites as you browse the internet."),
            "summary_template_name": "",
            "description": _("These cookies are set by our advertising partners to track your activity and show you relevant ads on other sites as you browse the internet."),
            "description_template_name": "",
            "providers": [
                {
                    "title": _("Facebook Pixel"),
                    "description": _("Facebook Pixel lets us track how well our ads on Facebook performed, and which ads led to registration or subscriptions."),
                    "description_template_name": "",
                    "cookies": [
                        {
                            "cookie_name": "c_user",
                            "duration": _("3 Months / Session"),
                            "description": _("Contains the user ID of the currently logged in user."),
                            "description_template_name": "",
                            "domain": ".facebook.com",
                        },
                        {
                            "cookie_name": "datr",
                            "duration": _("2 Years"),
                            "description": _("Identifies the web browser being used to connect to Facebook independent of the logged in user."),
                            "description_template_name": "",
                            "domain": ".facebook.com",
                        },
                        {
                            "cookie_name": "sb",
                            "duration": _("Persistent"),
                            "description": _("For storing browser details."),
                            "description_template_name": "",
                            "domain": ".facebook.com",
                        },
                        {
                            "cookie_name": "dpr",
                            "duration": _("7 Days"),
                            "description": _("For delivering an optimal experience for your device’s screen."),
                            "description_template_name": "",
                            "domain": ".facebook.com",
                        },
                        {
                            "cookie_name": "x-referer",
                            "duration": _("Session"),
                            "description": "",
                            "description_template_name": "",
                            "domain": ".facebook.com",
                        },
                        {
                            "cookie_name": "spin",
                            "duration": _("25 Hours"),
                            "description": "",
                            "description_template_name": "",
                            "domain": ".facebook.com",
                        },
                        {
                            "cookie_name": "xs",
                            "duration": _("3 Months / Session"),
                            "description": _("Stores the session ID."),
                            "description_template_name": "",
                            "domain": ".facebook.com",
                        },
                        {
                            "cookie_name": "fr",
                            "duration": _("3 Months / Session"),
                            "description": _("Provides ad delivery or retargeting."),
                            "description_template_name": "",
                            "domain": ".facebook.com",
                        },
                        {
                            "cookie_name": "usida",
                            "duration": _("Session"),
                            "description": "",
                            "description_template_name": "",
                            "domain": ".facebook.com",
                        },
                        {
                            "cookie_name": "presence",
                            "duration": _("Session"),
                            "description": "",
                            "description_template_name": "",
                            "domain": ".facebook.com",
                        },
                        {
                            "cookie_name": "_fbp",
                            "duration": _("3 Months"),
                            "description": _("Used to deliver a series of advertisement products such as real time bidding from third party advertisers."),
                            "description_template_name": "",
                            "domain": ".facebook.com",
                        },
                        {
                            "cookie_name": "m_pixel_ratio",
                            "duration": _("Session"),
                            "description": "",
                            "description_template_name": "",
                            "domain": ".facebook.com",
                        },
                    ]
                },
            ],
        },
    ]
}
```

### 7. Create templates for your conditional HTML snippets

Create templates for the snippets that will be loaded or rendered when a particular section is chosen, for example:

- conditional_html/functionality.html
- conditional_html/performance.html
- conditional_html/marketing.html

For testing, you can add the markup like:

```html
<script>
    console.log('Conditional functionality code loaded');
</script>
```

__Manage the scripts that create your strictly necessary cookies separately, unrelated to the Django GDPR Cookie Consent app.__

### 8. Translate your titles and descriptions

If your website has more than one language, prepare the translations:

- Use management command `makemessages` to collect translatable strings into `django.po` files.
- Translate the strings to the languages you need.
- Then, use the management command `compilemessages` to compile them to `django.mo` files.

### 9. Customize the styling

The modal dialog uses the fonts of the website.

Colors can be modified by CSS custom properties:

```css
body {
  --cc-primary: #007bff;
  --cc-light-gray: #ccc;
  --cc-dark-gray: #343a40;
  --cc-light: #fff;
  --cc-backdrop: rgba(0, 0, 0, 0.5);
}
```

Also, all essential elements have CSS classes with `cc-` prefix, and you can overwrite some of their properties. Alternatively, you can create a custom CSS file and link to it instead.

If you need even more control, you can overwrite the templates by copying them from the app to analogous your project's templates directory and change the markup.

### 10. Optional: add conditional renderings in templates

If you enabled the `gdpr_cookie_consent` context processor, then you can check if a section was chosen by the visitor in any template by this:

```html
{% if "functionality" in cookie_consent_controller.checked_sections %}
    <!-- add some optional functionality -->
{% endif %}
```

If you want to do the analogous check in JavaScript, you could, for example, set a global variable in conditional templates and check for its value elsewhere:

```html
{# conditional_html/functionality.html #}
<script>
window.ENABLE_YOUTUBE_EMBEDS = true;
</script>
```

```javascript
// some javascript file
if (window.ENABLE_YOUTUBE_EMBEDS) {
   embed_youtube_videos();
}
```

## Disclaimer

The actual website's compliance with the GDRP Cookie Law depends on the configuration of each use case. Django GDPR Cookie Consent app provides the mechanism to make that possible, but it's up to you how you configure and integrate it.

## Contact

For technical questions or bug reports, please contact [@archatas](https://github.com/archatas) on Github or [@DjangoTricks](https://twitter.com/DjangoTricks) on Twitter.