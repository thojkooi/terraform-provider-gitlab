---
# generated by https://github.com/hashicorp/terraform-plugin-docs
page_title: "gitlab_branch_protection Resource - terraform-provider-gitlab"
subcategory: ""
description: |-
  The gitlab_branch_protection resource allows to manage the lifecycle of a protected branch of a repository.
  ~> Branch Protection Behavior for the default branch
  Depending on the GitLab instance, group or project setting the default branch of a project is created automatically by GitLab behind the scenes.
  Due to some https://gitlab.com/gitlab-org/terraform-provider-gitlab/issues/792 limitations https://discuss.hashicorp.com/t/ignore-the-order-of-a-complex-typed-list/42242 in the Terraform Provider SDK and the GitLab API,
  when creating a new project and trying to manage the branch protection setting for its default branch the gitlab_branch_protection resource will
  automatically take ownership of the default branch without an explicit import by unprotecting and properly protecting it again.
  Having multiple gitlab_branch_protection resources for the same project and default branch will result in them overriding each other - make sure to only have a single one.
  This behavior might change in the future.
  ~> The allowed_to_push, allowed_to_merge, allowed_to_unprotect, unprotect_access_level and code_owner_approval_required attributes require a GitLab Enterprise instance.
  Upstream API: GitLab REST API docs https://docs.gitlab.com/api/protected_branches/
---

# gitlab_branch_protection (Resource)

The `gitlab_branch_protection` resource allows to manage the lifecycle of a protected branch of a repository.

~> **Branch Protection Behavior for the default branch**
   Depending on the GitLab instance, group or project setting the default branch of a project is created automatically by GitLab behind the scenes.
   Due to [some](https://gitlab.com/gitlab-org/terraform-provider-gitlab/issues/792) [limitations](https://discuss.hashicorp.com/t/ignore-the-order-of-a-complex-typed-list/42242) in the Terraform Provider SDK and the GitLab API,
   when creating a new project and trying to manage the branch protection setting for its default branch the `gitlab_branch_protection` resource will
   automatically take ownership of the default branch without an explicit import by unprotecting and properly protecting it again.
   Having multiple `gitlab_branch_protection` resources for the same project and default branch will result in them overriding each other - make sure to only have a single one.
   This behavior might change in the future.

~> The `allowed_to_push`, `allowed_to_merge`, `allowed_to_unprotect`, `unprotect_access_level` and `code_owner_approval_required` attributes require a GitLab Enterprise instance.

**Upstream API**: [GitLab REST API docs](https://docs.gitlab.com/api/protected_branches/)

## Example Usage

```terraform
resource "gitlab_branch_protection" "BranchProtect" {
  project                      = "12345"
  branch                       = "BranchProtected"
  push_access_level            = "developer"
  merge_access_level           = "developer"
  unprotect_access_level       = "developer"
  allow_force_push             = true
  code_owner_approval_required = true
  allowed_to_push {
    user_id = 5
  }
  allowed_to_push {
    user_id = 521
  }
  allowed_to_merge {
    user_id = 15
  }
  allowed_to_merge {
    user_id = 37
  }
  allowed_to_unprotect {
    user_id = 15
  }
  allowed_to_unprotect {
    group_id = 42
  }
}

# Example using dynamic block
resource "gitlab_branch_protection" "main" {
  project                = "12345"
  branch                 = "main"
  push_access_level      = "maintainer"
  merge_access_level     = "maintainer"
  unprotect_access_level = "maintainer"

  dynamic "allowed_to_push" {
    for_each = [50, 55, 60]
    content {
      user_id = allowed_to_push.value
    }
  }
}
```

<!-- schema generated by tfplugindocs -->
## Schema

### Required

- `branch` (String) Name of the branch.
- `project` (String) The id of the project.

### Optional

- `allow_force_push` (Boolean) Can be set to true to allow users with push access to force push.
- `allowed_to_merge` (Block Set) Array of access levels and user(s)/group(s) allowed to merge to protected branch. (see [below for nested schema](#nestedblock--allowed_to_merge))
- `allowed_to_push` (Block Set) Array of access levels and user(s)/group(s) allowed to push to protected branch. (see [below for nested schema](#nestedblock--allowed_to_push))
- `allowed_to_unprotect` (Block Set) Array of access levels and user(s)/group(s) allowed to unprotect push to protected branch. (see [below for nested schema](#nestedblock--allowed_to_unprotect))
- `code_owner_approval_required` (Boolean) Can be set to true to require code owner approval before merging. Only available for Premium and Ultimate instances.
- `merge_access_level` (String) Access levels allowed to merge. Valid values are: `no one`, `developer`, `maintainer`.
- `push_access_level` (String) Access levels allowed to push. Valid values are: `no one`, `developer`, `maintainer`.
- `unprotect_access_level` (String) Access levels allowed to unprotect. Valid values are: `developer`, `maintainer`, `admin`.

### Read-Only

- `branch_protection_id` (Number) The ID of the branch protection (not the branch name).
- `id` (String) The ID of this Terraform resource. In the format of `<project-id:branch>`.

<a id="nestedblock--allowed_to_merge"></a>
### Nested Schema for `allowed_to_merge`

Optional:

- `group_id` (Number) The ID of a GitLab group allowed to perform the relevant action. Mutually exclusive with `user_id`.
- `user_id` (Number) The ID of a GitLab user allowed to perform the relevant action. Mutually exclusive with `group_id`.

Read-Only:

- `access_level` (String) Access levels allowed to merge to protected branch. Valid values are: `no one`, `developer`, `maintainer`.
- `access_level_description` (String) Readable description of access level.


<a id="nestedblock--allowed_to_push"></a>
### Nested Schema for `allowed_to_push`

Optional:

- `deploy_key_id` (Number) The ID of a GitLab deploy key allowed to perform the relevant action. Mutually exclusive with `group_id` and `user_id`. This field is read-only until Gitlab 17.5.
- `group_id` (Number) The ID of a GitLab group allowed to perform the relevant action. Mutually exclusive with `deploy_key_id` and `user_id`.
- `user_id` (Number) The ID of a GitLab user allowed to perform the relevant action. Mutually exclusive with `deploy_key_id` and `group_id`.

Read-Only:

- `access_level` (String) Access levels allowed to push to protected branch. Valid values are: `no one`, `developer`, `maintainer`.
- `access_level_description` (String) Readable description of access level.


<a id="nestedblock--allowed_to_unprotect"></a>
### Nested Schema for `allowed_to_unprotect`

Optional:

- `group_id` (Number) The ID of a GitLab group allowed to perform the relevant action. Mutually exclusive with `user_id`.
- `user_id` (Number) The ID of a GitLab user allowed to perform the relevant action. Mutually exclusive with `group_id`.

Read-Only:

- `access_level` (String) Access levels allowed to unprotect push to protected branch. Valid values are: `developer`, `maintainer`, `admin`.
- `access_level_description` (String) Readable description of access level.

## Import

Starting in Terraform v1.5.0, you can use an [import block](https://developer.hashicorp.com/terraform/language/import) to import `gitlab_branch_protection`. For example:

```terraform
import {
  to = gitlab_branch_protection.example
  id = "see CLI command below for ID"
}
```

Importing using the CLI is supported with the following syntax:

```shell
# Gitlab protected branches can be imported with a key composed of `<project_id>:<branch>`, e.g.
terraform import gitlab_branch_protection.BranchProtect "12345:main"
```
