# Troubleshooting

Solutions to common issues you may encounter with LZ SKU Sync.

---

## Common Issues

### 1. Authentication Errors

**Symptoms:**

- "Invalid authentication" or "Token expired" errors
- "Failed to connect to Zoho"
- API calls return 401 Unauthorized

**Solutions:**

**A. Auto-Refresh**

- Tokens auto-refresh every 58 minutes
- Wait a few minutes and try again

**B. Reconnect**

1. Go to **LZ SKU Sync > Settings > Authentication**
2. Click **Disconnect**
3. Click **Connect with Zoho** again
4. Re-authorize the connection

**C. Verify Organization Access**

- Ensure you still have access to the organization in Zoho
- Check that Zoho Inventory is active
- Verify you have admin permissions

**D. Check API Rate Limits**

- Zoho limits: 100 requests per minute
- Plugin respects limits automatically
- If exceeded, sync will pause and retry

---

### 2. Images Not Downloading

**Symptoms:**

- Products sync successfully
- Images missing from product pages
- No featured image set

**Solutions:**

**A. Verify Images in Zoho**

1. Log into Zoho Inventory
2. Go to Items > [Product Name]
3. Check "Attachments" tab
4. Ensure images are uploaded

**B. Check Image Sync Settings**

1. Go to **LZ SKU Sync > Sync Status**
2. Verify "Product Image" is enabled
3. Verify "Gallery Images" is enabled (if wanted)
4. Save preferences and re-sync

**C. Check PHP ZIP Extension**
Run in terminal or phpinfo():
```bash
php -m | grep zip
```

If not installed, contact your hosting provider.

**D. Verify Upload Directory**
Check that `wp-content/uploads/` is writable (755 permissions).

**E. Review Action Scheduler**

1. Go to **LZ SKU Sync > Sync Log**
2. Check for failed image download actions
3. Review error messages

**F. Check API Rate Limits**

- Images are rate-limited (2 seconds between requests)
- For 100 products with images: ~3-5 minutes minimum
- This is normal - be patient!

---

### 3. Webhooks Not Receiving

**Symptoms:**

- Changes in Zoho don't appear in WooCommerce
- Webhook status shows inactive
- No recent items in sync log

**Solutions:**

**A. Verify Webhook Settings**

1. **Sync Frequency**: Set to "Always (Real-time webhook)"
2. **Status Indicator**: Should be green (active)
3. **License**: Must be active

**B. Test Endpoint**
```bash
curl -X POST https://yoursite.com/wp-json/lz-sku-sync/v1/webhook
```
Should return a response (even if 403 due to missing signature).

**C. Check Webhook Logs in Zoho**

1. Log into Zoho Inventory
2. Go to **Settings > Webhooks**
3. Find LZ SKU Sync webhook
4. Check delivery logs for errors

**D. Review WordPress Logs**

Enable debug logging in `wp-config.php`:
```php
define('WP_DEBUG', true);
define('WP_DEBUG_LOG', true);
define('WP_DEBUG_DISPLAY', false);
```

Check `wp-content/debug.log` for webhook errors.

**E. Verify License**

- Ensure license is active (not expired)
- License key is used for HMAC signature
- Check at **LZ SKU Sync > Settings**

---

### 4. Variations Not Creating

**Symptoms:**

- Variable products sync
- Variations missing from WooCommerce
- Only parent product created

**Solutions:**

**A. Enable Variation Creation**

1. Go to **LZ SKU Sync > Sync Status**
2. Enable **"Create Missing Variations"**
3. Save preferences
4. Re-sync the product

**B. Verify SKUs in Zoho**

- Each variation must have a unique SKU
- Check in Zoho Inventory > Items > Product Group
- Ensure all variants have SKU assigned

**C. Check Attribute Fields**

- Zoho must have `attribute_name1`, `attribute_name2`, etc.
- Zoho must have `attribute_option_name1`, `attribute_option_name2`, etc.
- These fields define WooCommerce attributes

**D. Review Action Scheduler**

1. Go to **LZ SKU Sync > Sync Log**
2. Check for failed variation actions
3. Review error messages

---

### 5. Duplicate Products Created

**Symptoms:**

- Multiple products with same name
- SKUs don't match
- Products created instead of updated

**Solutions:**

**A. Fix SKU Matching**

- SKUs must match **exactly** (case-sensitive)
- Check Zoho SKU vs WooCommerce SKU
- Fix typos, extra spaces, or case differences

**B. Disable Product Creation**

If you only want updates, not new products:

1. Disable **"Create Missing Products"**
2. Disable **"Create Missing Variations"**
3. Save preferences

**C. Delete Duplicates**

1. Go to **Products** in WooCommerce
2. Find duplicate products
3. Delete the incorrect ones
4. Re-sync with correct SKU matching

---

### 6. Stock Quantities Incorrect

**Symptoms:**

- WooCommerce stock doesn't match Zoho
- Always showing 0 stock
- Stock not updating

**Solutions:**

**A. Verify Warehouse Selection**

1. Go to **LZ SKU Sync > Sync Status**
2. Check **Sync Warehouse** setting
3. Ensure correct warehouse is selected
4. Save and re-sync

**B. Enable Stock Quantity Sync**

1. Check **"Available Stock Quantity"** is enabled in field settings
2. Save preferences
3. Run manual sync

**C. Check Item Status in Zoho**

- Ensure item is "Active" (not archived)
- Verify item is assigned to selected warehouse
- Check actual stock in Zoho matches expectations

---

### 7. License Activation Failures

**Symptoms:**

- "Invalid license key" error
- "Order ID not found" error
- Activation fails

**Solutions:**

**A. Verify License Details**

- **Order ID**: From your purchase confirmation
- **License Key**: Check for typos, extra spaces
- **Order Status**: Must be "Processing" or "Completed"

**B. Check Order at LinkZoho**

1. Log into [app.linkzoho.com](https://app.linkzoho.com)
2. Go to **Orders**
3. Verify order status
4. Copy license key directly (don't retype)

**C. Site Activation Limit**

- Each license has activation limit
- Check if you've exceeded allowed sites
- Deactivate from old sites if needed
- Contact support to increase limit

**D. Contact Support**
If issue persists:

- 📧 Email: sales@linkzoho.com, support@krenovate.com
- Include Order ID (not license key)
- Include error message screenshot

---

## Debug Mode

Enable detailed logging for troubleshooting:

### Enable WordPress Debug Logging

Add to `wp-config.php`:
```php
define('WP_DEBUG', true);
define('WP_DEBUG_LOG', true);
define('WP_DEBUG_DISPLAY', false);
```

### What Gets Logged

- API request/response details
- Action Scheduler execution logs
- Image processing errors
- Webhook signature verification
- Token refresh attempts
- Error stack traces

### View Logs

**WordPress Debug Log:**

- Location: `wp-content/debug.log`
- Download via FTP or File Manager
- Search for "LZ SKU Sync" or "Zoho"

**Action Scheduler Logs:**

- Navigate to **LZ SKU Sync > Sync Log**
- Click any action to view execution log
- Check "Failed" tab for error details

---

## Server Requirements Check

Verify your server meets all requirements:

### PHP Version
```bash
php -v
```
Required: 7.4 or higher

### PHP Extensions
```bash
php -m | grep -E 'curl|json|zip|gd|imagick|openssl'
```
All should be listed.

### Memory Limit
Check in `php.ini` or WordPress:
```php
echo WP_MEMORY_LIMIT; // Should be 256M+
```

### Max Execution Time
```php
echo ini_get('max_execution_time'); // Should be 300+
```

---

## FAQ

### Q: Can I sync from WooCommerce to Zoho?
A: No, this is a **one-way sync** from Zoho to WooCommerce only.

### Q: Can I sync from multiple warehouses?
A: Only one warehouse at a time. You can switch warehouses in settings.

### Q: Will existing products be overwritten?
A: Only if SKU matches. Enabled fields will be updated, others unchanged.

### Q: How do I stop creating new products?
A: Disable "Create Missing Products" in sync preferences.

### Q: Can I sync just inventory, no other changes?
A: Yes! Enable only "Stock Quantity" in field settings, disable all others.

### Q: Why are images taking so long?
A: Rate limiting (2 seconds per product) prevents API throttling. This is intentional.

### Q: Can I run syncs via cron or WP-CLI?
A: Manual syncs are UI-triggered. Use webhook mode for automation.

### Q: Does it support product categories?
A: Not currently. Only product data, attributes, and inventory.

---

## Getting Help

### Before Contacting Support

1. ✅ Check this troubleshooting guide
2. ✅ Review Action Scheduler logs
3. ✅ Enable debug mode and check logs
4. ✅ Verify authentication is connected

### Contacting Support

**Email:** sales@linkzoho.com, support@krenovate.com

**Include:**

- WordPress version
- PHP version
- WooCommerce version
- Plugin version
- Clear description of issue
- Screenshots of error messages
- Relevant log excerpts (from debug.log or Action Scheduler)
- Steps to reproduce

**Don't Include:**

- License key (only Order ID if needed)
- API credentials
- Full database dumps

---

## Additional Resources

- [📖 Full Documentation](index.md)
- [⚙️ Product Settings](product-settings.md)
- [📊 Monitoring Guide](monitoring.md)
- [🔗 Webhook Setup](webhooks.md)
- [🌐 LinkZoho Website](https://linkzoho.com)

---

*Still need help? We're here to support you at sales@linkzoho.com and support@krenovate.com*
