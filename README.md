# platform-workflows

The platform team's golden pipeline.

- `actions/lint`, `actions/secret-scan`, `actions/build-image`, `actions/image-scan`: composite actions.
- `.github/workflows/ci.yml`: the reusable workflow that service repos call.

## Why the checks are composite actions and the pipeline is a reusable workflow

<!-- DRAFT: rewrite this in your own words before you use it in an interview. Three sentences, no more. -->

The checks are composite actions because they are steps that must run inside one caller's job and share its workspace, which is how `image-scan` finds the image `build-image` just built without being handed a registry path. The pipeline is a reusable workflow because it is a whole job graph with its own runners and permissions, including a separate `publish` job, which a step-level action cannot express. So a service gets one `uses:` line at job level for the pipeline, and the platform team can change the steps inside it without touching fifty callers.
