---
name: Manage and share files in the CTERA Portal
description: Authenticate to a CTERA Portal, organize a folder tree, upload files, and share them via a public link using the official mcp-ctera-core server.
api: mcp/ctera-mcp.yml
server: mcp-ctera-core
operations:
  - ctera_portal_who_am_i
  - ctera_portal_list_dir
  - ctera_portal_makedirs
  - ctera_portal_upload_file
  - ctera_portal_create_public_link
  - ctera_portal_list_versions
  - ctera_portal_delete_items
  - ctera_portal_recover_items
---

# Manage and share files in the CTERA Portal

Use the official `mcp-ctera-core` MCP server (Apache-2.0) to operate the CTERA
Portal global namespace. All tool names below are the real, published tools in
`mcp/ctera-mcp.yml`.

## Prerequisites

- The server is configured with `ctera.mcp.core.settings.host`, `user`,
  `password`, `scope`, and `ssl` (env vars). Auth is CTERA administrator
  username/password establishing a session cookie (see
  `authentication/ctera-authentication.yml`); sessions time out after 30 minutes
  of inactivity.
- Choose your working scope first: `ctera_portal_browse_team_portal` to work
  inside a tenant, or `ctera_portal_browse_global_admin` for global admin.

## Steps

1. Confirm identity with `ctera_portal_who_am_i` to verify the authenticated
   user and domain before making changes.
2. Inspect the target location with `ctera_portal_list_dir` on the parent path.
3. Create the destination tree with `ctera_portal_makedirs` (recursive) if it
   does not yet exist.
4. Upload the document with `ctera_portal_upload_file` (or
   `ctera_portal_upload_from_content` for in-memory content).
5. Share it by calling `ctera_portal_create_public_link` for the uploaded item;
   use `ctera_portal_get_permalink` for an internal permalink instead.
6. To review history, call `ctera_portal_list_versions` on the file.

## Cleanup / recovery

- Remove items with `ctera_portal_delete_items`.
- If something was deleted in error, restore it with
  `ctera_portal_recover_items`.

## Error handling

The underlying Portal REST API returns standard HTTP status codes — `403`
(invalid/insufficient credentials), `404` (object not found), `400`/`500` carry
error text (see `errors/ctera-problem-types.yml`). On `401`, re-establish the
session. There is no documented idempotency-key contract, so avoid blind retries
of `create`/`upload` operations without first re-listing the directory.
