---
name: Perform file operations on a CTERA Edge Filer
description: Authenticate to a CTERA Edge Filer and list, create, upload, download, move, and delete files using the official mcp-ctera-edge server.
api: mcp/ctera-mcp.yml
server: mcp-ctera-edge
operations:
  - ctera_edge_filer_who_am_i
  - ctera_edge_filer_list_dir
  - ctera_edge_filer_makedirs
  - ctera_edge_filer_upload_file
  - ctera_edge_filer_download_file
  - ctera_edge_filer_move_item
  - ctera_edge_filer_delete_item
---

# Perform file operations on a CTERA Edge Filer

Use the official `mcp-ctera-edge` MCP server (Apache-2.0) to operate a CTERA Edge
Filer appliance. All tool names below are the real, published tools in
`mcp/ctera-mcp.yml`.

## Prerequisites

- The server is configured with `ctera.mcp.edge.settings.host`, `user`
  (default `admin`), `password`, and `ssl` (env vars). Auth uses the Edge Filer
  admin credentials.

## Steps

1. Confirm the connection with `ctera_edge_filer_who_am_i`.
2. Browse the appliance with `ctera_edge_filer_list_dir` on the target path.
3. Create any needed folders with `ctera_edge_filer_create_directory` (single)
   or `ctera_edge_filer_makedirs` (recursive tree).
4. Transfer data:
   - Upload with `ctera_edge_filer_upload_file` (or
     `ctera_edge_upload_from_content` for in-memory bytes).
   - Download with `ctera_edge_filer_download_file`, or inspect content in place
     with `ctera_edge_filer_read_file`.
5. Reorganize with `ctera_edge_filer_copy_item` and `ctera_edge_filer_move_item`.
6. Remove with `ctera_edge_filer_delete_item`.

## Notes

- The Edge Filer surface has no version-history or public-link tools — those live
  on the Portal server (`mcp-ctera-core`). Use the Portal skill for sharing and
  versioning.
- There is no documented idempotency-key contract; re-list the directory before
  retrying a failed create/upload rather than blindly repeating it.
