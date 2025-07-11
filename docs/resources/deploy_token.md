---
# generated by https://github.com/hashicorp/terraform-plugin-docs
page_title: "gitlab_deploy_token Resource - terraform-provider-gitlab"
subcategory: ""
description: |-
  The gitlab_deploy_token resource allows to manage the lifecycle of group and project deploy tokens.
  Upstream API: GitLab REST API docs https://docs.gitlab.com/api/deploy_tokens/
---

# gitlab_deploy_token (Resource)

The `gitlab_deploy_token` resource allows to manage the lifecycle of group and project deploy tokens.

**Upstream API**: [GitLab REST API docs](https://docs.gitlab.com/api/deploy_tokens/)

## Example Usage

```terraform
# Example Usage - Project
resource "gitlab_deploy_token" "example" {
  project    = "example/deploying"
  name       = "Example deploy token"
  username   = "example-username"
  expires_at = "2020-03-14T00:00:00.000Z"

  scopes = ["read_repository", "read_registry"]
}

resource "gitlab_deploy_token" "example-two" {
  project    = "12345678"
  name       = "Example deploy token expires in 24h"
  expires_at = timeadd(timestamp(), "24h")
}

# Example Usage - Group
resource "gitlab_deploy_token" "example" {
  group = "example/deploying"
  name  = "Example group deploy token"

  scopes = ["read_repository"]
}
```

<!-- schema generated by tfplugindocs -->
## Schema

### Required

- `name` (String) A name to describe the deploy token with.
- `scopes` (Set of String) Valid values: `read_repository`, `read_registry`, `read_package_registry`, `write_registry`, `write_package_registry`.

### Optional

- `expires_at` (String) Time the token will expire it, RFC3339 format. Will not expire per default.
- `group` (String) The name or id of the group to add the deploy token to.
- `project` (String) The name or id of the project to add the deploy token to.
- `username` (String) A username for the deploy token. Default is `gitlab+deploy-token-{n}`.

### Read-Only

- `deploy_token_id` (Number) The id of the deploy token.
- `id` (String) The ID of this resource.
- `token` (String, Sensitive) The secret token. This is only populated when creating a new deploy token. **Note**: The token is not available for imported resources.

## Import

Starting in Terraform v1.5.0, you can use an [import block](https://developer.hashicorp.com/terraform/language/import) to import `gitlab_deploy_token`. For example:

```terraform
import {
  to = gitlab_deploy_token.example
  id = "see CLI command below for ID"
}
```

Importing using the CLI is supported with the following syntax:

```shell
# GitLab deploy tokens can be imported using an id made up of `{type}:{type_id}:{deploy_token_id}`, where type is one of: project, group.
terraform import gitlab_deploy_token.group_token group:1:3
terraform import gitlab_deploy_token.project_token project:1:4

# Note: the `token` resource attribute is not available for imported resources as this information cannot be read from the GitLab API.
```
