# Django PainlessSEO

[![PyPI version](https://pypip.in/version/django-painless-seo/badge.svg?style=flat)](https://pypi.python.org/pypi/django-painless-seo)
[![PyPI downloads](https://pypip.in/download/django-painless-seo/badge.svg?style=flat)](https://pypi.python.org/pypi/django-painless-seo)

## Features

This app provides two ways of adding SEO metadata to your django site:

- Absolute paths
- Model instances

It's fully integrated with the admin site including inline forms for models.
It also includes support for multiple languages and localized URLs.

## Requirements

    Django >= 1.5.0

## Installation

The Git repository can be cloned with this command:

    git clone https://github.com/Glamping-Hub/django-painless-seo.git

The `painlessseo` package included in the distribution should be placed on the
`PYTHONPATH`. Add `painlessseo` to the `INSTALLED_APPS` in your *settings.py*.
Run `syncdb` command to create the needed tables.

## Settings

PainlessSEO uses two configuration variables in order to define the default
information that will be displayed if the URL has no SEO metadata related. You
have to add them to your *settings.py*:

    SEO_DEFAULT_TITLES = { 
        'en': 'Lorem ipsum title english',
        'es': 'Lorem ipsum title español',
    }
    SEO_DEFAULT_DESCRIPTIONS = { 
        'en': 'Lorem ipsum description english',
        'es': 'Lorem ipsum description español',
    }

If these variables are dictionaries, they are expected to contain the default metadata 
for each language (identified using its 2 digits code). If an specific language is not 
found, it will take the language defined in 'LANGUAGE_CODE' variable in settings. If 
this variable is not set, 'en' is considered the default language.

If these variabes are strings, they are expected to contain a generic fallback value
that will be outputed for all languages.

Furthermore, registered models can also contain a SEO_DEFAULT_TITLES and a 
SEO_DEFAULT_DESCRIPTIONS variables that will prevail over the generic ones for 
instances of this particular model.

    SEO_DEFAULT_TITLES = { 
        'en': 'Lorem ipsum model title english',
        'es': 'Lorem ipsum model title español',
    }
    SEO_DEFAULT_DESCRIPTIONS = { 
        'en': 'Lorem ipsum model description english',
        'es': 'Lorem ipsum model description español',
    }


### Registering Models

To create synced SEO metadata for model instances you have to define the
`SEO_MODELS` variable in your *settings.py* like this:

    SEO_MODELS = (
        ('myapp', 'mymodel'),
        ('anotherapp', 'anothermodel'),
    )

After registering the models, you can add the inline form to the admin instance
for each model:

    from painlessseo.admin import SeoMetadataInline

    class MyModelAdmin(admin.ModelAdmin):
        inlines = [SeoMetadataInline, ]

Now every time you save a model instance through the admin site, the SEO
metadata will be updated automatically.

In order to initialize the existing instances of a registered models, or when you
need to add a new language for all the existing instances of a registered model, 2 admin
commands are provided:
    
    sync_seo_models :: initializes the seo metadata info and initializes its
        with the default value for each of the languages declared in settings.

    reset_seo_models :: resets the SEO info that has not been manually modified
        into its current default value.


## SEO Output

As simple as loading the `seo` template library and using the `get_seo`
template tag like this:

    {% load seo %}

    <head>
        {% get_seo %}
    </head>

## Notes

[Why PainlessSEO does not include keywords meta tag](http://googlewebmastercentral.blogspot.in/2009/09/google-does-not-use-keywords-meta-tag.html).

## Legal Stuff

This software is licensed under the terms of the BSD 3-clause license. You can
find the whole text of the license in the LICENSE file.
