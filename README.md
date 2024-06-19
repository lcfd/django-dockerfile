# Django Dockerfile

## Setup

- Copy the `Dockerfile`, `Caddyfile` and `.dockerignore` files in your Django project (same place of `pyproject.toml`)
- Set the `static` and `media` config variables in the Django's `settings.py` file.

## On Coolify

Your project will be exposed on the `8001` port.

In `Ports Mappings` set `your_port:8001` and `Ports Exposes` to `8001`.

### Volumes

- Go to `Storages`
- `+ Add` to create new volumes
  - Add one volume for `static` with destination path to `/project/static`
  - Add one volume for `media` with destination path to `/project/media`

### Env variables

[Django doc](https://docs.djangoproject.com/en/5.0/howto/deployment/checklist/#critical-settings).
For production stage go to the `Environment Variables` tab of your service configuration and set those variables:

```bash
# If value is 1, this is the init service.
# For example: start.sh will run migrations
IS_INIT=1

DEBUG=False

APP_NAME=yourname

ALLOWED_HOSTS=*
CSRF_TRUSTED_ORIGINS=https://*.mydomain.com
CORS_ALLOWED_ORIGINS=https://...
DATABASE_URL=postgres://asdfasdf:asdfasdf:5432/postgres
SECRET_KEY=asdfasdfasdfasdfasdfasdf

# Superuser
DJANGO_SUPERUSER_EMAIL=your@superuser.email
DJANGO_SUPERUSER_PASSWORD=password
DJANGO_SUPERUSER_USERNAME=username

LANGUAGE_CODE=en-us
TIME_ZONE="UTC"
MEDIA_URL=media/
STATIC_URL=static/
# MEDIA_ROOT=path
# STATIC_ROOT=path

SESSION_COOKIE_SECURE=(bool, False),
CSRF_COOKIE_SECURE=(bool, False),
SECURE_SSL_REDIRECT=(bool, False),

EMAIL_HOST=localhost
EMAIL_PORT=25
EMAIL_HOST_USER=
EMAIL_HOST_PASSWORD=
EMAIL_USE_TLS=False
SERVER_EMAIL=root@localhost
DEFAULT_FROM_EMAIL=django@localhost
```

I suggest you install [django-environ](https://github.com/joke2k/django-environ).

```python
import environ

env = environ.Env(
    DEBUG=(bool, False),
    APP_NAME=(str, "APPNAME"),
    ALLOWED_HOSTS=(list[str], ["*"]),
    SECRET_KEY=(str, "asdfasdfasdfasdfasdfasdf"),
    CSRF_TRUSTED_ORIGINS=(list[str], []),
    CORS_ALLOWED_ORIGINS=(list[str], []),
    DATABASE_URL=(str, False),
    LANGUAGE_CODE=(str, "en-us"),
    TIME_ZONE=(str, "UTC"),
    MEDIA_ROOT=(str, BASE_DIR / "media"),
    MEDIA_URL=(str, "media/"),
    STATIC_URL=(str, "static/"),
    STATIC_ROOT=(str, BASE_DIR / "static"),
    SESSION_COOKIE_SECURE=(bool, False),
    CSRF_COOKIE_SECURE=(bool, False),
    SECURE_SSL_REDIRECT=(bool, False),
    EMAIL_HOST=(str, "localhost"),
    EMAIL_PORT=(int, 25),
    EMAIL_HOST_USER=(str, ""),
    EMAIL_HOST_PASSWORD=(str, ""),
    EMAIL_USE_TLS=(bool, False),
    SERVER_EMAIL=(str, "root@localhost"),
    DEFAULT_FROM_EMAIL=(str, "django@localhost"),
)

# Usage

DEBUG = env("DEBUG")
APP_NAME = env("APP_NAME")
SECRET_KEY = env("SECRET_KEY")
ALLOWED_HOSTS = env.list("ALLOWED_HOSTS")
CSRF_TRUSTED_ORIGINS = env.list("CSRF_TRUSTED_ORIGINS")

database_url = env("DATABASE_URL")

if database_url:
    DATABASES = {
        "default": env.db(),
    }
else:
    DATABASES = {
        "default": {
            "ENGINE": "django.db.backends.sqlite3",
            "NAME": BASE_DIR / 'db.sqlite3'
        }
    }

LANGUAGE_CODE = env("LANGUAGE_CODE")
TIME_ZONE = env("TIME_ZONE")

MEDIA_ROOT = env("MEDIA_ROOT")
MEDIA_URL = env("MEDIA_URL")
STATIC_URL = env("STATIC_URL")
STATIC_ROOT = env("STATIC_ROOT")

SESSION_COOKIE_SECURE = env("SESSION_COOKIE_SECURE")
CSRF_COOKIE_SECURE = env("CSRF_COOKIE_SECURE")

SECURE_SSL_REDIRECT = env("SECURE_SSL_REDIRECT")

EMAIL_HOST = env("EMAIL_HOST")
EMAIL_PORT = env("EMAIL_PORT")
EMAIL_HOST_USER = env("EMAIL_HOST_USER")
EMAIL_HOST_PASSWORD = env("EMAIL_HOST_PASSWORD")
EMAIL_USE_TLS = env("EMAIL_USE_TLS")
SERVER_EMAIL = env("SERVER_EMAIL")
DEFAULT_FROM_EMAIL = env("DEFAULT_FROM_EMAIL")
```

### Commands

`python manage.py createsuperuser --noinput`

## Domain and Ports

Set your domain in `Domains`.
Set `Ports Exposes` with the port number used by Caddy.
