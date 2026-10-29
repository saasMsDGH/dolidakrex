# DOLIDAKREX build

The `DOLIDAKREX image` workflow builds the application from `saasMsDGH/doli`,
at the exact commit recorded in `source.lock.json`. The official pinned image
provides the PHP/Apache runtime; all application and CLI source files are replaced
by our fork's sources. No source archive is fetched from upstream; installed application files come from the locked fork.

Images are published to `ghcr.io/saasmsdgh/dolidakrex`, tagged with the Docker
repository commit and labelled with the application source commit. Argo CD must
use the resulting digest, never a moving tag. The deployed environment retains
its database and document volumes when changing the image or public hostname.

The image runs as UID/GID 33. Kubernetes supplies the non-root Apache configuration,
the private Dolibarr config, persistent documents and custom modules, and the native
CLI bootstrap. The upstream root bootstrap is not the entrypoint of this image.
No password or registry credential belongs in this repository.

Changing `source.lock.json` rebuilds the image. Changing the application version
requires a separately planned database migration and a verified backup.
