##  **Access the Inventory Setup**
Go to WP Admin Menu> LinkZoho > Settings > Inventory Tab

## **Configure Warehouse Mapping**
1. Under Branch/Location/Warehouses, select the Branch/Locations/Warehouses you want to map for Woocommerce orders
    - Example: Head Office (Location), Main Warehouse (Warehouse linked to location)

2. Inventory levels of Items listed in mapped locations will be automatically deducted based on relevant Woocommerce orders

## **Set SKU Matching Rules**

### **Option A: Strict SKU Matching (Recommended)**

Ideal for businesses requiring precise inventory tracking.

- Check Set SKU as Primary ID for Inventory Deductions.
    - Behavior:
        - SO and Invoices only generate if all SKUs in the WooCommerce order exist in Zoho Items.
        - If any SKU is missing or not found in Zoho items or Woocommerce, the order is skipped, and an order note is added to the specific Woo order (e.g., "SKU Not Found – SO and Invoice Not Created").

    - Use Case: Ensures 100% inventory accuracy but requires rigorous SKU alignment.

### **Option B: Flexible Matching (SKU or Product Name)**

Ideal for businesses with inconsistent SKU systems.

- Uncheck Set SKU as Primary ID for Inventory Deductions.

    - Behavior:
        - Uses Product Name or SKU for inventory deductions.
        - SO and Invoices generate even if SKUs/names are missing in Zoho Items or woocommerce

    - Use Case: Useful for testing or transitional phases but risks inventory inaccuracies.

## **Handling SKU Mismatches**

- If SKU Not Found (Strict Mode):
    - The plugin skips SO/Invoice creation in Zoho Books
    - Adds an order note in WooCommerce (e.g., "LinkZoho: SKU Not Found – SO and Invoice Not Created").