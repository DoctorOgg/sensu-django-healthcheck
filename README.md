[![Sensu Bonsai Asset](https://img.shields.io/badge/Bonsai-Download%20Me-brightgreen.svg?colorB=89C967&logo=sensu)](https://bonsai.sensu.io/assets/DoctorOgg/sensu-django-healthcheck)
![goreleaser](https://github.com/DoctorOgg/sensu-django-healthcheck/workflows/goreleaser/badge.svg)

# sensu-django-healthcheck

## Table of Contents

- [Overview](#overview)
- [Files](#files)
- [Usage examples](#usage-examples)
- [Configuration](#configuration)
  - [Asset registration](#asset-registration)
  - [Check definition](#check-definition)
- [Installation from source](#installation-from-source)

## Overview

This is a simple health check for Django applications. It makes a GET request to the specified URL and expects a JSON response. If the response contains all keys with value "working", the check passes.

[Django Health Check](https://django-health-check.readthedocs.io/en/latest/)

```json
{
  "Cache backend: default": "working",
  "DatabaseBackend": "working",
  "DefaultFileStorageHealthCheck": "working",
  "MigrationsHealthCheck": "working",
  "ProductsHealthCheckBackend": "working"
}
```

## Files

- sensu-django-healthcheck

## Usage examples

```bash
sensu-django-healthcheck -u https://example.com/health/?format=json

all checks are working: map[Cache backend: default:working DatabaseBackend:working DefaultFileStorageHealthCheck:working MigrationsHealthCheck:working ProductsHealthCheckBackend:working]%
```

Help:

```bash
sensu-django-healthcheck -h

Usage:
  sensu-django-healthcheck [flags]
  sensu-django-healthcheck [command]

Available Commands:
  help        Help about any command
  version     Print the version number of this plugin

Flags:
  -d, --debug                  Enable debug mode
  -h, --help                   help for sensu-django-healthcheck
  -i, --insecure-skip-verify   Skip TLS certificate verification (not recommended!)
  -T, --timeout int            Request timeout in seconds (default 15)
  -u, --url string             URL to test (default "http://localhost:80/")
```

## Configuration

### Asset registration

[Sensu Assets][10] are the best way to make use of this plugin. If you're not using an asset, please consider doing so! If you're using sensuctl 5.13 with Sensu Backend 5.13 or later, you can use the following command to add the asset:

```bash
sensuctl asset add DoctorOgg/sensu-django-healthcheck
```

If you're using an earlier version of sensuctl, you can find the asset on the [Bonsai Asset Index][https://bonsai.sensu.io/assets/DoctorOgg/sensu-django-healthcheck].

### Check definition

```yml
---

type: CheckConfig
api_version: core/v2
metadata:
  name: sensu-check-http-json
  namespace: default
spec:
  command: sensu-django-healthcheck -u https://example.com/health/?format=json
  subscriptions:

- system
  runtime_assets:
- DoctorOgg/sensu-check-http-json
```
