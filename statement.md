import os
from datetime import datetime

def safe_int(prompt):
    while True:
        try:
            v = int(input(prompt))
            if v < 0:
                print("Please enter a non-negative integer.")
                continue
            return v
        except ValueError:
            print("Invalid input. Please enter an integer.")

def safe_float(prompt):
    while True:
        try:
            v = float(input(prompt))
            if v < 0:
                print("Please enter a non-negative number.")
                continue
            return v
        except ValueError:
            print("Invalid input. Please enter a number (e.g. 49.99).")

def format_currency(x):
    return f"₹ {x:,.2f}"

def main():
    print("===================================")
    print("      SHOP MANAGEMENT SYSTEM       ")
    print("===================================")

    # Step 1: Number of items
    n = safe_int("Enter number of items: ")
    if n == 0:
        print("No items entered. Exiting.")
        return

    items = []
    quantities = []
    prices = []
    totals = []

    # Step 2: Input for each item with validation
    for i in range(n):
        print(f"\nEnter details for Item {i+1}:")
        name = input("Item name: ").strip()
        if not name:
            name = f"Item_{i+1}"
        qty = safe_int("Quantity: ")
        price = safe_float("Price per item: ")
        total = qty * price

        items.append(name)
        quantities.append(qty)
        prices.append(price)
        totals.append(total)

    # Step 3: Display final bill
    print("\n-------------------------------------")
    print("            FINAL BILL               ")
    print("-------------------------------------")
    print(f"{'Item':<20}{'Qty':<8}{'Price':<12}{'Total':<12}")
    print("-------------------------------------")

    grand_total = 0.0
    for i in range(n):
        print(f"{items[i]:<20}{quantities[i]:<8}{prices[i]:<12.2f}{totals[i]:<12.2f}")
        grand_total += totals[i]

    print("-------------------------------------")
    print(f"{'Grand Total':<40}{grand_total:>10.2f}")
    print("-------------------------------------")
    print("Thank you for shopping!")

    # Step 4: Save bill in 'bills' folder with timestamp and date/time in content
    folder = "bills"
    try:
        os.makedirs(folder, exist_ok=True)
    except OSError as e:
        print(f"Error creating folder '{folder}': {e}")
        return

    timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")
    filename = f"bill_{timestamp}.txt"
    filepath = os.path.join(folder, filename)

    try:
        with open(filepath, "w", encoding="utf-8") as f:
            f.write("===================================\n")
            f.write("        SHOP MANAGEMENT SYSTEM     \n")
            f.write("===================================\n\n")
            f.write(f"Date: {datetime.now().strftime('%Y-%m-%d %H:%M:%S')}\n\n")
            f.write(f"{'Item':<20}{'Qty':<8}{'Price':<12}{'Total':<12}\n")
            f.write("-----------------------------------\n")
            for i in range(n):
                f.write(f"{items[i]:<20}{quantities[i]:<8}{prices[i]:<12.2f}{totals[i]:<12.2f}\n")
            f.write("-----------------------------------\n")
            f.write(f"{'Grand Total':<40}{grand_total:>10.2f}\n")
            f.write("-----------------------------------\n")
            f.write("Thank you for shopping!\n")
        print(f"\n✅ Bill saved successfully inside '{folder}' as '{filename}'.")
        print(f"Location: {os.path.abspath(filepath)}")
    except OSError as e:
        print(f"Error saving bill to '{filepath}': {e}")

if __name__ == "__main__":
    main()
