
import java.util.ArrayList;
import java.util.Scanner;

class Product {
    int id;
    String name;
    double price;

    Product(int id, String name, double price) {
        this.id = id;
        this.name = name;
        this.price = price;
    }
}

class CartItem {
    Product product;
    int quantity;

    CartItem(Product product, int quantity) {
        this.product = product;
        this.quantity = quantity;
    }
}

public class EcommerceApp {

    static ArrayList<Product> products = new ArrayList<>();
    static ArrayList<CartItem> cart = new ArrayList<>();
    static Scanner sc = new Scanner(System.in);

    public static void main(String[] args) {

        products.add(new Product(1, "Laptop", 50000));
        products.add(new Product(2, "Mobile", 20000));
        products.add(new Product(3, "Headphones", 1500));
        products.add(new Product(4, "Keyboard", 800));
        products.add(new Product(5, "Mouse", 500));

        while (true) {
            System.out.println("\n===== E-COMMERCE APPLICATION =====");
            System.out.println("1. View Products");
            System.out.println("2. Add to Cart");
            System.out.println("3. View Cart");
            System.out.println("4. Remove from Cart");
            System.out.println("5. Checkout");
            System.out.println("6. Exit");
            System.out.print("Enter your choice: ");

            int choice = readInt();

            switch (choice) {
                case 1:
                    viewProducts();
                    break;
                case 2:
                    addToCart();
                    break;
                case 3:
                    viewCart();
                    break;
                case 4:
                    removeFromCart();
                    break;
                case 5:
                    checkout();
                    break;
                case 6:
                    System.out.println("Thank you for shopping!");
                    sc.close();
                    return;
                default:
                    System.out.println("Invalid choice!");
            }
        }
    }

    static int readInt() {
        while (!sc.hasNextInt()) {
            System.out.print("Enter a valid number: ");
            sc.nextLine();
        }
        int value = sc.nextInt();
        sc.nextLine();
        return value;
    }

    static void viewProducts() {
        System.out.println("\n===== AVAILABLE PRODUCTS =====");

        for (Product p : products) {
            System.out.printf(
                "%d. %s - Rs. %.2f%n",
                p.id, p.name, p.price
            );
        }
    }

    static void addToCart() {
        viewProducts();

        System.out.print("Enter product ID: ");
        int id = readInt();

        Product selected = null;

        for (Product p : products) {
            if (p.id == id) {
                selected = p;
                break;
            }
        }

        if (selected == null) {
            System.out.println("Product not found!");
            return;
        }

        System.out.print("Enter quantity: ");
        int quantity = readInt();

        if (quantity <= 0) {
            System.out.println("Quantity must be positive!");
            return;
        }

        for (CartItem item : cart) {
            if (item.product.id == id) {
                item.quantity += quantity;
                System.out.println("Cart updated successfully!");
                return;
            }
        }

        cart.add(new CartItem(selected, quantity));
        System.out.println("Product added to cart!");
    }

    static void viewCart() {
        if (cart.isEmpty()) {
            System.out.println("Your cart is empty!");
            return;
        }

        double total = 0;

        System.out.println("\n===== SHOPPING CART =====");

        for (CartItem item : cart) {
            double subtotal =
                item.product.price * item.quantity;

            System.out.printf(
                "%s | Qty: %d | Rs. %.2f%n",
                item.product.name,
                item.quantity,
                subtotal
            );

            total += subtotal;
        }

        System.out.printf("Total: Rs. %.2f%n", total);
    }

    static void removeFromCart() {
        if (cart.isEmpty()) {
            System.out.println("Cart is empty!");
            return;
        }

        System.out.print("Enter product ID to remove: ");
        int id = readInt();

        for (CartItem item : cart) {
            if (item.product.id == id) {
                cart.remove(item);
                System.out.println("Product removed from cart!");
                return;
            }
        }

        System.out.println("Product not found in cart!");
    }

    static void checkout() {
        if (cart.isEmpty()) {
            System.out.println("Cart is empty!");
            return;
        }

        viewCart();

        System.out.println("\nOrder placed successfully!");
        System.out.println("Thank you for your purchase!");

        cart.clear();
    }
}
