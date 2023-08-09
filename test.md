---
title: Organize CockroachDB Cloud Clusters Using Folders
summary: Learn how to use folders to organize CockroachDB Cloud clusters and manage access to them.
toc: true
docs_area: manage
---

{% include_cached feature-phases/limited-access.md %}

This page explains how to organize and manage access to your {{ site.data.products.db }} organization's clusters using folders. For more details about managing access to {{ site.data.products.db }} resources, refer to [Managing Users, Roles, and Service Accounts in {{ site.data.products.db }}](managing-access.html).

## How folders work

By default, clusters are created at the level of the organization. Folders provide additional ways to organize clusters and manage access to them. A folder can contain both clusters and descendent folders.

Each folder that you create has an ID. To create a cluster in a folder or to move a cluster into a folder, you set its `parent_id` field to the folder's ID. A folder can contain a mix of child folders and clusters.

If no parent ID is specified for a cluster, it is created at the level of the organization. Clusters and folders at the organization level have their `parent_id` set to `root`.

{{site.data.alerts.callout_success}}
Moving a folder or cluster is a lightweight operation that involves updating only a single property. Operations that involve multiple resources, such as moving a folder that contains multiple clusters and folders, are atomic; if all resources are not updated successfully, the operation is rolled back.
{{site.data.alerts.end}}

### Folder structure

Folders give you the flexibility to organize and manage access to your clusters using the structure that makes the most sense for your organization. Keep the following structural limitations in mind:

- Folders can be nested a maximum of three levels deep, including the organization level.

    In the following example, `Subfolder A` will be created successfully, but `Subfolder B2` will not, because it is nested too deeply:

    ~~~
    root
      -- Folder A
          -- Subfolder A
      -- Folder B
          -- Subfolder B
            -- Subfolder B2
    ~~~

add a line

- Each folder, including the root of the organization, can contain a maximum of 200 direct descendents, including folders or clusters.

Operations that would violate these limitations result in an error.
