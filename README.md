import java.util.ArrayList;
import java.util.Scanner;

// Car class
class Car {
    private String model;
    private String plateNumber;
    private double pricePerDay;
    private boolean isAvailable;

    public Car(String model, String plateNumber, double pricePerDay) {
        this.model = model;
        this.plateNumber = plateNumber;
        this.pricePerDay = pricePerDay;
        this.isAvailable = true;
    }

    public String getModel() { return model; }
    public String getPlateNumber() { return plateNumber; }
    public double getPricePerDay() { return pricePerDay; }
    public boolean isAvailable() { return isAvailable; }

    public void rent() { isAvailable = false; }
    public void returnCar() { isAvailable = true; }

    @Override
    public String toString() {
        return model + " (" + plateNumber + ") - $" + pricePerDay + "/day - " +
               (isAvailable ? "Available" : "Rented");
    }
}

// Customer class
class Customer {
    private String name;
    private String id;

    public Customer(String name, String id) {
        this.name = name;
        this.id = id;
    }

    public String getName() { return name; }
    public String getId() { return id; }

    @Override
    public String toString() {
        return name + " (ID: " + id + ")";
    }
}

// Rental Agency class
class RentalAgency {
    private ArrayList<Car> cars = new ArrayList<>();
    private ArrayList<Customer> customers = new ArrayList<>();

    public void addCar(Car car) {
        cars.add(car);
    }

    public void addCustomer(Customer customer) {
        customers.add(customer);
    }

    public void showAvailableCars() {
        System.out.println("🚗 Available Cars:");
        for (Car car : cars) {
            if (car.isAvailable()) {
                System.out.println(car);
            }
        }
    }

    public void rentCar(String plateNumber, String customerId) {
        for (Car car : cars) {
            if (car.getPlateNumber().equals(plateNumber) && car.isAvailable()) {
                car.rent();
                System.out.println("✅ Car " + car.getModel() + " rented successfully by Customer ID " + customerId);
                return;
            }
        }
        System.out.println("❌ Car not available or wrong plate number.");
    }

    public void returnCar(String plateNumber) {
        for (Car car : cars) {
            if (car.getPlateNumber().equals(plateNumber) && !car.isAvailable()) {
                car.returnCar();
                System.out.println("✅ Car " + car.getModel() + " returned successfully.");
                return;
            }
        }
        System.out.println("❌ Invalid return. Car not found or already available.");
    }
}

// Main class
public class CarRentalSystem {
    public static void main(String[] args) {
        RentalAgency agency = new RentalAgency();

        // Add sample data
        agency.addCar(new Car("Toyota Corolla", "KDA123A", 50));
        agency.addCar(new Car("Honda Civic", "KDB456B", 60));
        agency.addCustomer(new Customer("Alice", "C001"));
        agency.addCustomer(new Customer("Bob", "C002"));

        Scanner scanner = new Scanner(System.in);
        int choice;

        do {
            System.out.println("\n=== Car Rental Menu ===");
            System.out.println("1. Show Available Cars");
            System.out.println("2. Rent a Car");
            System.out.println("3. Return a Car");
            System.out.println("4. Exit");
            System.out.print("Enter choice: ");
            choice = scanner.nextInt();

            switch (choice) {
                case 1:
                    agency.showAvailableCars();
                    break;
                case 2:
                    System.out.print("Enter Plate Number: ");
                    String plate = scanner.next();
                    System.out.print("Enter Customer ID: ");
                    String cid = scanner.next();
                    agency.rentCar(plate, cid);
                    break;
                case 3:
                    System.out.print("Enter Plate Number to return: ");
                    String returnPlate = scanner.next();
                    agency.returnCar(returnPlate);
                    break;
                case 4:
                    System.out.println("👋 Goodbye!");
                    break;
                default:
                    System.out.println("❌ Invalid choice, try again.");
            }
        } while (choice != 4);

        scanner.close();
    }
}
