# week 2 Module Challenge python-challenge-1
NOTE: The Starter Code provided in the challenge is deficient. While everything works fine per the instructions set, it is deficient when the customer says “yes” to to whether they would like to keep ordering. When I was running it to ensure everything works accordingly, I tried to experience all the various possibilities for this to work in a real restaurant environment. But I encountered the following issue. When I, the presumed customer, input “y” or “Y” meaning “yes,” to keep ordering, it gets stuck and keeps asking me if I would like to keep ordering instead of going back to “place_order = True” to ask me “("From which menu would you like to order? ").” I am enclosing a paaliative code below for your review. It shows how this insufficiency can be fixed to return the customer to “place_order” and run that whole loop (while place_order:) again instead of continuously asking me “Would you like to keep ordering? (Y)es or (N)o:-->?” In the code below, after completing each order, it asks if the user wants to keep ordering (keep_ordering). If the user responds with "N" or "n", it breaks out of the place_order loop, completes the order, and prints the final bill. If the user responds with "Y" or "y", it continues the place_order loop, allowing the user to select another menu category. That structure ensures that the user can continue ordering or complete the order as desired.
 
MY SUGGESTED CODE TO CORRECT THE INSUFFICIENCY:
menu = {
    "Snacks": {
        "Cookie": .99,
        "Banana": .69,
        "Apple": .49,
        "Granola bar": 1.99
    },
    "Meals": {
        "Burrito": 4.49,
        "Teriyaki Chicken": 9.99,
        "Sushi": 7.49,
        "Pad Thai": 6.99,
        "Pizza": {
            "Cheese": 8.99,
            "Pepperoni": 10.99,
            "Vegetarian": 9.99
        },
        "Burger": {
            "Chicken": 7.49,
            "Beef": 8.49
        }
    },
    "Drinks": {
        "Soda": {
            "Small": 1.99,
            "Medium": 2.49,
            "Large": 2.99
        },
        "Tea": {
            "Green": 2.49,
            "Thai iced": 3.99,
            "Irish breakfast": 2.49
        },
        "Coffee": {
            "Espresso": 2.99,
            "Flat white": 2.99,
            "Iced": 3.49
        }
    },
    "Dessert": {
        "Chocolate lava cake": 10.99,
        "Cheesecake": {
            "New York": 4.99,
            "Strawberry": 6.49
        },
        "Australian Pavlova": 9.99,
        "Rice pudding": 4.99,
        "Fried banana": 4.49
    }
}

# Launch the store and present a greeting to the customer
print("Welcome to the variety food truck.")

# Set up order list
order_list = []

# Main ordering loop
while True:
    # Reset place_order to True at the beginning of each loop iteration
    place_order = True

    while place_order:
        # Ask the customer from which menu category they want to order
        print("From which menu would you like to order? ")

        i = 1
        menu_items = {}

        # Print the menu options
        for key in menu.keys():
            print(f"{i}: {key}")
            menu_items[i] = key
            i += 1

        # Get the customer's input
        menu_category = input("Type menu number: ")

        if menu_category.isdigit() and int(menu_category) in menu_items.keys():
            menu_category_name = menu_items[int(menu_category)]
            print(f"You selected {menu_category_name}")

            # Print out the menu options
            print(f"What {menu_category_name} item would you like to order?")
            i = 1
            menu_items = {}
            print("Item # | Item name                | Price")
            print("-------|--------------------------|-------")
            for key, value in menu[menu_category_name].items():
                if isinstance(value, dict):
                    for key2, value2 in value.items():
                        num_item_spaces = 24 - len(key + key2) - 3
                        item_spaces = " " * num_item_spaces
                        print(f"{i}      | {key} - {key2}{item_spaces} | ${value2}")
                        menu_items[i] = {
                            "Item name": key + " - " + key2,
                            "Price": value2
                        }
                        i += 1
                else:
                    num_item_spaces = 24 - len(key)
                    item_spaces = " " * num_item_spaces
                    print(f"{i}      | {key}{item_spaces} | ${value}")
                    menu_items[i] = {
                        "Item name": key,
                        "Price": value
                    }
                    i += 1

            menu_selection = input("Type menu number to order or q to quit: ")

            if menu_selection.lower() == 'q':
                place_order = False
                print("Thank you for visiting!")
                break

            if menu_selection.isdigit() and int(menu_selection) in menu_items.keys():
                menu_selection = int(menu_selection)
                item_name = menu_items[int(menu_selection)]

                quantity = input(f"How many {item_name['Item name']}s would you like to order? (Default is 1): ")

                if quantity.isdigit():
                    quantity = int(quantity)
                else:
                    quantity = 1

                order_list.append({
                    "Item name": item_name["Item name"],
                    "Price": item_name["Price"],
                    "Quantity": quantity
                })

            else:
                print("Invalid input. Please select a valid menu item.")

        else:
            print("Invalid input. Please select a valid menu number.")

        # Ask if the customer wants to keep ordering
        
        while True:
            keep_ordering = input("Would you like to keep ordering? (Y)es or (N)o: ")

            if keep_ordering.lower() == 'n':
                place_order = False
                print("Order completed.")
                print("Thank you for your order!")
                break
            elif keep_ordering.lower() == 'y':
                break
            else:
                print("Invalid input. Please enter Y or N.")

    # If place_order is False, break the main loop and print the order
    if not place_order:
        break

# Print the customer's order
print("This is what we are preparing for you.\n")
print("Item name                 | Price  | Quantity")
print("--------------------------|--------|----------")

for item in order_list:
    item_name = item['Item name']
    price = item['Price']
    quantity = item['Quantity']

    name_spaces = ' ' * (24 - len(item_name))
    price_spaces = ' ' * (10 - len(str(price)))
    quantity_spaces = ' ' * (8 - len(str(quantity)))

    print(f"{item_name}{name_spaces}${price}{price_spaces}{quantity}{quantity_spaces}")

# Calculate and print the total cost
total_cost = sum(item['Price'] * item['Quantity'] for item in order_list)
print(f"\nTotal: ${total_cost:.2f}")


