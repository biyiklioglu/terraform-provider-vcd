---
layout: "vcd"
page_title: "vCloudDirector: vcd_catalog_item"
sidebar_current: "docs-vcd-data-source-catalog-item"
description: |-
  Provides a catalog item data source.
---

# vcd\_catalog\_item

Provides a vCloud Director Catalog item data source. A Catalog item can be used to reference a catalog item and use its 
data within other resources or data sources.

Supported in provider *v2.5+*

## Example Usage

```hcl
data "vcd_catalog_item" "my-first-item" {
  org     = "my-org"
  catalog = "my-cat"
  name    = "my-first-item"
}

resource "vcd_catalog_item" "my-second-item" {
  # Using the data source, two properties from another catalog items are
  # used in this resource.
  # You can read it as "use the org from catalog item `my-first-item`"
  # and "use the catalog from catalog item `my-first-item`"
  org     = "${data.vcd_catalog_item.my-first-item.org}"
  catalog = "${data.vcd_catalog_item.my-first-item.catalog}"

  name                 = "my-second-item"
  # The description uses the data source to create a dynamic text
  # The description will become "Belongs to my-cat"
  description          = "Belongs to ${data.vcd_catalog_item.my-first-item.catalog}"
  ova_path             = "/path/to/test_vapp_template.ova"
  upload_piece_size    = 5
  show_upload_progress = "true"
  metadata             = "${data.vcd_catalog_item.my-first-item.metadata}"
}
```

## Argument Reference

The following arguments are supported:

* `org` - (Optional, but required if not set at provider level) Org name 
* `catalog` - (Required) Catalog name
* `name` - (Required) Catalog Item name

## Attribute Reference

* `description` - Catalog item description.
* `metadata` -  Key value map of metadata.
