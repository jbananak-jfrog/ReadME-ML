---
title: Installation Issues
deprecated: false
hidden: false
metadata:
  title: Installation Issues
  robots: index
  legacyUUIDs:
    - UUID-594d4b09-f776-b578-47d5-80ce81cc8f4c
    - UUID-f3354958-00ed-a28b-1c14-97ba92a806e8
---
**I can't install FrogML SDK**

* Python SDK deployment on M1 - make sure you are not running with rosetta.

**I'm getting grpc errors (have 'x86\_64', need 'arm64')**

When using`conda`and running on Mac M1 CPU, simply run the following command:

`conda install -c conda-forge grpcio`

If the issue is

*dependency\_injector/providers.cpython-39-darwin.so' (mach-o file, but is an incompatible architecture (have 'x86\_64', need 'arm64'))*

Do the following:

`pip uninstall dependency\_injector`

`ARCHFLAGS="-arch arm64" pip install dependency\_injector --compile --no-cache-dir`
