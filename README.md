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

I suggest you install [django-environ](https://github.com/joke2k/django-environ).

Follow the [doc](https://docs.djangoproject.com/en/5.0/howto/deployment/checklist/#critical-settings).
Go to the `Environment Variables` tab of your service configuration.

### Superuser

`python manage.py createsuperuser --noinput`

with env vars:
- `DJANGO_SUPERUSER_EMAIL`
- `DJANGO_SUPERUSER_PASSWORD`
- `DJANGO_SUPERUSER_USERNAME`