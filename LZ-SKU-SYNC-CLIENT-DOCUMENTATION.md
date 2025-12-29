# LZ SKU Sync - Complete Plugin Documentation

**Version:** 1.3.0  
**Developer:** Krenovate Agency  
**Website:** [https://linkzoho.com](https://linkzoho.com)  
**License:** GPL-2.0+

---

## Table of Contents

1. [Plugin Overview](#plugin-overview)
2. [Key Features](#key-features)
3. [System Requirements](#system-requirements)
4. [Installation & Activation](#installation--activation)
5. [Initial Setup & Configuration](#initial-setup--configuration)
6. [Synchronization Methods](#synchronization-methods)
7. [Product Sync Settings](#product-sync-settings)
8. [Image Management](#image-management)
9. [Inventory Management](#inventory-management)
10. [Sync Status & Monitoring](#sync-status--monitoring)
11. [Webhook Integration](#webhook-integration)
12. [Troubleshooting](#troubleshooting)
13. [Technical Architecture](#technical-architecture)
14. [FAQ](#faq)

---

## Plugin Overview

**LZ SKU Sync** is a premium WordPress plugin that automates product synchronization between **Zoho Inventory** and **WooCommerce**. It eliminates manual data entry by syncing product information, inventory levels, images, and attributes in real-time, ensuring your online store always reflects accurate product data.

### What Problems Does It Solve?

- **Manual Data Entry**: Eliminates the need to manually update products in WooCommerce when changes occur in Zoho Inventory
- **Inventory Inaccuracies**: Keeps stock levels synchronized automatically to prevent overselling
- **Time-Consuming Updates**: Automates bulk product synchronization with one-click manual sync
- **Product Image Management**: Automatically downloads and attaches product images from Zoho Inventory
- **Multi-Warehouse Support**: Syncs inventory from specific warehouses in your Zoho account
- **Real-Time Updates**: Uses webhooks to instantly reflect Zoho changes in WooCommerce

---

## Key Features

### 1. **Dual Synchronization Modes**
- **Manual Sync (One-Time)**: Bulk synchronize all products on-demand
- **Real-Time Webhook Sync**: Automatic synchronization when products are created/updated in Zoho Inventory

### 2. **Comprehensive Product Support**
- **Simple Products**: Sync standalone products with full details
- **Variable Products**: Sync product groups with variations (attributes and options)
- **Product Attributes**: Automatically create and map WooCommerce attributes from Zoho variations
- **SKU Management**: Matches products by SKU for accurate updates

### 3. **Rich Product Data Sync**
- Product Name
- Description
- SKU
- Sales Price/Regular Price
- Stock Quantity
- Low Stock Threshold (Reorder Level)
- Weight (with unit conversion)
- Dimensions (Length, Width, Height with unit conversion)
- Product Images (Featured and Gallery)

### 4. **Advanced Image Management**
- Downloads product images from Zoho Inventory as ZIP archives
- Extracts and uploads images to WordPress Media Library
- Generates optimized WebP thumbnails for better performance
- Prevents duplicate images using MD5 hash verification
- Assigns images to parent variable products from variations
- Sets featured images and gallery images automatically

### 5. **Intelligent Inventory Management**
- Multi-warehouse support (select specific warehouse for sync)
- Real-time stock quantity updates
- Stock status management (In Stock/Out of Stock)
- Low stock notifications
- Inventory overview dashboard with statistics

### 6. **Granular Sync Control**
- Choose to create missing products or only update existing ones
- Separate controls for simple products and variations
- Configurable product status (Draft, Pending Review, or Published)
- Selective field synchronization (enable/disable specific product fields)

### 7. **Batch Processing & Performance**
- Processes products in batches to prevent timeouts
- Uses WordPress Action Scheduler for background processing
- Queue-based architecture for reliable synchronization
- Pause/Resume/Cancel controls for manual syncs
- Progress tracking with real-time updates

### 8. **Comprehensive Logging & Monitoring**
- Sync status dashboard with live progress
- Recently synced items log (30-day history)
- Action Scheduler integration for detailed job logs
- Error tracking and reporting
- Webhook activity monitoring

### 9. **License Management**
- Secure license activation with order ID and license key
- Automatic plugin updates from GitHub
- License validation and expiry management
- Multi-site support

### 10. **Developer-Friendly Architecture**
- Clean, well-documented codebase
- WordPress coding standards compliant
- Hook-based architecture for extensibility
- Separate API classes for different Zoho endpoints
- RESTful webhook endpoint

---

## System Requirements

### WordPress Environment
- **WordPress Version**: 4.7 or higher
- **PHP Version**: 7.4 or higher (8.0+ recommended)
- **WooCommerce Version**: 7.9 or higher (tested up to 9.7.1)
- **MySQL Version**: 5.6 or higher
- **Server Memory**: 128MB minimum (256MB recommended)
- **Max Execution Time**: 300 seconds recommended for large syncs

### Required PHP Extensions
- `cURL` - For API communications
- `JSON` - For data serialization
- `ZIP` - For extracting product images
- `GD` or `Imagick` - For image processing (WebP generation)
- `OpenSSL` - For encryption

### Zoho Inventory Requirements
- Active Zoho Inventory account
- API access enabled (included in all Zoho Inventory plans)
- Valid organization with products and items

### WordPress Plugin Dependencies
- **WooCommerce** (Required): Plugin will not activate without WooCommerce
- **Action Scheduler** (Bundled with WooCommerce): For background processing

---

## Installation & Activation

### Step 1: Install WooCommerce

The plugin **requires WooCommerce** to function. If not already installed:

1. Go to **Plugins > Add New** in WordPress admin
2. Search for "WooCommerce"
3. Click **Install Now** then **Activate**
4. Complete WooCommerce setup wizard

### Step 2: Upload LZ SKU Sync Plugin

1. Purchase and download the plugin ZIP file from [linkzoho.com](https://linkzoho.com)
2. Log in to your WordPress admin panel
3. Navigate to **Plugins > Add New > Upload Plugin**
4. Click **Choose File** and select the `LZ-SKU-Sync.zip` file
5. Click **Install Now**
6. Once installed, click **Activate Plugin**

### Step 3: Activate Your License

After activation, you'll be redirected to the **LZ SKU Sync** settings page:

1. Navigate to **LZ SKU Sync > Settings** (or you'll be redirected automatically)
2. You'll see the **License Activation** screen
3. Enter your **Order ID** and **License Key** (received via email after purchase)
4. Click **Activate License**

> **Important**: License activation connects to LinkZoho's servers to validate your purchase and enable automatic updates.

### Step 4: Check Pre-Requisites

After license activation, the plugin will check:

- ✅ WooCommerce is active
- ✅ PHP version meets requirements
- ✅ Required PHP extensions are installed
- ✅ File system permissions are correct

If any issues are detected, you'll see warnings with instructions to resolve them.

---

## Initial Setup & Configuration

### Authentication with Zoho Inventory

#### Step 1: Connect to Zoho

1. Go to **LZ SKU Sync > Settings > Authentication** tab
2. Click the **Connect with Zoho** button
3. You'll be redirected to the **LinkZoho OAuth Bridge** (app.linkzoho.com)
4. Click **Authorize** to grant permissions to Zoho Inventory
5. Log in to your Zoho account if prompted
6. Select **Zoho Inventory Full Access** scope
7. Click **Accept** to approve the connection
8. You'll be redirected back to your WordPress site

#### Step 2: Select Organization

If your Zoho account has multiple organizations:

1. A dropdown will appear with all available organizations
2. Select the organization you want to sync from
3. Click **Save Organization**

The plugin stores:
- Access Token (auto-refreshed every 58 minutes)
- Refresh Token
- Organization ID
- Zoho API Domain (based on your data center: .com, .in, .eu, .com.au, .jp, .ca, .com.cn, .sa)

---

## Synchronization Methods

### Method 1: Manual Sync (One-Time)

Best for initial setup or occasional bulk updates.

#### How It Works:

1. Navigate to **LZ SKU Sync > Sync Status**
2. Configure **Sync Preferences** (see section below)
3. Click **Start Sync** button
4. Monitor progress in real-time
5. View recently synced items as they complete

#### Sync Process Flow:

```
1. Fetch Inventory → Retrieves all items from selected warehouse
2. Compare SKUs → Matches Zoho items with WooCommerce products
3. Prepare Queue → Creates list of products to create/update
4. Batch Processing → Processes products one at a time via Action Scheduler
5. Image Download → Fetches and attaches product images
6.  Image Optimization → Generates WebP thumbnails
7. Variation Handling → Creates parent variable products with variations
8. Completion → Updates sync status and logs
```

#### Sync Controls:

- **Pause**: Temporarily stops the sync (can be resumed later)
- **Resume**: Continues a paused sync from where it stopped
- **Cancel**: Stops the sync permanently (cannot be resumed)

### Method 2: Real-Time Webhook Sync (Always-On)

Best for continuous operations where Zoho is the source of truth.

#### How It Works:

1. Navigate to **LZ SKU Sync > Sync Status**
2. Set **Sync Frequency** to **Always (Real-time webhook)**
3. Click **Save Preferences**
4. The plugin automatically:
   - Creates a webhook in Zoho Inventory
   - Creates a workflow rule to trigger on item create/update
   - Sets up a REST API endpoint in WordPress

#### Webhook Flow:

```
Zoho Event (Item Created/Updated)
        ↓
Zoho Workflow Rule Triggers
        ↓
Webhook POST to WordPress
        ↓
Plugin Validates Signature (HMAC SHA256)
        ↓
Fetches Full Item Details
        ↓
Enqueues Sync Job via Action Scheduler
        ↓
Product Created/Updated in WooCommerce
```

#### Security:

- **HMAC Signature Verification**: Every webhook request is signed with your license key
- **License Validation**: Active license required to receive webhooks
- **Sync Frequency Check**: Webhooks only processed when enabled in settings

#### Toggle Webhook:

- **Active**: Green indicator, receiving real-time updates
- **Inactive**: Red indicator, webhooks paused (click to activate)

---

## Product Sync Settings

### Sync Preferences

Navigate to **LZ SKU Sync > Sync Status** to configure:

#### 1. **Sync Warehouse**

Select which Zoho Inventory warehouse to sync from:
- Dropdown lists all warehouses/locations in your organization
- Only items from the selected warehouse will be synced
- Stock quantities will reflect the selected warehouse's inventory

#### 2. **Sync Frequency**

- **One-Time**: Manual sync only (click Start Sync button)
- **Always (Real-time)**: Webhook-based automatic sync

#### 3. **Product Creation Options**

##### Create Missing Products (Simple)
- ✅ **Enabled**: Creates new simple products in WooCommerce if SKU doesn't exist
- ❌ **Disabled**: Only updates existing simple products, ignores new ones

##### Create Missing Variations
- ✅ **Enabled**: Creates new variable products and variations if SKU doesn't exist
- ❌ **Disabled**: Only updates existing variations, ignores new ones

#### 4. **Product Status**

Choose the WordPress post status for newly created products:
- **Draft**: Products saved but not visible on store
- **Pending Review**: Awaiting manual approval
- **Published**: Immediately visible on store (default)

### Product Field Sync Settings

#### Simple Product Fields

Toggle which fields to sync for simple products:

| Field | Description | Note |
|-------|-------------|------|
| **Sales Price** | Regular/Sale price from Zoho [rate](file:///home/sudhar/Local%20Sites/linkzoho/app/public/wp-content/plugins/LZ-SKU-Sync/includes/API/class-zoho-inventory-api-authentication.php#164-202) field | Always synced to regular_price |
| **Available Stock Quantity** | Current stock from selected warehouse | Sets stock_quantity and stock_status |
| **Low Stock** | Reorder level threshold | Sets WooCommerce low_stock_amount |
| **Description** | Product long description | Synced to post_content |
| **Weight** | Product weight with unit conversion | Converts kg, g, lb, oz to WooCommerce unit |
| **Dimensions** | Length, Width, Height | Converts cm, m, in, ft to WooCommerce unit |
| **Product Image** | Featured image | First image in Zoho becomes featured |
| **Gallery Image** | Additional product images | All other images added to gallery |

#### Variable Product Fields

Same fields as simple products, applied to variations:
- Each variation gets its own SKU, price, stock, images
- Parent product inherits first variation's images
- Attributes automatically extracted from Zoho `attribute_name*` and `attribute_option_name*` fields

---

## Image Management

### Image Download Process

When a product is synced:

1. **Fetch ZIP**: Downloads images from Zoho API endpoint `/items/{item_id}/images`
2. **Rate Limiting**: 2-second delay between requests to respect Zoho's 100 req/min limit
3. **Retry Logic**: Up to 5 retries with 60-second backoff on rate limit errors
4. **Extract**: Unzips images to temporary directory
5. **Upload**: Adds images to WordPress Media Library
6. **Deduplicate**: Checks MD5 hash to prevent duplicate uploads
7. **Attach**: Links images to product as featured or gallery
8. **Cleanup**: Deletes temporary files

### Image Optimization

For each uploaded image:

1. **Generate Thumbnails**: WordPress creates standard thumbnail sizes
2. **WebP Conversion**: Plugin generates WebP versions for modern browsers
3. **Metadata**: Stores original filename, alt text, dimensions
4. **Action Scheduler**: Image processing happens in background to avoid timeouts

### Variable Product Image Handling

For variable products (product groups in Zoho):

1. Each variation gets its own images based on its `item_id`
2. After all variations are processed:
   - **Featured Image**: First variation's first image
   - **Gallery Images**: Unique images from all variations (no duplicates)
3. Parent product images set via separate Action Scheduler job

### Preventing Duplicates

Images are tracked by MD5 hash:
- Hash calculated from image file content
- Stored in postmeta: `_wzi_image_md5_{hash_value}` = `{attachment_id}`
- Before upload, hash is checked against existing media
- If match found, existing attachment ID is reused

---

## Inventory Management

### Inventory Overview Dashboard

Access via **LZ SKU Sync > Inventory Overview**:

#### Statistics Display

- **Total Products**: Count of all WooCommerce products
- **Simple Products**: Count with/without SKU
- **Variable Products**: Count of parent products
- **Variations**: Count with/without SKU
- **Zoho Items**: Total items in selected warehouse
- **Ungrouped Items**: Simple products in Zoho
- **Grouped Items**: Product groups (variations) in Zoho
- **Stock Totals**: Available vs. Actual stock for each category

#### SKU Comparison Tables

##### Matching SKUs
- Lists SKUs that exist in both Zoho and WooCommerce
- Shows current sync status

##### Missing in WooCommerce
- Zoho SKUs not found in WooCommerce
- Indicates products that will be created on next sync (if enabled)

##### Missing in Zoho
- WooCommerce SKUs not in Zoho selected warehouse
- May indicate discontinued products or warehouse mismatch

### Warehouse Management

The plugin supports Zoho Inventory's multi-warehouse feature:

1. Fetches all warehouses/locations via `/inventory/v1/settings/warehouses` API
2. Fallback to `/inventory/v1/locations` for older Zoho accounts
3. Normalizes data to standard format (`warehouse_id`, `warehouse_name`)
4. Stock quantities pulled from warehouse-specific inventory levels

---

## Sync Status & Monitoring

### Real-Time Sync Status

Located in **LZ SKU Sync > Sync Status** tab:

#### Status Indicators

- **Not Started**: No sync has been initiated
- **In Progress**: Active synchronization (shows progress bar)
- **Paused**: Sync temporarily stopped (can resume)
- **Cancelled**: Sync terminated (cannot resume)
- **Completed**: All products processed successfully

#### Progress Metrics

- **Total Products**: Number of products queued for sync
- **Synced**: Successfully processed products
- **Failed**: Products that encountered errors
- **Pending**: Products waiting in queue
- **Progress Percentage**: Visual progress bar

### Recently Synced Items

Displays last 30 days of sync activity in real-time table:

| Column | Description |
|--------|-------------|
| **Product Name** | Name from Zoho |
| **SKU** | Product SKU |
| **Type** | Simple, Group, or Variation |
| **Action** | Created, Updated, Failed, Image Upload, etc. |
| **Date** | Timestamp of sync |
| **Action ID** | Link to Action Scheduler logs |

#### Table Features

- **Pagination**: 10 items per page
- **Auto-Refresh**: Updates every 3 seconds during active sync
- **Search**: Filter by name or SKU
- **Export**: Download sync history as CSV

### Sync Log

Detailed 30-day log of all sync activities:

- Manual sync events
- Webhook sync events
- Image processing jobs
- Variation creation jobs
- Parent image assignment jobs
- Error details

Access full logs via **Tools > Action Scheduler**:
- Filter by group: `wzi_manual_sync_group`, `wzi_webhook_sync_group`, `wzi_variation_processing_group`
- View execution logs for each action
- Check for failed actions and error messages

---

## Webhook Integration

### Webhook Architecture

#### Components:

1. **Zoho Webhook**: Receives POST requests from Zoho when items change
2. **Zoho Workflow Rule**: Triggers webhook on item create/update events
3. **WordPress REST API Endpoint**: `/wp-json/lz-sku-sync/v1/webhook`
4. **Signature Verification**: HMAC SHA256 with license key secret
5. **Item Fetcher**: Retrieves full item details from Zoho API
6. **Sync Queue**: Enqueues background job via Action Scheduler

#### Webhook Lifecycle:

##### Creation
1. User enables "Always (Real-time)" sync
2. Plugin calls `/inventory/v1/settings/webhooks` to create webhook
3. Plugin calls `/inventory/v1/settings/workflows` to create workflow rule
4. Workflow rule set to trigger on "Item Add/Edit" events
5. Status saved as "active" in database

##### Deactivation
1. User disables webhook or changes sync frequency
2. Plugin calls `/inventory/v1/settings/workflows/{id}/inactive`
3. Status updated to "inactive" (webhook remains but doesn't trigger)

##### Deletion
1. User deactivates plugin
2. Plugin calls DELETE `/inventory/v1/settings/workflows/{id}`
3. Plugin calls DELETE `/inventory/v1/settings/webhooks/{id}`
4. Cleanup on plugin uninstall

### Webhook Request Format

Zoho sends POST with:

**Headers:**
```
Content-Type: application/json
X-Zoho-Webhook-Signature: {hmac_sha256_signature}
```

**Body:**
```json
{
  "item": {
    "item_id": "1234567890",
    "sku": "PROD-001",
    "name": "Sample Product",
    "rate": 99.99,
    "available_stock": 10,
    "group_id": "",
    ...
  }
}
```

### Security & Validation

1. **Signature Verification**: 
   ```php
   $secret = substr($license_key, 0, 50);
   $computed = hash_hmac('sha256', $request_body, $secret);
   if (!hash_equals($computed, $received_signature)) {
       return 403 Forbidden;
   }
   ```

2. **License Validation**: Active license required

3. **Sync Frequency Check**: Webhooks only processed if enabled

4. **Rate Limiting**: Zoho respects 100 requests/minute limit

### Monitoring Webhooks

- **Last Received**: Timestamp of last successful webhook
- **Last Data**: Full JSON of last received item
- **Workflow Status**: Active/Inactive indicator
- **Sync Log**: All webhook-triggered syncs with timestamps

---

## Troubleshooting

### Common Issues

#### 1. **Sync Stuck/Not Progressing**

**Symptoms**: Sync shows "In Progress" but no products being processed

**Solutions**:
- Check Action Scheduler for failed actions: **LZ SKU Sync > Sync Log**
- Increase PHP `max_execution_time` to 300+ seconds
- Increase WordPress memory limit to 256MB

#### 2. **Authentication Errors**

**Symptoms**: "Invalid authentication" or "Token expired" errors

**Solutions**:
- Tokens auto-refresh every 58 minutes
- If refresh fails, go to **Authentication** tab and reconnect
- Verify organization is still accessible in Zoho
- Check Zoho API rate limits haven't been exceeded

#### 3. **Images Not Downloading**

**Symptoms**: Products sync but images missing

**Solutions**:
- Verify product has attachments in Zoho Inventory
- Check Action Scheduler for image download errors
- Ensure PHP ZIP extension is installed
- Verify WordPress uploads directory is writable
- Check Zoho API rate limits (100 req/min)

#### 4. **Webhook Not Receiving**

**Symptoms**: Changes in Zoho don't reflect in WooCommerce

**Solutions**:
- Verify sync frequency is set to "Always (Real-time)"
- Check webhook status indicator (should be green/active)
- Test webhook endpoint: `curl -X POST https://yoursite.com/wp-json/lz-sku-sync/v1/webhook`
- Check webhook logs in Zoho Inventory settings
- Verify license is active

#### 5. **Variations Not Creating**

**Symptoms**: Variable products sync but variations missing

**Solutions**:
- Enable "Create Missing Variations" in sync preferences
- Verify variations have SKUs in Zoho
- Check that `attribute_name` fields are populated in Zoho
- Review Action Scheduler for variation processing errors

### Debug Mode

Enable WordPress debug logging:

```php
// wp-config.php
define('WP_DEBUG', true);
define('WP_DEBUG_LOG', true);
define('WP_DEBUG_DISPLAY', false);
```

Check logs at: `wp-content/debug.log`

The plugin logs:
- API request/response details
- Action Scheduler execution logs
- Image processing errors
- Webhook signature verification issues

---

## Technical Architecture

### Database Schema

#### Custom Table: `wp_lz_sku_sync`

Stores all plugin configuration and state:

| Field | Type | Description |
|-------|------|-------------|
| [id](file:///home/sudhar/Local%20Sites/linkzoho/app/public/wp-content/plugins/LZ-SKU-Sync/includes/API/class-zoho-inventory-product-sync.php#108-120) | INT | Auto-increment primary key |
| `field_name` | VARCHAR(191) | Configuration key |
| `field_value` | LONGTEXT | Serialized value  |

#### Key Database Fields:

- [access_token](file:///home/sudhar/Local%20Sites/linkzoho/app/public/wp-content/plugins/LZ-SKU-Sync/includes/API/class-zoho-inventory-api-base.php#120-137) - Zoho OAuth access token
- `refresh_token` - Zoho OAuth refresh token
- `token_expiry` - Token expiration timestamp
- `organization_id` - Selected Zoho organization
- `warehouse_id` - Selected warehouse for sync
- `zoho_inventory_url` - API domain (e.g., inventoryapi.zoho.com)
- `sync_preferences` - Serialized sync configuration
- [sync_status](file:///home/sudhar/Local%20Sites/linkzoho/app/public/wp-content/plugins/LZ-SKU-Sync/includes/API/class-zoho-inventory-product-sync.php#744-782) - Current sync state and progress
- `products_to_sync` - Queue of products for current sync
- [sync_log](file:///home/sudhar/Local%20Sites/linkzoho/app/public/wp-content/plugins/LZ-SKU-Sync/includes/API/class-zoho-inventory-product-sync.php#885-947) - 30-day sync history
- [webhook_sync_log](file:///home/sudhar/Local%20Sites/linkzoho/app/public/wp-content/plugins/LZ-SKU-Sync/includes/API/class-zoho-inventory-product-sync.php#948-987) - Webhook-triggered sync history
- `webhook_id` - Zoho webhook ID
- `workflow_id` - Zoho workflow rule ID
- `workflow_status` - Active/Inactive
- `plugins_activation` - License details
- `last_inventory_sync` - Timestamp of last inventory fetch
- `last_inventory_sync_response` - Full inventory comparison data

### Class Architecture

#### Core Classes

1. **LZ_SKU_Sync** ([class-lz-sku-sync.php](file:///home/sudhar/Local%20Sites/linkzoho/app/public/wp-content/plugins/LZ-SKU-Sync/includes/class-lz-sku-sync.php))
   - Main plugin orchestrator
   - Loads dependencies
   - Registers hooks and actions

2. **LZ_SKU_Sync_Loader** ([class-lz-sku-sync-loader.php](file:///home/sudhar/Local%20Sites/linkzoho/app/public/wp-content/plugins/LZ-SKU-Sync/includes/class-lz-sku-sync-loader.php))
   - Hook registration manager
   - Centralized action/filter management

3. **LZ_SKU_Sync_Integration_DB_Operations** ([class-lz-sku-sync-integration-db-operations.php](file:///home/sudhar/Local%20Sites/linkzoho/app/public/wp-content/plugins/LZ-SKU-Sync/includes/class-lz-sku-sync-integration-db-operations.php))
   - CRUD operations for custom table
   - Field serialization/deserialization
   - Data access layer

#### API Classes

4. **Zoho_Inventory_API_Base** ([class-zoho-inventory-api-base.php](file:///home/sudhar/Local%20Sites/linkzoho/app/public/wp-content/plugins/LZ-SKU-Sync/includes/API/class-zoho-inventory-api-base.php))
   - Base class for all API interactions
   - Handles authentication
   - Manages access token refresh
   - Provides API request helper methods

5. **Zoho_Inventory_API_Authentication** ([class-zoho-inventory-api-authentication.php](file:///home/sudhar/Local%20Sites/linkzoho/app/public/wp-content/plugins/LZ-SKU-Sync/includes/API/class-zoho-inventory-api-authentication.php))
   - OAuth 2.0 flow implementation
   - Token exchange and refresh
   - Multi-datacenter support

6. **Zoho_Inventory_Items_API** ([class-zoho-inventory-items-api.php](file:///home/sudhar/Local%20Sites/linkzoho/app/public/wp-content/plugins/LZ-SKU-Sync/includes/API/class-zoho-inventory-items-api.php))
   - Fetch all items (with pagination)
   - Fetch single item by ID
   - Product type filtering

7. **Zoho_Inventory_Locations_API** ([class-zoho-inventory-locations-api.php](file:///home/sudhar/Local%20Sites/linkzoho/app/public/wp-content/plugins/LZ-SKU-Sync/includes/API/class-zoho-inventory-locations-api.php))
   - Warehouse/location listing
   - Warehouse details

8. **Zoho_Inventory_Image_Download** ([class-zoho-inventory-image-download.php](file:///home/sudhar/Local%20Sites/linkzoho/app/public/wp-content/plugins/LZ-SKU-Sync/includes/API/class-zoho-inventory-image-download.php))
   - Download images ZIP from Zoho
   - Retry logic with exponential backoff
   - Rate limit handling

9. **Zoho_Inventory_API_Webhook** ([class-zoho-inventory-api-webhook.php](file:///home/sudhar/Local%20Sites/linkzoho/app/public/wp-content/plugins/LZ-SKU-Sync/includes/API/class-zoho-inventory-api-webhook.php))
   - Create/delete webhooks
   - Create/delete workflow rules
   - Activate/deactivate workflow rules

10. **Zoho_Inventory_API_Webhook_Receive** ([class-zoho-inventory-api-webhook-receive.php](file:///home/sudhar/Local%20Sites/linkzoho/app/public/wp-content/plugins/LZ-SKU-Sync/includes/API/class-zoho-inventory-api-webhook-receive.php))
    - REST API endpoint registration
    - Webhook signature verification
    - Webhook payload processing

#### Sync Classes

11. **Zoho_Inventory_Overview** ([class-zoho-inventory-overview.php](file:///home/sudhar/Local%20Sites/linkzoho/app/public/wp-content/plugins/LZ-SKU-Sync/includes/API/class-zoho-inventory-overview.php))
    - Fetch and summarize Zoho inventory
    - Fetch and summarize WooCommerce products
    - SKU comparison and matching
    - Annotate products with create/update actions

12. **Zoho_Inventory_Product_Sync** ([class-zoho-inventory-product-sync.php](file:///home/sudhar/Local%20Sites/linkzoho/app/public/wp-content/plugins/LZ-SKU-Sync/includes/API/class-zoho-inventory-product-sync.php))
    - Main synchronization logic
    - Batch processing with Action Scheduler
    - Simple product create/update
    - Variable product create/update
    - Variation handling
    - Image attachment
    - Progress tracking
    - Error handling and recovery

### Action Scheduler Integration

The plugin leverages WooCommerce's Action Scheduler for:

#### Custom Action Hooks:

- `wzi_process_sync_batch` - Process one batch of products
- `wzi_process_webhook_sync` - Process single webhook-triggered sync
- `wzi_upload_single_image` - Upload one image to WordPress
- `wzi_process_image_metadata` - Generate WebP and thumbnails
- `wzi_process_single_variation` - Create/update one variation
- `wzi_assign_parent_images` - Assign images to variable product parent

#### Custom Action Groups:

- `wzi_manual_sync_group` - Manual sync jobs
- `wzi_webhook_sync_group` - Webhook sync jobs
- `wzi_variation_processing_group` - Variation processing chain
- `wzi_image_processing_group` - Image processing jobs

#### Benefits:

- **Reliability**: Jobs survive server restarts
- **Queueing**: Prevents race conditions and duplicate processing
- **Logging**: Full execution logs for debugging
- **Retry**: Failed actions can be retried
- **Performance**: Offloads long-running tasks from HTTP requests

### Security Measures

1. **Access Token Encryption**: AES-128-CBC encryption with HMAC SHA256
2. **Nonce Verification**: WordPress nonce for all AJAX requests
3. **Capability Checks**: Only administrators can access settings
4. **SQL Prepared Statements**: Prevents SQL injection
5. **Input Sanitization**: All user inputs sanitized
6. **Output Escaping**: All outputs escaped
7. **Webhook Signature**: HMAC verification on all webhook requests
8. **License Validation**: Server-side validation before sync operations

---

## FAQ

### General Questions

**Q: Does this plugin sync from WooCommerce to Zoho?**  
A: No, this is a **one-way sync from Zoho Inventory to WooCommerce**. Zoho is the source of truth.

**Q: Can I sync products from multiple warehouses?**  
A: Yes, but only one warehouse at a time. You can change the selected warehouse in settings.

**Q: Will existing WooCommerce products be overwritten?**  
A: Only if the SKU matches. Products are matched by SKU, and only enabled fields will be updated.

**Q: Can I prevent creating new products?**  
A: Yes, disable "Create Missing Products" and "Create Missing Variations" in sync preferences.

**Q: How often do webhooks sync?**  
A: Instantly! When an item is created/updated in Zoho, the webhook triggers within seconds.

### Pricing & Licensing

**Q: Is this a one-time purchase or subscription?**  
A: Contact [linkzoho.com](https://linkzoho.com) for current pricing and licensing model.

**Q: Can I use one license on multiple sites?**  
A: Each site requires its own license activation. Contact sales for multi-site licensing.

**Q: What happens if my license expires?**  
A: The plugin will stop syncing. You can renew your license to continue using the plugin.

### Technical Questions

**Q: Does this work with WooCommerce High-Performance Order Storage (HPOS)?**  
A: Yes, the plugin declares HPOS compatibility as of version 1.1.2.

**Q: Can I customize which product fields are synced?**  
A: Yes, use the field toggles in Sync Preferences for granular control.

**Q: Does it support custom product types (e.g., subscriptions)?**  
A: The plugin handles Simple and Variable product types. Custom types may need customization.

**Q: Can I sync categories?**  
A: Currently, no. Only product data and attributes are synced.

**Q: Does it sync order data?**  
A: No, this plugin is focused on product/inventory synchronization only.

### Troubleshooting

**Q: Why are some products not syncing?**  
A: Check that:
- Products have SKUs in Zoho
- Selected warehouse has stock for the product
- Sync preferences allow creating new products (if applicable)
- No errors in Action Scheduler logs

**Q: Why are images taking so long?**  
A: Image downloads are rate-limited (2 seconds between requests) to comply with Zoho's API limits. Large catalogs will take time.

**Q: Can I run syncs via WP-CLI or cron?**  
A: Manual syncs are triggered via admin UI. Use webhook mode for continuous syncing.

---

## Support & Updates

### Getting Help

- **Documentation**: This comprehensive guide
- **Email Support**: sales@linkzoho.com, support@krenovate.com
- **Action Scheduler Logs**: **Tools > Action Scheduler** for detailed execution logs
- **Debug Logs**: Enable `WP_DEBUG_LOG` and check `wp-content/debug.log`

### Updates

The plugin includes automatic updates via GitHub:

1. When a new version is released on GitHub
2. Plugin checks for updates daily
3. Notification appears in WordPress admin
4. Click **Update Now** to install
5. Requires active license for update access

### Changelog

#### Version 1.3.0 (Current)
- Enhanced sync logging
- Improved image deduplication
- Variable product image handling
- Performance optimizations

#### Version 1.2.7
- Added inventory management features
- Plugin deactivation issue fixes

#### Version 1.1.2
- HPOS compatibility declaration

#### Version 1.1.0
- Image optimization (WebP generation)
- Business location support
- Comprehensive error handling

#### Version 1.0.0
- Initial release
- Manual and webhook sync modes
- Simple and variable product support

---

## Conclusion

**LZ SKU Sync** is a powerful, enterprise-grade solution for automating product synchronization between Zoho Inventory and WooCommerce. With its dual sync modes, comprehensive product support, intelligent image management, and robust error handling, it eliminates manual data entry and ensures your online store always reflects accurate inventory data.

For additional support or customization requests, please contact us at [https://linkzoho.com](https://linkzoho.com).

---

**Thank you for choosing LZ SKU Sync!**

*Documentation version 1.0 - Last updated: December 2024*
