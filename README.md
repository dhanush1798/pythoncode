# pythoncode
class TouristicPackage:
    def __init__(self, destination_city=None, destination_country=None, num_days=None, num_nights=None, hotel_name=None,
                 price=None, activities=None):
        self.destination_city = destination_city
        self.destination_country = destination_country
        self.num_days = num_days
        self.num_nights = num_nights
        self.hotel_name = hotel_name
        self.price = price
        self.activities = activities

    def __str__(self):
        return f"Destination ={self.destination_city, self.destination_country}\n" \
               f"Number of days={self.num_days}\n" \
               f"Number of nights={self.num_nights}\n" \
               f"Hotel name={self.hotel_name}\n" \
               f"Price={self.price}\n" \
               f"Activities included={self.activities}"


class Customer:
    def __init__(self, first_name=None, last_name=None, date_of_birth=None, email=None):
        self.first_name = first_name
        self.last_name = last_name
        self.date_of_birth = date_of_birth
        self.email = email


class Booking:
    def __init__(self, customer=None, package=None, departure_date=None):
        self.customer = customer
        self.package = package
        self.departure_date = departure_date


class TravelBookingSystem:
    def __init__(self):
        self.packages = []
        self.customers = []
        self.bookings = []

    def create_package(self, package):
        self.packages.append(package)

    def get_packages(self):
        n = 0
        for package in self.packages:
            n += 1
            print (f"Tourist package {n}:")
            print(package)

    def create_customer(self, customer):
        self.customers.append(customer)

    def make_booking(self, customer, package, departure_date):
        booking = Booking(customer, package, departure_date)
        self.bookings.append(booking)

    def modify_booking(self, booking, departure_date):
        booking.departure_date = departure_date

    def cancel_booking(self, booking):
        self.bookings.remove(booking)

    def generate_reports(self):
        report = []
        for booking in self.bookings:
            package = booking.package
            if package in report:
                report[package]["bookings"] += 1
                report[package]["revenue"] += package.price
            else:
                report[package] = {"bookings": 1, "revenue": package.price}
        return report


system = TravelBookingSystem()

# Create Package
num_of_packages = int(input("Enter number of packages your want create:"))
while num_of_packages <= 0:
    print("Please enter a natural number i.e., 1, 2, 3 ....")
    num_of_packages = int(input("Enter number of packages your want create:"))

for i in range(num_of_packages):
    input_package = TouristicPackage()
    input_package.destination_city = input("Enter destination city:")
    input_package.destination_country = input("Enter destination country:")
    input_package.num_days = int(input("Enter number of days of the trip:"))
    input_package.num_nights = int(input("Enter number nights of the trip:"))
    input_package.hotel_name = input("Enter the name of the hotel customer want to stay:")
    input_package.price = int(input("Enter the price of the tourist package:"))
    input_activities = input("Enter a list of activities separated by space: ")
    input_activities = input_activities.split()
    input_package.activities = [activity for activity in input_activities]

    system.create_package(input_package)
# Create Customer
num_of_customers = int(input("Enter number of customers your want create:"))
while num_of_customers <= 0:
    print("Please enter a natural number i.e., 1, 2, 3 ....")
    num_of_customers = int(input("Enter number of customers your want create:"))

for i in range(num_of_customers):
    input_customer = Customer()
    input_customer.first_name = input("Enter the first name of the customer:")
    input_customer.last_name = input("Enter the last name of the customer:")
    input_customer.date_of_birth = input("Enter date of birth of the customer:")
    input_customer.email = input("Enter email id of the customer:")

    system.create_customer(input_customer)

# Make Booking
customer_value = 0
for customer_num in system.customers:
    customer_value += 1
    print("Select any of the following tourist packages for customer " + str(customer_value))
    system.get_packages()
    package_num = int(input()) - 1
    date_of_departure = input("Enter departure date of the trip:")
    system.make_booking(customer_num, system.packages[package_num], date_of_departure)


# Modifying Booking
modification = input("Do you want to modify departure date for any booking? Y or N:")
if modification == "Y":
    booking_num = system.bookings[int(input("Which booking departure date do you want to modify? Y or N:")) - 1]
    system.modify_booking(booking_num, input("Enter new departure date:"))

# Cancelling Booking
cancellation = input("Do you want to cancel any booking? Y or N:")
if cancellation == "Y":
    booking_to_cancel = system.bookings[int(input("Enter the booking number do you want to cancel:")) - 1]
    system.cancel_booking(booking_to_cancel)

# Generating Reports
reports = system.generate_reports()
for input_package, data in reports.items():
    print("---------------------")
    print("Package:", input_package.destination_city)
    print("Bookings:", data["bookings"])
    print("---------------------")
print("Revenue:", data["revenue"])
print("---------------------")
