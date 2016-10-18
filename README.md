# django-maint-mode-toggle

This package adds a Django management command to toggle maintenance mode on and off, so even if the Django backend is dead, your customers still see a user-friendly web page. It does this simply by renaming a file of your choice. You can then configure your web server to check for the existence of that file before forwarding any requests on to Django.

## Installation

Install this package first, using the normal pip install:

```bash
pip install django-maint-mode-toggle
```

Like any other Django app, add it to `INSTALLED_APPS`:

```python
INSTALLED_APPS = [
	...
	'django_maint_mode_toggle',
	...
]
```

Optionally, you can specify the maintenance file to rename:

```python
maint_mode_file = os.path.join(settings.STATIC_ROOT, 'maint.html')
```

## Use

```bash
python manage.py maintmode [--on|--off]
```

When run without any parameters, it will show the current status of the maintenance mode file. `--on` will remove the `.disabled` suffix, and `--off` will add it.

## Web server configuration

Of course, for any of this to be useful, you must configure your web server to check for the existence of this file before passing any requests on to Django. Here is how this could be done with nginx:

```
location / {
	try_files /maint.html @djangoapp;
}
location @djangoapp {
	uwsgi_pass							127.0.0.1:8000;
	include								/etc/nginx/uwsgi_params;
}
```
