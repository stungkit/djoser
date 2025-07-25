.. _authentication-backends:

Authentication Backends
=======================

.. note::

    Both Token Based and JWT Authentication can coexist at same time.
    Simply, follow instructions for both authentication methods and it should work.

Token Based Authentication
--------------------------

Add ``'rest_framework.authtoken'`` to ``INSTALLED_APPS``:

.. code-block:: python

    INSTALLED_APPS = [
        'django.contrib.auth',
        (...),
        'rest_framework',
        'rest_framework.authtoken',
        'djoser',
        (...),
    ]

Configure ``urls.py``. Pay attention to ``djoser.url.authtoken`` module path:

.. code-block:: python

    urlpatterns = [
        (...),
        re_path(r'^auth/', include('djoser.urls')),
        re_path(r'^auth/', include('djoser.urls.authtoken')),
    ]

Add ``rest_framework.authentication.TokenAuthentication`` to Django REST Framework
authentication strategies tuple:

.. code-block:: python

    REST_FRAMEWORK = {
        'DEFAULT_AUTHENTICATION_CLASSES': (
            'rest_framework.authentication.TokenAuthentication',
            (...)
        ),
    }

Run migrations - this step will create tables for ``auth`` and ``authtoken`` apps:

.. code-block:: text

    $ ./manage.py migrate

JSON Web Token Authentication
-----------------------------

Django Settings
~~~~~~~~~~~~~~~

Add ``rest_framework_simplejwt.authentication.JWTAuthentication`` to
Django REST Framework authentication strategies tuple:

.. code-block:: python

    REST_FRAMEWORK = {
        'DEFAULT_AUTHENTICATION_CLASSES': (
            'rest_framework_simplejwt.authentication.JWTAuthentication',
            (...)
        ),
    }

Configure `django-rest-framework-simplejwt` to use the
`Authorization: JWT <access_token>` header:

.. code-block:: python

    SIMPLE_JWT = {
       'AUTH_HEADER_TYPES': ('JWT',),
    }


Disable TokenAuthentication in Django REST Framework:

.. note::

    Skip this if you are using Token Based Authentication on top of JWT Authentication,you should add 'rest_framework.authtoken' to INSTALLED_APPS and run migrations.


.. code-block:: python

    DJOSER = {
        'TOKEN_MODEL': None,
        # other settings
    }



urls.py
~~~~~~~

Configure ``urls.py`` with ``djoser.url.jwt`` module path:

.. code-block:: python

    urlpatterns = [
        (...),
        re_path(r'^auth/', include('djoser.urls')),
        re_path(r'^auth/', include('djoser.urls.jwt')),
    ]
