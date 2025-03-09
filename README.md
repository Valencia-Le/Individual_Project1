package LeThiKimNgan_6496;

import javax.swing.*;
import java.awt.*;
import java.util.ArrayList;
import java.util.List;

public class POSSystem {

    private static final List<String> cart = new ArrayList<>();
    private static int total = 0;
    private static String customerName = "";
    private static String customerPhone = "";

    public static void main(String[] args) {
        JFrame frame = new JFrame("POS System");
        frame.setSize(600, 400);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setLayout(new GridLayout(3, 2, 10, 10));

        JButton btnAddProduct = createButton("Add Product");
        JButton btnAddCustomer = createButton("Add Customer");
        JButton btnCreateInvoice = createButton("Create Invoice");
        JButton btnCheckout = createButton("Checkout");
        JButton btnViewProducts = createButton("View Product List");
        JButton btnViewCart = createButton("View Cart");

        btnAddProduct.addActionListener(e -> showViewProductsDialog());
        btnAddCustomer.addActionListener(e -> showAddCustomerDialog());
        btnCreateInvoice.addActionListener(e -> showCreateInvoiceDialog());
        btnCheckout.addActionListener(e -> showCheckoutDialog());
        btnViewProducts.addActionListener(e -> showViewProductsDialog());
        btnViewCart.addActionListener(e -> showViewCartDialog());

        frame.add(btnAddProduct);
        frame.add(btnAddCustomer);
        frame.add(btnCreateInvoice);
        frame.add(btnCheckout);
        frame.add(btnViewProducts);
        frame.add(btnViewCart);

        frame.setExtendedState(JFrame.MAXIMIZED_BOTH);
        frame.setVisible(true);
    }

    private static JButton createButton(String text) {
        JButton button = new JButton(text);
        button.setFont(new Font("Arial", Font.BOLD, 24));
        return button;
    }

    private static void showViewProductsDialog() {
        JFrame productCategoryFrame = new JFrame("Select Product Category");
        productCategoryFrame.setSize(600, 400);
        productCategoryFrame.setLayout(new GridLayout(2, 1, 5, 5));

        JButton btnDrinks = createButton("Drinks");
        JButton btnCakes = createButton("Cakes");

        btnDrinks.addActionListener(e -> showProductSelectionDialog("Drinks"));
        btnCakes.addActionListener(e -> showProductSelectionDialog("Cakes"));

        productCategoryFrame.add(btnDrinks);
        productCategoryFrame.add(btnCakes);

        productCategoryFrame.setLocationRelativeTo(null);
        productCategoryFrame.setVisible(true);
    }

    private static void showProductSelectionDialog(String category) {
        JFrame productFrame = new JFrame("Product List: " + category);
        productFrame.setSize(600, 400);
        productFrame.setLayout(new BorderLayout(5, 5));

        String[][] products = category.equals("Drinks")
                ? new String[][]{{"Iced Milk Coffee", "30000"}, {"Green Tea Latte", "45000"},
                {"Espresso", "35000"}, {"Americano", "40000"}}
                : new String[][]{{"Croissant", "25000"}, {"Salted Egg Sponge Cake", "50000"},
                {"Tiramisu", "60000"}, {"Macaron", "30000"}};

        List<String> selectedProducts = new ArrayList<>();
        List<Integer> selectedPrices = new ArrayList<>();

        JPanel productPanel = new JPanel();
        productPanel.setLayout(new GridLayout(products.length, 1, 5, 5));

        for (String[] product : products) {
            JButton btnProduct = createButton(product[0] + " - " + product[1] + "đ");
            btnProduct.addActionListener(e -> {
                if (!selectedProducts.contains(product[0])) {
                    selectedProducts.add(product[0]);
                    selectedPrices.add(Integer.parseInt(product[1]));
                } else {
                    int index = selectedProducts.indexOf(product[0]);
                    selectedProducts.remove(index);
                    selectedPrices.remove(index);
                }
            });
            productPanel.add(btnProduct);
        }

        JPanel buttonPanel = new JPanel();
        buttonPanel.setLayout(new GridLayout(1, 2, 5, 5));

        JButton btnOk = createButton("OK");
        btnOk.addActionListener(e -> {
            for (int i = 0; i < selectedProducts.size(); i++) {
                addProductToCart(selectedProducts.get(i), selectedPrices.get(i));
            }
            productFrame.dispose();
        });

        JButton btnCancel = createButton("Cancel");
        btnCancel.addActionListener(e -> productFrame.dispose());

        buttonPanel.add(btnOk);
        buttonPanel.add(btnCancel);

        productFrame.add(productPanel, BorderLayout.CENTER);
        productFrame.add(buttonPanel, BorderLayout.SOUTH);
        productFrame.setLocationRelativeTo(null);
        productFrame.setVisible(true);
    }

    private static void showAddCustomerDialog() {
        JTextField nameField = new JTextField();
        JTextField phoneField = new JTextField();

        Object[] message = {"Customer Name:", nameField, "Phone Number:", phoneField};

        int option = JOptionPane.showConfirmDialog(null, message, "Enter Customer Information", JOptionPane.OK_CANCEL_OPTION);
        if (option == JOptionPane.OK_OPTION) {
            customerName = nameField.getText();
            customerPhone = phoneField.getText();
            JOptionPane.showMessageDialog(null, "Customer information saved!", "Notification", JOptionPane.INFORMATION_MESSAGE);
        }
    }

    private static void showCreateInvoiceDialog() {
        if (cart.isEmpty()) {
            JOptionPane.showMessageDialog(null, "The cart is empty. Please add products before creating an invoice.", "Error", JOptionPane.WARNING_MESSAGE);
            return;
        }
        StringBuilder invoice = new StringBuilder("INVOICE\n-------------------\n");
        invoice.append("Customer: ").append(customerName.isEmpty() ? "N/A" : customerName).append("\n");
        invoice.append("Phone Number: ").append(customerPhone.isEmpty() ? "N/A" : customerPhone).append("\n\n");

        for (String item : cart) {
            invoice.append(item).append("\n");
        }
        invoice.append("-------------------\nTotal Amount: ").append(total).append("đ");

        JOptionPane.showMessageDialog(null, invoice.toString(), "Invoice", JOptionPane.INFORMATION_MESSAGE);
    }

    private static void showCheckoutDialog() {
        StringBuilder receipt = new StringBuilder("Invoice\n-------------------\n");
        for (String item : cart) {
            receipt.append(item).append("\n");
        }
        receipt.append("-------------------\nTotal Amount: ").append(total).append("đ");

        JOptionPane.showMessageDialog(null, receipt.toString(), "Checkout", JOptionPane.INFORMATION_MESSAGE);
        cart.clear();
        total = 0;
        JOptionPane.showMessageDialog(null, "Payment successful!", "Notification", JOptionPane.INFORMATION_MESSAGE);
    }

    private static void addProductToCart(String product, int price) {
        cart.add(product + " - " + price + "đ");
        total += price;
    }

    private static void showViewCartDialog() {
        StringBuilder cartContent = new StringBuilder("Shopping Cart:\n");
        for (String item : cart) {
            cartContent.append(item).append("\n");
        }
        cartContent.append("-------------------\nTotal Amount: ").append(total).append("đ");

        JOptionPane.showMessageDialog(null, cartContent.toString(), "Shopping Cart", JOptionPane.INFORMATION_MESSAGE);
    }
}
