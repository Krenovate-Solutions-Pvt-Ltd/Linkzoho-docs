# Image Management

Learn how LZ SKU Sync automatically downloads, optimizes, and manages product images from Zoho Inventory.

---

## Overview

LZ SKU Sync handles all aspects of product image management automatically:

- ✅ Downloads images from Zoho as ZIP archives
- ✅ Extracts and uploads to WordPress Media Library
- ✅ Generates standard WordPress thumbnails  
- ✅ Prevents duplicate images
- ✅ Assigns images to products and variations
- ✅ Manages gallery images for variable products

---

## How Image Download Works

### Step-by-Step Process

1. **Fetch ZIP**: Downloads all product images from Zoho
2. **Rate Limiting**: 2-second delay between requests (respects Zoho's 100 req/min limit)
3. **Retry Logic**: Up to 5 retries with 60-second backoff on rate limit errors
4. **Extract**: Unzips images to temporary directory
5. **Upload**: Adds images to WordPress Media Library
6. **Deduplicate**: Checks MD5 hash to prevent duplicates
7. **Attach**: Links images to product as featured or gallery
8. **Cleanup**: Deletes temporary files

---

## Image Optimization

### Thumbnail Generation

WordPress standard thumbnail sizes are automatically created:
- Thumbnail (150x150)
- Medium (300x300)
- Large (1024x1024)
- Plus any custom sizes defined by your theme

!!! note
    Image processing happens in the background via Action Scheduler to prevent timeouts.

---

## Variable Product Image Handling

For variable products (product groups in Zoho), images are managed intelligently:

### For Each Variation

1. Each variation gets its own images based on its `item_id`
2. Images are attached directly to the variation
3. First image becomes the variation's featured image

### For Parent Product

After all variations are processed:

- **Featured Image**: First variation's first image
- **Gallery Images**: Unique images from all variations (no duplicates)
- Parent images set via separate Action Scheduler job

---

## Preventing Duplicate Images

Images are tracked using MD5 hash to prevent uploading the same image multiple times:

### How It Works

1. **Hash Calculation**: MD5 hash generated from image file content
2. **Storage**: Hash stored in database as `_wzi_image_md5_{hash_value}` = `{attachment_id}`
3. **Check Before Upload**: Hash compared against existing media
4. **Reuse If Match**: If hash exists, existing attachment ID is reused

### Benefits

- ✅ Saves storage space
- ✅ Prevents database bloat
- ✅ Faster sync (no redundant uploads)
- ✅ Works across multiple syncs

!!! tip
    If you delete an image from Media Library and then re-sync, it will be downloaded again since the hash is cleared.

---

## Image Fields in Sync Settings

Control which image fields are synchronized:

### Product Image (Featured)
- Main product image
- First image from Zoho becomes featured image
- Shown on product listings and product page

### Gallery Images
- Additional product images
- All other images from Zoho added to gallery
- Displayed in product image carousel

!!! note
    Both options can be enabled/disabled independently in [Product Settings](product-settings.md).

---

## Image Metadata

When images are uploaded, the plugin stores:

- Original filename from Zoho
- Alt text (product name)
- Dimensions (width x height)
- MD5 hash (for deduplication)
- Product association

---

## Background Processing

Image downloads and processing happen asynchronously:

### Action Scheduler Jobs

- `wzi_upload_single_image` - Uploads one image to WordPress
- `wzi_process_image_metadata` - Generates thumbnails
- `wzi_assign_parent_images` - Assigns images to variable product parent

### Benefits

- ✅ Prevents PHP timeouts
- ✅ Handles large image batches
- ✅ Continues even if you close browser
- ✅ Full logging for debugging

!!! tip
    Check image processing status at **LZ SKU Sync > Sync Log** → Search for `Image Upload`

---

## Image Requirements

### Zoho Inventory

- Images must be attached to items in Zoho Inventory
- Supported formats: JPG, PNG, GIF, WebP
- Maximum recommended size: 5MB per image
- Multiple images per product supported

### WordPress

- GD or Imagick PHP extension recommended (for image processing)
- Uploads directory must be writable
- Sufficient disk space for images
- max_upload_size should accommodate ZIP files

---

## Common Image Scenarios

### Simple Product
- All images from Zoho → WordPress Media Library
- First image → Featured image
- Other images → Gallery

### Variable Product (Group)
- Each variation downloads its own images
- Parent product inherits variation images
- Duplicates automatically filtered out

### Product with No Images
- No images downloaded
- No featured image set
- Product syncs normally (without images)

---

## Troubleshooting Images

### Images Not Downloading

**Problem:** Products sync but images are missing

**Solutions:**

- ✅ Verify images exist in Zoho Inventory (Items → Attachments)
- ✅ Check Action Scheduler for errors: **LZ SKU Sync > Sync Log**
- ✅ Verify WordPress uploads directory permissions (should be 755)
- ✅ Check Zoho API rate limits (100 requests/min)

### Images Downloaded Multiple Times

**Problem:** Same image appears multiple times in Media Library

**Solutions:**

- MD5 deduplication should prevent this
- Check if hash is stored correctly: `SELECT * FROM wp_postmeta WHERE meta_key LIKE '_wzi_image_md5%'`
- Ensure plugin version is up to date

### Image Optimization Not Happening

**Problem:** Images not being optimized (e.g., WebP conversion)

**Solutions:**

- The plugin generates standard WordPress thumbnails only
- For advanced optimization (WebP, compression), use a dedicated image optimization plugin
- Recommended plugins: ShortPixel, Imagify, EWWW Image Optimizer

### Low Quality Images

**Problem:** Images look pixelated or compressed

**Solutions:**

- Upload higher resolution images to Zoho Inventory
- Check WordPress media settings: **Settings > Media** (increase max dimensions)
- Original image quality comes from Zoho - ensure Zoho has high-res versions

### Image Sync Taking Too Long

**Problem:** Image download is very slow

**Explanation:**

- Rate limiting: 2 seconds between requests (Zoho API limit)
- For 100 products with 3 images each = ~10 minutes minimum
- This is intentional to comply with Zoho's API limits

**Solutions:**

- Be patient - large catalogs take time
- Images download in background (can close browser)
- Check progress at **LZ SKU Sync > Sync Log**

---

## Best Practices

### Before Syncing

1. ✅ Upload high-quality images to Zoho Inventory
2. ✅ Use descriptive image filenames
3. ✅ Remove unnecessary images from Zoho to speed up sync

### During Sync

1. ✅ Let image processing complete in background
2. ✅ Don't cancel sync during image downloads
3. ✅ Monitor Action Scheduler for errors

### After Sync

1. ✅ Review products to ensure images are correct
2. ✅ Delete unused media via **Media > Library** (if needed)
3. ✅ Optimize remaining images with image optimization plugins (optional)

---

## Next Steps

- [**Configure Inventory Settings →**](inventory.md) - Manage warehouse and stock
- [**Monitor Sync Progress →**](monitoring.md) - Track image downloads
- [**Troubleshooting Guide →**](troubleshooting.md) - Solve image issues

---

## Support

Questions about image management?

- 📧 Email: sales@linkzoho.com, support@krenovate.com
- Include product SKU and image file names when reporting issues
