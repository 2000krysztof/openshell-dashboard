# Local hermetic image build

Requires Git, Python 3, and a running Podman engine. On macOS, start your
Podman machine first with `podman machine start`.

From the repository root:

```sh
bash deploy/konflux/build-local.sh
```

The script snapshots the current tracked files, prefetches the frontend npm
and backend Go dependencies with Hermeto, and injects the generated environment
into a temporary Dockerfile. It builds with `--network none` for the Podman
engine's native architecture, then runs the executable with `--help`.
Image pulls and dependency prefetch require network access.

The default image tag is `localhost/openshell-dashboard:konflux-local`.
Pass a different tag as the first argument. Set `HERMETO_IMAGE` to choose a
specific fetcher image or `PODMAN` to select a Podman executable or wrapper.

Source snapshots and prefetched dependencies remain under
`.cache/konflux-build/` for inspection. Hermeto's manifest rewrites affect only
the snapshot. The temporary clone retains the repository's origin; GitHub SSH
origins are converted to HTTPS so the fetcher does not need local SSH keys.
