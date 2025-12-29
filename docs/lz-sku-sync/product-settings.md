# Product Sync Settings

Configure exactly what product data gets synchronized from Zoho Inventory to WooCommerce.

---

## Accessing Sync Preferences

1. Go to **LZ SKU Sync > Sync Status**
2. Find the **Sync Preferences** Tab
3. Configure the settings described below
4. Click **Save Preferences** after making changes

![Sync Preferences](images/sync-preferences.png)

---

## Warehouse Selection

### Sync Warehouse

Choose which Zoho Inventory warehouse to sync products from.

**Settings:**

- Dropdown lists all warehouses/locations in your organization
- Only items from the selected warehouse will be synced
- Stock quantities reflect the selected warehouse's inventory

!!! important
    **Multi-Warehouse Limitation**: You can only sync from one warehouse at a time. To switch warehouses, change the selection and run a new sync.

---

## Sync Frequency

Control when synchronization occurs.

### Options

**One-Time (Manual)**

- Sync only when you click "Start Sync" button
- Full control over timing
- Best for testing or occasional updates

**Always (Real-time webhook)**

- Automatic sync when items change in Zoho
- Real-time updates within seconds
- Best for ongoing operations

!!! tip
    See [Sync Methods](sync-methods.md) for detailed comparison of these options.

---

## Product Creation Options

Control whether new products are created or only existing products are updated.

### Create Missing Products (Simple)

When **enabled** ✅:

- Creates new simple products in WooCommerce if SKU doesn't exist
- Useful for adding new inventory to your store

When **disabled** ❌:

- Only updates existing simple products
- Ignores new SKUs from Zoho
- Useful when you want to prevent automatic product creation

### Create Missing Variations

When **enabled** ✅:

- Creates new variable products and variations if SKU doesn't exist
- Automatically generates WooCommerce attributes from Zoho variation data
- Useful for adding new product variants

When **disabled** ❌:

- Only updates existing variations
- Ignores new variation SKUs from Zoho
- Useful when you manage product structure manually in WooCommerce

!!! note
    **Existing Products**: Whether enabled or disabled, existing products matching the SKU will always be updated according to your field sync settings.

---

## Product Status

Choose the WordPress post status for newly created products.

### Available Options

**Draft** 📝

- Products saved but not visible on your store
- Allows manual review before publishing
- Best for quality control workflows

**Pending Review** 👀

- Products awaiting manual approval
- Visible to administrators only
- Best for multi-user approval processes

**Published** ✅ (Recommended)

- Products immediately visible on your storefront
- Skip manual review step
- Best for fully automated workflows

!!! tip
    **Recommended**: Use "Published" if you trust your Zoho Inventory data. Use "Draft" if you want to review products before making them live.

---

## Field Sync Settings

Enable or disable synchronization of specific product fields.

### Simple Product Fields

Control which fields are synced for simple products:

| Field | Description | Recommendation |
|-------|-------------|----------------|
| **Sales Price** | Regular price from Zoho | ✅ Always enable |
| **Stock Quantity** | Available stock from selected warehouse | ✅ Always enable |
| **Low Stock** | Reorder level threshold | ✅ Enable for inventory alerts |
| **Description** | Product long description | ✅ Enable for SEO |
| **Weight** | Product weight (with unit conversion) | ⚠️ Enable if you ship by weight |
| **Dimensions** | Length, Width, Height | ⚠️ Enable if needed for shipping |
| **Product Image** | Featured image | ✅ Enable for visual appeal |
| **Gallery Images** | Additional product images | ✅ Enable for better product display |

### Variable Product Fields

The same field options apply to variations, plus:

- Each variation gets its own SKU, price, stock, and images
- Parent product automatically inherits first variation's images
- Attributes are automatically extracted from Zoho `attribute_name*` fields

### Unit Conversions

The plugin automatically converts units from Zoho to WooCommerce:

**Weight**

- Supports: kg, g, lb, oz
- Converts to your WooCommerce weight unit

**Dimensions**

- Supports: cm, m, in, ft
- Converts to your WooCommerce dimension unit

!!! note
    Conversion is automatic. No manual configuration needed!

---

## Best Practice Configurations

### Initial Setup (First Sync)

```
✅ Create Missing Products (Simple): Enabled
✅ Create Missing Variations: Enabled
✅ Product Status: Draft (for review)
✅ All Fields: Enabled
```

**Why?** Import everything first, review products, then publish.

### Ongoing Operations (Webhook Sync)

```
✅ Sync Frequency: Always (Real-time webhook)
✅ Create Missing Products: Enabled
✅ Create Missing Variations: Enabled
✅ Product Status: Published
✅ All Fields: Enabled
```

**Why?** Fully automated real-time sync.

### Update Only (No New Products)

```
❌ Create Missing Products: Disabled
❌ Create Missing Variations: Disabled
✅ Product Status: Published
✅ Selected Fields: As needed
```

**Why?** Only update existing products, don't create new ones.

### Inventory-Only Sync

```
❌ Create Missing Products: Disabled
✅ Stock Quantity: Enabled
✅ Low Stock: Enabled
❌ Other Fields: Disabled
```

**Why?** Only sync stock levels, leave other data unchanged.

---

## Advanced Settings

### SKU Matching Logic

Products are matched between Zoho and WooCommerce using **SKU** as the unique identifier.

**For Simple Products:**
- Zoho `sku` → WooCommerce `_sku`

**For Variations:**
- Zoho variation `sku` → WooCommerce variation `_sku`

!!! important
    **SKUs must match exactly** (case-sensitive). Mismatched SKUs will result in duplicate products if "Create Missing Products" is enabled.

### Product Type Detection

The plugin automatically determines product type:

- **Simple Product**: Zoho item with no `group_id`
- **Variation**: Zoho item with a `group_id` (part of product group)
- **Variable Product (Parent)**: Created from product group in Zoho

---

## What Doesn't Get Synced?

The following are **not currently synced** from Zoho to WooCommerce:

- ❌ Product categories/tags
- ❌ Custom fields (unless mapped)
- ❌ Product reviews
- ❌ Sales data
- ❌ Purchase price (cost of goods)
- ❌ Supplier information

!!! note
    LZ SKU Sync is **one-way** from Zoho to WooCommerce. Changes in WooCommerce are not sent back to Zoho.

---

## Saving Your Preferences

After configuring all settings:

1. Scroll to the bottom of the Sync Preferences section
2. Click **Save Preferences**
3. You'll see a success message
4. Settings are now active for all future syncs

---

## Next Steps

Once you've configured your sync preferences:

- [**Run Your First Sync →**](sync-methods.md) - Start synchronizing products
- [**Learn About Image Management →**](image-management.md) - Understand how images are handled
- [**Monitor Sync Progress →**](monitoring.md) - Track what's happening

---

## Troubleshooting Settings

### Changes Not Taking Effect

**Problem:** Settings don't seem to apply

**Solution:**

- Ensure you clicked "Save Preferences"
- Refresh the page to verify settings were saved
- For webhooks, changes apply to new events only (not retroactive)

### Too Many Products Created

**Problem:** Duplicate or unwanted products appearing

**Solution:**

- Disable "Create Missing Products"
- Check SKU matching in Zoho vs WooCommerce
- Run manual sync after fixing SKUs to update existing products

### Images Not Syncing

**Problem:** Product Image/Gallery Image enabled but images missing

**Solution:**

- Verify products have images in Zoho Inventory
- Check Action Scheduler for image download errors
- See [Image Management](image-management.md) for detailed troubleshooting

---

## Support

Questions about sync settings?

- 📧 Email: sales@linkzoho.com, support@krenovate.com
- 📖 [View Full Documentation](index.md)
