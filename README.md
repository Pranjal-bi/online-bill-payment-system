# online-bill-payment-system
import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.HashMap;
import java.util.Map;

class User {
    private String username;
    private String password;

    public User(String username, String password) {
        this.username = username;
        this.password = password;
    }

    public String getUsername() {
        return username;
    }

    public boolean authenticate(String password) {
        return this.password.equals(password);
    }
}

class BillPaymentSystem {
    private Map<String, User> users = new HashMap<>();

    public void registerUser(String username, String password) {
        users.put(username, new User(username, password));
    }

    public boolean loginUser(String username, String password) {
        User user = users.get(username);
        return user != null && user.authenticate(password);
    }
}

 class OnlineBillPaymentSystemUI {
    private static BillPaymentSystem system = new BillPaymentSystem();
    private static JFrame frame;

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> createAndShowGUI());
    }

    private static void createAndShowGUI() {
        frame = new JFrame("Online Bill Payment System");
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setSize(400, 300);
        frame.setLayout(new CardLayout());

        JPanel mainPanel = new JPanel(new GridLayout(3, 1, 10, 10));
        JLabel welcomeLabel = new JLabel("Welcome to Online Bill Payment System", JLabel.CENTER);
        JButton registerButton = new JButton("Register");
        JButton loginButton = new JButton("Login");

        mainPanel.add(welcomeLabel);
        mainPanel.add(registerButton);
        mainPanel.add(loginButton);

        JPanel registerPanel = new JPanel(new GridLayout(4, 1, 10, 10));
        JTextField regUsernameField = new JTextField();
        JPasswordField regPasswordField = new JPasswordField();
        JButton registerSubmitButton = new JButton("Register");
        JButton backButton1 = new JButton("Back");
        registerPanel.add(new JLabel("Enter Username:"));
        registerPanel.add(regUsernameField);
        registerPanel.add(new JLabel("Enter Password:"));
        registerPanel.add(regPasswordField);
        registerPanel.add(registerSubmitButton);
        registerPanel.add(backButton1);

        JPanel loginPanel = new JPanel(new GridLayout(4, 1, 10, 10));
        JTextField loginUsernameField = new JTextField();
        JPasswordField loginPasswordField = new JPasswordField();
        JButton loginSubmitButton = new JButton("Login");
        JButton backButton2 = new JButton("Back");
        loginPanel.add(new JLabel("Enter Username:"));
        loginPanel.add(loginUsernameField);
        loginPanel.add(new JLabel("Enter Password:"));
        loginPanel.add(loginPasswordField);
        loginPanel.add(loginSubmitButton);
        loginPanel.add(backButton2);

        JPanel billPanel = new JPanel(new GridLayout(4, 1, 10, 10));
        JTextField billTypeField = new JTextField();
        JTextField billAmountField = new JTextField();
        JButton payBillButton = new JButton("Pay Bill");
        JButton backButton3 = new JButton("Logout");
        billPanel.add(new JLabel("Enter Bill Type (e.g., Electricity, Water):"));
        billPanel.add(billTypeField);
        billPanel.add(new JLabel("Enter Amount:"));
        billPanel.add(billAmountField);
        billPanel.add(payBillButton);
        billPanel.add(backButton3);

        frame.add(mainPanel, "Main");
        frame.add(registerPanel, "Register");
        frame.add(loginPanel, "Login");
        frame.add(billPanel, "Bill");

        CardLayout cl = (CardLayout) frame.getContentPane().getLayout();

        registerButton.addActionListener(e -> cl.show(frame.getContentPane(), "Register"));
        loginButton.addActionListener(e -> cl.show(frame.getContentPane(), "Login"));

        backButton1.addActionListener(e -> cl.show(frame.getContentPane(), "Main"));
        backButton2.addActionListener(e -> cl.show(frame.getContentPane(), "Main"));
        backButton3.addActionListener(e -> cl.show(frame.getContentPane(), "Main"));

        registerSubmitButton.addActionListener(e -> {
            String username = regUsernameField.getText();
            String password = new String(regPasswordField.getPassword());
            system.registerUser(username, password);
            JOptionPane.showMessageDialog(frame, "Registration Successful!");
            regUsernameField.setText("");
            regPasswordField.setText("");
            cl.show(frame.getContentPane(), "Main");
        });

        loginSubmitButton.addActionListener(e -> {
            String username = loginUsernameField.getText();
            String password = new String(loginPasswordField.getPassword());
            if (system.loginUser(username, password)) {
                JOptionPane.showMessageDialog(frame, "Login Successful! Welcome, " + username);
                loginUsernameField.setText("");
                loginPasswordField.setText("");
                cl.show(frame.getContentPane(), "Bill");
            } else {
                JOptionPane.showMessageDialog(frame, "Invalid Username or Password. Please try again.");
            }
        });


        payBillButton.addActionListener(e -> {
            String billType = billTypeField.getText();
            String amount = billAmountField.getText();
            JOptionPane.showMessageDialog(frame, "Payment Successful for " + billType + " ($" + amount + ")! Thank you.");
            billTypeField.setText("");
            billAmountField.setText("");
        });

        frame.setVisible(true);
    }
}
