# Rithik-Internship-YBI
import pandas as pd

# Create an initial inventory as a DataFrame
inventory = pd.DataFrame({
    'Item': ['Rice', 'Dal', 'Spices', 'Cooking Oil'],
    'Quantity': [50, 30, 20, 15],  # Quantities in stock
    'Price': [40, 120, 200, 150]  # Prices per kg/liter
})

# Function to update inventory after a sale
def update_inventory(item_name, quantity_sold):
    global inventory
    if item_name in inventory['Item'].values:
        item_index = inventory[inventory['Item'] == item_name].index[0]
        current_quantity = inventory.at[item_index, 'Quantity']
        if current_quantity >= quantity_sold:
            inventory.at[item_index, 'Quantity'] -= quantity_sold
            print(f"Updated {item_name}: {inventory.at[item_index, 'Quantity']} kg/l remaining.")
        else:
            print(f"Not enough stock of {item_name}. Only {current_quantity} kg/l available.")
    else:
        print(f"{item_name} is not in inventory.")

# Function to generate a bill
def generate_bill(items, quantities):
    global inventory
    total_amount = 0
    print("\n--- Bill ---")
    for item, quantity in zip(items, quantities):
        if item in inventory['Item'].values:
            item_index = inventory[inventory['Item'] == item].index[0]
            price_per_unit = inventory.at[item_index, 'Price']
            cost = price_per_unit * quantity
            total_amount += cost
            print(f"{item}: {quantity} x {price_per_unit} = {cost}")
            update_inventory(item, quantity)
        else:
            print(f"{item} is not available in the inventory.")
    print(f"Total Amount: {total_amount}\n")
    return total_amount

# Function to check for low stock
def check_low_stock(threshold=20):
    global inventory
    print("\n--- Low Stock Alert ---")
    low_stock_items = inventory[inventory['Quantity'] < threshold]
    if not low_stock_items.empty:
        for _, row in low_stock_items.iterrows():
            print(f"{row['Item']} is below the threshold ({row['Quantity']} kg/l left). Please reorder.")
    else:
        print("All items are sufficiently stocked.")

# Adding a new item to inventory
def add_new_item(item_name, quantity, price):
    global inventory
    if item_name not in inventory['Item'].values:
        inventory = inventory.append({'Item': item_name, 'Quantity': quantity, 'Price': price}, ignore_index=True)
        print(f"Added {item_name} to inventory.")
    else:
        print(f"{item_name} already exists in the inventory.")

# Example usage
print("Initial Inventory:")
print(inventory)

# Generate a bill for a customer
items_to_buy = ['Rice', 'Dal']
quantities_to_buy = [10, 5]
generate_bill(items_to_buy, quantities_to_buy)

# Check for low stock
check_low_stock()

# Add a new item
add_new_item('Sugar', 25, 50)

# View updated inventory
print("\nUpdated Inventory:")
print(inventory)
