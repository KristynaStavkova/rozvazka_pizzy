# rozvazka_pizzy
class Item:
    def __init__(self, name, price):
        self.name = name
        self.price = price

    def __str__(self):
        return f"{self.name}: {self.price} Kč"

class Pizza(Item):
    def __init__(self, name, price, ingredients):
        super().__init__(name, price)
        self.ingredients = ingredients

    def add_extra(self, ingredient, quantity, price_per_ingredient):
        self.ingredients[ingredient] = quantity
        self.price += quantity * price_per_ingredient

    def __str__(self):
        ingredients_str = ", ".join([f"{ingredient}: {quantity}g" for ingredient, quantity in self.ingredients.items()])
        return f"{self.name} ({ingredients_str}): {self.price} Kč"

class Drink(Item):
    def __init__(self, name, price, volume):
        super().__init__(name, price)
        self.volume = volume

    def __str__(self):
        return f"{self.name} ({self.volume} ml): {self.price} Kč"
    
# 2
class Order:
    def __init__(self, customer_name, delivery_address, items):
        self.customer_name = customer_name
        self.delivery_address = delivery_address
        self.items = items
        self.status = "Nová"

    def mark_delivered(self):
        self.status = "Doručeno"

    def __str__(self):
        item_list = "\n".join([str(item) for item in self.items])
        return f"Objednávka pro: {self.customer_name}\nAdresa doručení: {self.delivery_address}\n\nPoložky v objednávce:\n{item_list}\n\nStav objednávky: {self.status}"
    
# 3
class DeliveryPerson:
    def __init__(self, name, phone_number):
        self.name = name
        self.phone_number = phone_number
        self.available = True
        self.current_order = None

    def assign_order(self, order):
        if self.available:
            self.current_order = order
            self.current_order.status = "Na cestě"
            self.available = False
            print(f"Objednávka přidělena doručovateli {self.name}.")
        else:
            print("Doručovatel není dostupný.")

    def complete_delivery(self):
        if self.current_order:
            self.current_order.mark_delivered()
            print(f"Objednávka doručena zákazníkovi.")
            self.available = True
            self.current_order = None
        else:
            print("Doručovatel nemá žádnou objednávku k doručení.")

    def __str__(self):
        availability = "Dostupný" if self.available else "Nedostupný"
        return f"Doručovatel: {self.name}\nTelefon: {self.phone_number}\nDostupnost: {availability}"


# přiklad použití
margarita = Pizza("Margarita", 200, {"sýr": 100, "rajčata": 150})
margarita.add_extra("olivy", 50, 10)
cola = Drink("Cola", 1.5, 500)
order = Order("Jan Novák", "Pražská 123", [margarita, cola])
print(order)

delivery_person = DeliveryPerson("Petr Novotný", "777 888 999")
delivery_person.assign_order(order)
print(delivery_person)

delivery_person.complete_delivery()
print(delivery_person)

print(order)
