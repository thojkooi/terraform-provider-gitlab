---
# generated by https://github.com/hashicorp/terraform-plugin-docs
page_title: "gitlab_user_runner Resource - terraform-provider-gitlab"
subcategory: ""
description: |-
  The gitlab_user_runner resource allows creating a GitLab runner using the new GitLab Runner Registration Flow https://docs.gitlab.com/ci/runners/new_creation_workflow/.
  Upstream API: GitLab REST API docs https://docs.gitlab.com/api/users/#create-a-runner
---

# gitlab_user_runner (Resource)

The `gitlab_user_runner` resource allows creating a GitLab runner using the new [GitLab Runner Registration Flow](https://docs.gitlab.com/ci/runners/new_creation_workflow/).

**Upstream API**: [GitLab REST API docs](https://docs.gitlab.com/api/users/#create-a-runner)

## Example Usage

```terraform
# Create a project runner
resource "gitlab_user_runner" "project_runner" {
  runner_type = "project_type"
  project_id  = 123456

  description = "A runner created using a user access token instead of a registration token"
  tag_list    = ["a-tag", "other-tag"]
  untagged    = true
}

# Create a group runner
resource "gitlab_user_runner" "group_runner" {
  runner_type = "group_type"
  group_id    = 123456

  # populate other attributes...
}

# Create a instance runner
resource "gitlab_user_runner" "instance_runner" {
  runner_type = "instance_type"

  # populate other attributes...
}

# Create a configuration string you can write to a file file that can be used to start a gitlab-runner on a remote machine
# This could be used in startup scripts in major cloud providers to automatically create a runner
# See GitLab Runner Advanced Configuration Options here: https://docs.gitlab.com/runner/configuration/advanced-configuration/
locals {
  config_toml = <<-EOT
concurrent = 1
check_interval = 0

[session_server]
  session_timeout = 1800

[[runners]]
  name = "my_gitlab_runner"
  url = "https://example.gitlab.com"
  token = "${gitlab_user_runner.group_runner.token}"
  executor = "docker"

  [runners.custom_build_dir]
  [runners.cache]
    [runners.cache.s3]
    [runners.cache.gcs]
    [runners.cache.azure]
  [runners.docker]
    tls_verify = false
    image = "ubuntu"
    privileged = true
    disable_entrypoint_overwrite = false
    oom_kill_disable = false
    disable_cache = false
    volumes = ["/cache", "/certs/client"]
    shm_size = 0
  EOT
}
```

<!-- schema generated by tfplugindocs -->
## Schema

### Required

- `runner_type` (String) The scope of the runner. Valid values are: `instance_type`, `group_type`, `project_type`.

### Optional

- `access_level` (String) The access level of the runner. Valid values are: `not_protected`, `ref_protected`.
- `description` (String) Description of the runner.
- `group_id` (Number) The ID of the group that the runner is created in. Required if runner_type is group_type.
- `locked` (Boolean) Specifies if the runner should be locked for the current project.
- `maintenance_note` (String) Free-form maintenance notes for the runner (1024 characters)
- `maximum_timeout` (Number) Maximum timeout that limits the amount of time (in seconds) that runners can run jobs. Must be at least 600 (10 minutes).
- `paused` (Boolean) Specifies if the runner should ignore new jobs.
- `project_id` (Number) The ID of the project that the runner is created in. Required if runner_type is project_type.
- `tag_list` (Set of String) A list of runner tags.
- `untagged` (Boolean) Specifies if the runner should handle untagged jobs.

### Read-Only

- `id` (String) The ID of the gitlab runner.
- `token` (String, Sensitive) The authentication token to use when setting up a new runner with this configuration. This value cannot be imported.

## Import

Starting in Terraform v1.5.0, you can use an [import block](https://developer.hashicorp.com/terraform/language/import) to import `gitlab_user_runner`. For example:

```terraform
import {
  to = gitlab_user_runner.example
  id = "see CLI command below for ID"
}
```

Importing using the CLI is supported with the following syntax:

```shell
# You can import a gitlab runner using its ID
# Note: Importing a runner will not provide access to the `token` attribute
terraform import gitlab_user_runner.example 12345
```
