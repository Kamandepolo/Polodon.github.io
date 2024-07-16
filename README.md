class Product:
    def __init__(self, id, name, price):
        self.id = id
        self.name = name
        self.price = price

class Car(Product):
    def __init__(self, id, name, price, image):
        super().__init__(id, name, price)
        self.image = image

class App(Product):
    def __init__(self, id, name, price, platform):
        super().__init__(id, name, price)
        self.platform = platform

class Order:
    def __init__(self, id, products):
        self.id = id
        self.products = products

class Customer:
    def __init__(self, id, name, email):
        self.id = id
        self.name = name
        self.email = email
        self.orders = []

    def add_order(self, order):
        self.orders.append(order)

class ECommerceSite:
    def __init__(self, name, location, contact_email):
        self.name = name
        self.location = location
        self.contact_email = contact_email
        self.products = []
        self.customers = []

    def add_product(self, product):
        self.products.append(product)

    def list_products(self):
        for product in self.products:
            if isinstance(product, Car):
                print(f"Car: {product.name} | Price: ${product.price} | Image: {product.image}")
            elif isinstance(product, App):
                print(f"App: {product.name} | Price: ${product.price} | Platform: {product.platform}")

    def register_customer(self, name, email):
        customer_id = len(self.customers) + 1  # Generate a simple incremental ID
        new_customer = Customer(id=customer_id, name=name, email=email)
        self.customers.append(new_customer)
        return new_customer

    def place_order(self, customer, product_ids):
        ordered_products = []
        for pid in product_ids:
            for product in self.products:
                if product.id == pid:
                    ordered_products.append(product)
                    break
        order_id = len(customer.orders) + 1  # Generate a simple incremental ID
        new_order = Order(id=order_id, products=ordered_products)
        customer.add_order(new_order)
        return new_order

# Create an instance of the E-commerce site with your contact email
site = ECommerceSite(name="My E-commerce Site", location="Nairobi Kimathi Street 527", contact_email="kamandengaroiya@gmail.com")

# Add cars
site.add_product(Car(id=1, name="Car 1", price=50000, image="car1.jpg"))
site.add_product(Car(id=2, name="Car 2", price=60000, image="car2.jpg"))
site.add_product(Car(id=3, name="Car 3", price=70000, image="car3.jpg"))

# Add apps
site.add_product(App(id=1, name="Music App", price=10, platform="Android"))
site.add_product(App(id=2, name="Navigation App", price=5, platform="iOS"))
site.add_product(App(id=3, name="Utility App", price=8, platform="Windows"))

# Register customers
customer1 = site.register_customer(name="John Doe", email="johndoe@example.com")
customer2 = site.register_customer(name="Jane Smith", email="janesmith@example.com")

# Simulate orders
order1 = site.place_order(customer1, [1, 3])  # John Doe orders Car 1 and Utility App
order2 = site.place_order(customer2, [2])    # Jane Smith orders Car 2

# List all products
print("List of Products:")
site.list_products()

# List all customers and their orders
print("\nList of Customers and their Orders:")
for customer in site.customers:
    print(f"Customer: {customer.name} ({customer.email})")
    for order in customer.orders:
        print(f"Order ID: {order.id}, Products: {[product.name for product in order.products]}")
    print("\n")

# Display contact email
print(f"Contact Email: {site.contact_email}")
