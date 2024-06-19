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

Follow the [doc](https://docs.djangoproject.com/en/5.0/howto/deployment/checklist/#critical-settings).
Go to the `Environment Variables` tab of your service configuration.

```bash
DEBUG=on # Don't use it if it's production

ALLOWED_HOSTS=*
CSRF_TRUSTED_ORIGINS=https://*.mydomain.com
DATABASE_URL=postgres://asdfasdf:asdfasdf:5432/postgres
IS_INIT=1
SECRET_KEY=asdfasdfasdfasdfasdfasdf

# Superuser
DJANGO_SUPERUSER_EMAIL=your@superuser.email
DJANGO_SUPERUSER_PASSWORD=password
DJANGO_SUPERUSER_USERNAME=username
```

I suggest you install [django-environ](https://github.com/joke2k/django-environ).

```python
import environ

env = environ.Env(
    DEBUG=(bool, False),
    ALLOWED_HOSTS=(list[str], ["*"]),
    CSRF_TRUSTED_ORIGINS=(list[str], []),
)

DEBUG = env("DEBUG")
SECRET_KEY = env("SECRET_KEY")
ALLOWED_HOSTS = env.list("ALLOWED_HOSTS")
CSRF_TRUSTED_ORIGINS = env.list("CSRF_TRUSTED_ORIGINS")
DATABASES = {
    "default": env.db(),
    "extra": {
        "ENGINE": "django.db.backends.sqlite3",
        "NAME": BASE_DIR / "db.sqlite3",
    },
}
```

### Commands

`python manage.py createsuperuser --noinput`

## Domain and Ports

Set your domain in `Domains`.
Set `Ports Exposes` with the port number used by Caddy.
