# Grocery-Inventory-Manager-for-FreshMart-Retail.ipynb
# Grocery Inventory Manager for FreshMart Retail

import pandas as pd

# Sample Inventory Data
data = {
    'ItemID': [101, 102, 103, 104],
    'ItemName': ['Milk', 'Bread', 'Eggs', 'Rice'],
    'Category': ['Dairy', 'Bakery', 'Dairy', 'Grains'],
    'Stock': [25, 15, 50, 100],
    'ReorderLevel': [10, 5, 20, 30]
}

# Create DataFrame
inventory_df = pd.DataFrame(data)

# Display Inventory
def display_inventory():
    print("\nCurrent Inventory Status:")
    display(inventory_df)

# Update stock after purchase
def update_stock(item_id, quantity_sold):
    global inventory_df
    if item_id in inventory_df['ItemID'].values:
        idx = inventory_df[inventory_df['ItemID'] == item_id].index[0]
        if inventory_df.loc[idx, 'Stock'] >= quantity_sold:
            inventory_df.loc[idx, 'Stock'] -= quantity_sold
            print(f"\n✅ Sold {quantity_sold} units of {inventory_df.loc[idx, 'ItemName']}")
        else:
            print(f"\n❌ Not enough stock to sell {quantity_sold} units of {inventory_df.loc[idx, 'ItemName']}")
    else:
        print("\n❌ Item not found!")

# Check for low stock or overstock
def check_stock_status():
    print("\n🔍 Stock Alerts:")
    for idx, row in inventory_df.iterrows():
        if row['Stock'] <= row['ReorderLevel']:
            print(f"⚠️ LOW STOCK ALERT: {row['ItemName']} (Stock: {row['Stock']})")
        elif row['Stock'] > 3 * row['ReorderLevel']:
            print(f"📦 EXCESS STOCK ALERT: {row['ItemName']} (Stock: {row['Stock']})")

# Add new item
def add_new_item(item_id, name, category, stock, reorder_level):
    global inventory_df
    if item_id in inventory_df['ItemID'].values:
        print("\n❌ Item ID already exists.")
    else:
        new_row = pd.DataFrame([[item_id, name, category, stock, reorder_level]], columns=inventory_df.columns)
        inventory_df = pd.concat([inventory_df, new_row], ignore_index=True)
        print(f"\n✅ Item '{name}' added successfully.")

# Example Usage
display_inventory()
update_stock(102, 5)         # Selling 5 units of Bread
add_new_item(105, 'Sugar', 'Grains', 20, 8)
check_stock_status()
display_inventory()
