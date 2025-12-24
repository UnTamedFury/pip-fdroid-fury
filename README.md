# Fury Pip Cache

External pip cache repository for fury-fdroid-repo workflows.
This repository automates the caching of pip dependencies to speed up CI/CD pipelines.

## Usage in Workflows

To use this cache in your GitHub Actions workflows, add the following steps:

```yaml
- name: Get latest pip cache release
  id: get_release
  uses: oprypin/find-latest-tag@v1
  with:
    repository: UnTamedFury/pip-fdroid-repo
    releases-only: true

- name: Download pip cache
  run: |
    curl -L -o pip-cache.tar.gz https://github.com/UnTamedFury/pip-fdroid-repo/releases/download/${{ steps.get_release.outputs.tag }}/pip-cache.tar.gz

- name: Install from cache
  run: |
    SITE_PACKAGES=$(python -m site --user-site)
    mkdir -p "$SITE_PACKAGES"
    tar -xzf pip-cache.tar.gz -C "$SITE_PACKAGES"
    echo "PYTHONPATH=$SITE_PACKAGES" >> $GITHUB_ENV
```