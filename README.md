import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;

abstract class Region {
    String name;

    public Region(String name) {
        this.name = name;
    }

    public abstract double calculateTax(double carPrice);

    public abstract double calculatePrice(double basePrice);
}

class NorthAmerica extends Region {
    public NorthAmerica() {
        super("North America");
    }

    @Override
    public double calculateTax(double carPrice) {
        return carPrice * 0.08; // 8% tax in North America
    }

    @Override
    public double calculatePrice(double basePrice) {
        return basePrice + calculateTax(basePrice);
    }
}

class Europe extends Region {
    public Europe() {
        super("Europe");
    }

    @Override
    public double calculateTax(double carPrice) {
        return carPrice * 0.12; // 12% tax in Europe
    }

    @Override
    public double calculatePrice(double basePrice) {
        return basePrice + calculateTax(basePrice);
    }
}

class Asia extends Region {
    public Asia() {
        super("Asia");
    }

    @Override
    public double calculateTax(double carPrice) {
        return carPrice * 0.05; // 5% tax in Asia
    }

    @Override
    public double calculatePrice(double basePrice) {
        return basePrice + calculateTax(basePrice);
    }
}

class Globe {
    private Map<String, Region> regions;

    public Globe() {
        regions = new HashMap<>();
        regions.put("R1", new NorthAmerica());
        regions.put("R2", new Europe());
        regions.put("R3", new Asia());
    }

    public double[] calculatePriceAndTax(String carName, String countryName) {
        Region region = getRegion(countryName);
        double basePrice = getBasePrice(carName);
        double tax = region.calculateTax(basePrice);
        double totalPrice = region.calculatePrice(basePrice);

        return new double[]{tax, totalPrice};
    }

    private Region getRegion(String countryName) {
        switch (countryName) {
            case "USA":
            case "CANADA":
                return regions.get("R1");
            case "Europe":
                return regions.get("R2");
            case "Asia":
                return regions.get("R3");
            default:
                throw new IllegalArgumentException("Invalid country name");
        }
    }

    private double getBasePrice(String carName) {
        // You can implement a more sophisticated logic to fetch car prices based on the carName
        // For simplicity, a static map is used here
        Map<String, Double> carPrices = new HashMap<>();
        carPrices.put("Car1", 25000.0);
        carPrices.put("Car2", 30000.0);
        carPrices.put("Car3", 35000.0);

        return carPrices.getOrDefault(carName, 0.0);
    }
}

public class CarPriceTaxCalculator {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Globe globe = new Globe();

        System.out.print("Enter the car name: ");
        String carName = scanner.nextLine();

        System.out.print("Enter the country name: ");
        String countryName = scanner.nextLine();

        double[] result = globe.calculatePriceAndTax(carName, countryName);

        System.out.printf("Tax imposed on %s in %s: $%.2f%n", carName, countryName, result[0]);
        System.out.printf("Total price of %s in %s: $%.2f%n", carName, countryName, result[1]);
    }
}
