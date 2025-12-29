# Sync Log

Comprehensive logging and monitoring of all synchronization activities with advanced search, filtering, and sorting capabilities.

---

## Overview

The **Sync Log** provides a detailed 30-day history of all synchronization activities in LZ SKU Sync. It tracks every action performed by the plugin, including product creation, updates, image processing, and webhook events, along with their execution status and detailed logs.

---

## Accessing Sync Log

Navigate to **LZ SKU Sync > Sync Log** in your WordPress admin panel.

The Sync Log interface displays all sync activities from the last 30 days with powerful search and filtering tools.

---

## Understanding the Sync Log Table

### Table Columns

| Column | Description |
|--------|-------------|
| **S.No** | Sequential number for the current page view |
| **Action ID** | Unique identifier from WordPress Action Scheduler |
| **Action Type** | Type of sync operation performed |
| **SKU / Group ID** | Product SKU or Group ID from Zoho |
| **Product Name** | Name of the product from Zoho Inventory |
| **Product Type** | Simple, Group, or Variation |
| **Date & Time** | When the action was queued/executed |
| **Action Status** | Current status of the action (see below) |
| **Action Logs** | Detailed execution logs from Action Scheduler |

### Action Status Badges

The **Action Status** column displays color-coded badges indicating the current state of each action:

#### 🟡 Pending
- **Color**: Amber/Yellow badge
- **Meaning**: Action is queued but not yet started
- **Next Steps**: Will be processed automatically by WordPress cron

#### 🔵 In-Progress
- **Color**: Blue badge
- **Meaning**: Action is currently being executed
- **Note**: May appear briefly during active sync

#### 🟢 Complete
- **Color**: Green badge
- **Meaning**: Action successfully completed
- **Result**: Product/image successfully synced

#### 🔴 Failed
- **Color**: Red badge
- **Meaning**: Action encountered an error
- **Action Required**: Check Action Logs for error details

#### ⚫ Canceled
- **Color**: Gray badge
- **Meaning**: Action was canceled (usually by user)
- **Note**: Will not be retried

---

## Common Action Types

### Product Actions
- **Created** - New simple product created in WooCommerce
- **Updated** - Existing simple product updated
- **Product Sync** - General product synchronization

### Variation Actions
- **Created Variation** - New product variation created
- **Updated Variation** - Existing variation updated
- **Variation Processing** - Background variation processing

### Image Actions
- **Image Upload** - Product images downloaded and uploaded
- **Image Optimization** - Thumbnail generation completed
- **Parent Images Assigned** - Gallery images set for variable products

### Webhook Actions
- **Webhook Sync** - Real-time webhook-triggered sync
- **Webhook Received** - Webhook POST received from Zoho

---

## Search Functionality

The search box allows you to quickly find specific sync activities.

### What Can You Search?
- Action ID
- Action Type
- SKU or Group ID
- Product Name
- Product Type
- Action Status

### How to Search

1. Type your search term in the **Search** box
2. Results filter automatically as you type (150ms debounce)
3. Search is case-insensitive
4. Matches partial text across all searchable columns

**Examples:**
- Search `"12345"` - Finds Action ID 12345 or SKUs containing 12345
- Search `"Image Upload"` - Shows all image upload actions
- Search `"Complete"` - Shows all completed actions
- Search `"PROD-001"` - Finds product with SKU PROD-001

---

## Filtering Options

### Action Status Filter

Filter sync activities by their execution status:

1. Click **Action Status** dropdown
2. Select status:
   - **All** - Show all statuses
   - **Pending** - Queued actions
   - **In-progress** - Currently executing
   - **Complete** - Successfully finished
   - **Failed** - Encountered errors
   - **Canceled** - User-canceled actions
3. Table updates automatically

### Product Type Filter

Filter by the type of product:

1. Click **Product Type** dropdown
2. Select from available types:
   - **All** - Show all types
   - **Simple** - Simple products only
   - **Group** - Product groups (variable products)
   - **Variation** - Individual variations
3. Dropdown is auto-populated from your actual data

### Date Range Filters

Filter actions within a specific date range:

**From Date:**

1. Click **From date** field
2. Select start date from calendar picker
3. Only actions on or after this date will show

**To Date:**

1. Click **To date** field
2. Select end date from calendar picker
3. Only actions on or before this date will show

**Notes:**

- From date starts at 00:00:00 (midnight)
- To date ends at 23:59:59 (end of day)
- Both dates are optional
- Use only "From" to see everything since a date
- Use only "To" to see everything before a date

### Reset Filters

Click the **Reset** button to clear all filters and search:

- Clears search text
- Resets all dropdowns to "All"
- Clears date range
- Returns to page 1

---

## Sorting

The Sync Log supports sorting on two columns: **Action ID** and **Date & Time**.

### How to Sort

1. Click on a sortable column header
   - **Action ID** (sortable)
   - **Date & Time** (sortable)
2. Click once for **descending** order (newest/highest first)
3. Click again to toggle to **ascending** order
4. Visual indicators show current sort:
   - **▲** = Ascending
   - **▼** = Descending

### Sort Behavior

**Default Sort:**

- First click sorts descending (newest/highest first)
- Best for finding recent actions or high Action IDs

**Action ID Sorting:**

- Numeric sort (1, 2, 10, 100, not 1, 10, 100, 2)
- Descending shows newest actions first
- Ascending shows oldest actions first

**Date & Time Sorting:**

- Chronological sort by timestamp
- Descending shows most recent first
- Ascending shows oldest first

!!! note
    Sorting reorders all filtered results before pagination. The order you see on each page reflects the global sort order.

---

## Pagination

The Sync Log displays **10 items per page** for optimal performance and readability.

### Pagination Controls

Located at the bottom of the table:

- **«** - Jump to first page
- **‹** - Previous page
- **Page Numbers** - Direct page navigation (up to 5 pages shown)
- **›** - Next page
- **»** - Jump to last page

### Page Statistics

Below the pagination controls, you'll see:
```
Showing 1–10 of 150
```

This indicates:

- **1–10** - Items currently displayed on this page
- **150** - Total items matching current filters

### Navigation Tips

- Click page numbers to jump directly to a page
- Use arrow buttons for sequential navigation
- First/Last page buttons are disabled when already on that page
- When filtering reduces total items, pagination adjusts automatically

---

## Action Logs Column

The **Action Logs** column provides detailed execution information for each action.

### What's Logged

- Timestamp of each log entry
- Execution progress messages
- API request/response details
- Error messages and stack traces
- Completion confirmations

### Log Entry Format

```
2025-12-29 15:30:45 - Product created successfully (ID: 12345)
2025-12-29 15:30:46 - Images downloaded and attached
2025-12-29 15:30:47 - Action completed
```

### Scrollable Logs

- Logs box has maximum height of 200px
- Scroll vertically to view all log entries
- Long error messages are fully visible (word-wrapped)

### No Logs Available

If you see "No logs available", it means:

- Action hasn't started yet (Pending status)
- Logs were pruned after 30 days
- Action Scheduler couldn't retrieve logs

---

## Using Sync Log for Troubleshooting

### Finding Failed Actions

1. Set **Action Status** filter to **Failed**
2. Review the **Action Logs** column for error messages
3. Note the **Action Type** and **Product Name**
4. Check common issues in [Troubleshooting Guide](troubleshooting.md)

### Tracking Specific Products

1. Enter product **SKU** in search box
2. Or enter **Product Name** in search box
3. Review all actions for that product
4. Check for any failed or pending actions

### Monitoring Webhook Activity

1. Search for **"Webhook"** in search box
2. Or filter by recent dates to see latest webhooks
3. Verify webhook status is "Complete"
4. Check Action Logs for any errors

### Investigating Slow Syncs

1. Filter by **Date Range** for your sync period
2. Look for many **Pending** or **Failed** actions
3. Check **Action Logs** for timeout errors
4. Review server performance during that time

---

## Best Practices

### Regular Monitoring

- ✅ Review Sync Log weekly
- ✅ Check for failed actions and address them
- ✅ Monitor webhook activity if using real-time sync
- ✅ Clear old failed actions if resolved

### After Manual Sync

- ✅ Filter to recent date range
- ✅ Check for any failed actions
- ✅ Verify all expected products show as "Complete"
- ✅ Review Action Logs for any warnings

### Troubleshooting Workflow

1. Identify the issue (missing product, failed webhook, etc.)
2. Search Sync Log for related product/SKU
3. Check Action Status and Action Logs
4. Note any error messages
5. Consult [Troubleshooting Guide](troubleshooting.md) with error details

### Data Retention

- Logs are retained for **30 days only**
- Action Scheduler automatically prunes old logs
- Export important error messages for records
- Take screenshots of critical failures

---

## Common Scenarios

### Scenario 1: Product Not Syncing

**Steps:**

1. Search for product **SKU** in Sync Log
2. If no results: Product may not be in Zoho or warehouse
3. If found with "Failed" status: Check Action Logs for errors
4. If found with "Pending": Check if Action Scheduler is running

### Scenario 2: Images Missing

**Steps:**

1. Search for **"Image Upload"** or **"Image Optimization"**
2. Filter by product SKU or name
3. Check if image actions completed successfully
4. Review Action Logs for download errors or API limits

### Scenario 3: Webhook Not Processing

**Steps:**

1. Search for **"Webhook"** near the time of Zoho change
2. Check if webhook was received (should show in log)
3. If missing: Verify webhook status is active
4. If failed: Check Action Logs for signature or API errors

### Scenario 4: Variation Not Created

**Steps:**

1. Search for **"Variation"** or parent product SKU
2. Check if "Created Variation" actions exist
3. If failed: Review Action Logs for SKU or attribute errors
4. Verify "Create Missing Variations" is enabled

---

## Technical Details

### Action Scheduler Integration

The Sync Log displays actions from WordPress Action Scheduler:

- **Action ID**: Unique identifier in `actionscheduler_actions` table
- **Status**: Real-time status from Action Scheduler
- **Logs**: Retrieved from `actionscheduler_logs` table
- **Auto-cleanup**: Actions older than 30 days are automatically removed

### Performance Optimization

- **Client-side filtering**: Instant filter/search without server requests
- **Pagination**: Only 10 rows render at a time for fast loading
- **Debounced search**: 150ms delay prevents excessive re-filtering
- **In-memory model**: Filtering operates on data model, not DOM text

### Sorting Algorithm

- **Numeric columns** (Action ID): Integer comparison
- **Date columns** (Date & Time): Unix timestamp comparison
- **Stable sort**: Maintains relative order of equal elements

---

## Keyboard Shortcuts

While there are no built-in keyboard shortcuts, you can use browser features:

- **Ctrl+F** / **Cmd+F** - Browser find (searches visible page only)
- **Tab** - Navigate between filters
- **Enter** - Apply date picker selection
- **Esc** - Close date picker

---

## Limitations

### 30-Day Window
- Only actions from last 30 days are shown
- Older actions are automatically pruned
- Export or screenshot critical errors for long-term records

### Action Scheduler Dependency
- Relies on WordPress Action Scheduler
- If Action Scheduler is disabled, logs won't appear
- Requires Action Scheduler database tables

### No Export Function
- Currently no CSV export feature
- Use browser print or screenshots to save records
- Copy-paste log entries for documentation

---

## Next Steps

- [**Monitor Sync Status →**](monitoring.md) - Real-time sync progress
- [**Troubleshooting Guide →**](troubleshooting.md) - Fix sync issues
- [**Webhook Integration →**](webhooks.md) - Real-time sync setup

---

## Support

Questions about Sync Log?

- 📧 Email: sales@linkzoho.com, support@krenovate.com
- Include Action ID and log screenshots when reporting issues
- Note the Date & Time of the problematic action
