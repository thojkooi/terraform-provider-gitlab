---
# generated by https://github.com/hashicorp/terraform-plugin-docs
page_title: "gitlab_project_tag Resource - terraform-provider-gitlab"
subcategory: ""
description: |-
  The gitlab_project_tag resource allows to manage the lifecycle of a tag in a project.
  Upstream API: GitLab API docs https://docs.gitlab.com/api/tags/
---

# gitlab_project_tag (Resource)

The `gitlab_project_tag` resource allows to manage the lifecycle of a tag in a project.

**Upstream API**: [GitLab API docs](https://docs.gitlab.com/api/tags/)

## Example Usage

```terraform
# Create a project for the tag to use
resource "gitlab_project" "example" {
  name         = "example"
  description  = "An example project"
  namespace_id = gitlab_group.example.id
}

resource "gitlab_project_tag" "example" {
  name    = "example"
  ref     = "main"
  project = gitlab_project.example.id
}
```

<!-- schema generated by tfplugindocs -->
## Schema

### Required

- `name` (String) The name of a tag.
- `project` (String) The ID or URL-encoded path of the project owned by the authenticated user.
- `ref` (String) Create tag using commit SHA, another tag name, or branch name. This attribute is not available for imported resources.

### Optional

- `message` (String) The message of the annotated tag.

### Read-Only

- `commit` (Set of Object) The commit associated with the tag. (see [below for nested schema](#nestedatt--commit))
- `id` (String) The ID of this resource.
- `protected` (Boolean) Bool, true if tag has tag protection.
- `release` (Set of Object) The release associated with the tag. (see [below for nested schema](#nestedatt--release))
- `target` (String) The unique id assigned to the commit by Gitlab.

<a id="nestedatt--commit"></a>
### Nested Schema for `commit`

Read-Only:

- `author_email` (String)
- `author_name` (String)
- `authored_date` (String)
- `committed_date` (String)
- `committer_email` (String)
- `committer_name` (String)
- `id` (String)
- `message` (String)
- `parent_ids` (Set of String)
- `short_id` (String)
- `title` (String)


<a id="nestedatt--release"></a>
### Nested Schema for `release`

Read-Only:

- `description` (String)
- `tag_name` (String)

## Import

Starting in Terraform v1.5.0, you can use an [import block](https://developer.hashicorp.com/terraform/language/import) to import `gitlab_project_tag`. For example:

```terraform
import {
  to = gitlab_project_tag.example
  id = "see CLI command below for ID"
}
```

Importing using the CLI is supported with the following syntax:

```shell
# Gitlab project tags can be imported with a key composed of `<project_id>:<tag_name>`, e.g.
terraform import gitlab_project_tag.example "12345:develop"

# NOTE: the `ref` attribute won't be available for imported `gitlab_project_tag` resources.
```
