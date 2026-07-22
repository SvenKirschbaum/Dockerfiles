# Paperless

This image extends the upstream `ghcr.io/paperless-ngx/paperless-ngx` image.

The custom build keeps the Paperless application unchanged, but applies Debian package updates and upgrades selected Python dependencies via `constraints.txt` to reduce reported CVEs. Only dependencies that need to differ from the upstream image should be listed in `constraints.txt`.
