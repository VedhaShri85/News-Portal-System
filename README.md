import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.util.ArrayList;

public class NewsPortalSwing {

    // Lists to hold news articles and users
    static ArrayList<Article> articles = new ArrayList<>();
    static ArrayList<User> users = new ArrayList<>();

    // GUI elements
    JFrame mainFrame;
    CardLayout cardLayout;
    JPanel mainPanel;

    // Current logged-in user
    User currentUser = null;

    public static void main(String[] args) {
        NewsPortalSwing portal = new NewsPortalSwing();
        portal.createUI();
    }

    public NewsPortalSwing() {
        // Add some default articles
        articles.add(new Article("Politics", "Global leaders meet at UN summit", "World leaders discussed climate change and global policies."));
        articles.add(new Article("Sports", "World Cup 2022 Highlights", "The championship game ended in a thrilling penalty shootout."));
        articles.add(new Article("Technology", "AI revolutionizes tech industry", "AI is transforming how industries operate, with new advancements daily."));
        articles.add(new Article("Entertainment", "Blockbuster Movie Released", "A highly anticipated movie breaks box office records."));

        // Add default admin for simplicity
        users.add(new User("admin", "admin123", "Admin"));
    }

    public void createUI() {
        mainFrame = new JFrame("News Portal");
        cardLayout = new CardLayout();
        mainPanel = new JPanel(cardLayout);

        // Create panels for each module
        JPanel mainMenuPanel = createMainMenuPanel();
        JPanel userMenuPanel = createUserMenuPanel();
        JPanel adminMenuPanel = createAdminMenuPanel();
        JPanel guestPanel = createGuestPanel();

        // Add panels to main layout
        mainPanel.add("MainMenu", mainMenuPanel);
        mainPanel.add("UserMenu", userMenuPanel);
        mainPanel.add("AdminMenu", adminMenuPanel);
        mainPanel.add("GuestView", guestPanel);

        mainFrame.setLayout(new BorderLayout());
        mainFrame.add(mainPanel, BorderLayout.CENTER);
        mainFrame.setSize(600, 500);
        mainFrame.setVisible(true);
        mainFrame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    }

    // Main Menu Panel
    private JPanel createMainMenuPanel() {
        JPanel panel = new JPanel();
        panel.setLayout(new GridLayout(4, 1));

        JButton userButton = new JButton("User");
        JButton adminButton = new JButton("Admin");
        JButton guestButton = new JButton("Guest");
        JButton exitButton = new JButton("Exit");

        userButton.addActionListener(e -> cardLayout.show(mainPanel, "UserMenu"));
        adminButton.addActionListener(e -> cardLayout.show(mainPanel, "AdminMenu"));
        guestButton.addActionListener(e -> cardLayout.show(mainPanel, "GuestView"));
        exitButton.addActionListener(e -> System.exit(0));

        panel.add(userButton);
        panel.add(adminButton);
        panel.add(guestButton);
        panel.add(exitButton);

        return panel;
    }

    // User Module Panel
    private JPanel createUserMenuPanel() {
        JPanel panel = new JPanel();
        panel.setLayout(new BorderLayout());

        JTextField usernameField = new JTextField();
        JPasswordField passwordField = new JPasswordField();

        JButton loginButton = new JButton("Login");
        JButton registerButton = new JButton("Register");
        JLabel messageLabel = new JLabel("User Login/Registration", JLabel.CENTER);
        messageLabel.setForeground(Color.RED);

        loginButton.addActionListener(e -> {
            String username = usernameField.getText();
            String password = new String(passwordField.getPassword());
            if (isValidUser(username, password)) {
                currentUser = new User(username, password, "User");
                showUserCategories();
            } else {
                messageLabel.setText("Invalid credentials. Please try again.");
            }
        });

        registerButton.addActionListener(e -> {
            String username = usernameField.getText();
            String password = new String(passwordField.getPassword());
            if (!isUserExists(username)) {
                users.add(new User(username, password, "User"));
                messageLabel.setText("Registration successful! Login to proceed.");
            } else {
                messageLabel.setText("User already exists.");
            }
        });

        JButton exitButton = new JButton("Exit");
        exitButton.addActionListener(e -> cardLayout.show(mainPanel, "MainMenu"));

        JPanel inputPanel = new JPanel(new GridLayout(3, 1));
        inputPanel.add(new JLabel("Username:"));
        inputPanel.add(usernameField);
        inputPanel.add(new JLabel("Password:"));
        inputPanel.add(passwordField);

        JPanel buttonPanel = new JPanel();
        buttonPanel.add(loginButton);
        buttonPanel.add(registerButton);
        buttonPanel.add(exitButton);

        panel.add(inputPanel, BorderLayout.CENTER);
        panel.add(buttonPanel, BorderLayout.SOUTH);
        panel.add(messageLabel, BorderLayout.NORTH);

        return panel;
    }

    // Admin Module Panel
    private JPanel createAdminMenuPanel() {
        JPanel panel = new JPanel();
        panel.setLayout(new BorderLayout());

        JTextField usernameField = new JTextField();
        JPasswordField passwordField = new JPasswordField();

        JButton loginButton = new JButton("Login");
        JButton registerButton = new JButton("Register");
        JLabel messageLabel = new JLabel("Admin Login/Registration", JLabel.CENTER);
        messageLabel.setForeground(Color.RED);

        loginButton.addActionListener(e -> {
            String username = usernameField.getText();
            String password = new String(passwordField.getPassword());
            if (isValidAdmin(username, password)) {
                currentUser = new User(username, password, "Admin");
                showAdminControls();
            } else {
                messageLabel.setText("Invalid credentials. Please try again.");
            }
        });

        registerButton.addActionListener(e -> {
            String username = usernameField.getText();
            String password = new String(passwordField.getPassword());
            if (!isUserExists(username)) {
                users.add(new User(username, password, "Admin"));
                messageLabel.setText("Registration successful! Login to proceed.");
            } else {
                messageLabel.setText("User already exists.");
            }
        });

        JButton exitButton = new JButton("Exit");
        exitButton.addActionListener(e -> cardLayout.show(mainPanel, "MainMenu"));

        JPanel inputPanel = new JPanel(new GridLayout(3, 1));
        inputPanel.add(new JLabel("Username:"));
        inputPanel.add(usernameField);
        inputPanel.add(new JLabel("Password:"));
        inputPanel.add(passwordField);

        JPanel buttonPanel = new JPanel();
        buttonPanel.add(loginButton);
        buttonPanel.add(registerButton);
        buttonPanel.add(exitButton);

        panel.add(inputPanel, BorderLayout.CENTER);
        panel.add(buttonPanel, BorderLayout.SOUTH);
        panel.add(messageLabel, BorderLayout.NORTH);

        return panel;
    }

    // Show Admin Controls
    private void showAdminControls() {
        JPanel panel = new JPanel();
        panel.setLayout(new GridLayout(5, 1));

        JButton addArticleButton = new JButton("Add Article");
        JButton manageArticleButton = new JButton("Manage Articles");
        JButton categorizeButton = new JButton("Categorization");
        JButton feedbackButton = new JButton("View Feedback");
        JButton exitButton = new JButton("Exit");

        exitButton.addActionListener(e -> cardLayout.show(mainPanel, "AdminMenu"));

        panel.add(addArticleButton);
        panel.add(manageArticleButton);
        panel.add(categorizeButton);
        panel.add(feedbackButton);
        panel.add(exitButton);

        mainPanel.add("AdminControls", panel);
        cardLayout.show(mainPanel, "AdminControls");
    }

    // Show User Categories
    private void showUserCategories() {
        JPanel panel = new JPanel();
        panel.setLayout(new GridLayout(0, 1));

        JButton politicsButton = new JButton("Politics");
        JButton sportsButton = new JButton("Sports");
        JButton technologyButton = new JButton("Technology");
        JButton entertainmentButton = new JButton("Entertainment");
        JButton exitButton = new JButton("Exit");

        politicsButton.addActionListener(e -> showNewsForCategory("Politics"));
        sportsButton.addActionListener(e -> showNewsForCategory("Sports"));
        technologyButton.addActionListener(e -> showNewsForCategory("Technology"));
        entertainmentButton.addActionListener(e -> showNewsForCategory("Entertainment"));
        exitButton.addActionListener(e -> cardLayout.show(mainPanel, "UserMenu"));

        panel.add(politicsButton);
        panel.add(sportsButton);
        panel.add(technologyButton);
        panel.add(entertainmentButton);
        panel.add(exitButton);

        mainPanel.add("UserCategories", panel);
        cardLayout.show(mainPanel, "UserCategories");
    }

    // Show News for Selected Category
    private void showNewsForCategory(String category) {
        JPanel panel = new JPanel();
        panel.setLayout(new BorderLayout());

        JTextArea newsArea = new JTextArea();
        for (Article article : articles) {
            if (article.getCategory().equalsIgnoreCase(category)) {
                newsArea.append(article.getTitle() + "\n" + article.getContent() + "\n\n");
            }
        }

        JButton backButton = new JButton("Back");
        backButton.addActionListener(e -> cardLayout.show(mainPanel, "UserCategories"));

        panel.add(newsArea, BorderLayout.CENTER);
        panel.add(backButton, BorderLayout.SOUTH);

        mainPanel.add("CategoryNews", panel);
        cardLayout.show(mainPanel, "CategoryNews");
    }

    // Guest Panel
    private JPanel createGuestPanel() {
        JPanel panel = new JPanel();
        panel.setLayout(new BorderLayout());

        JTextArea newsArea = new JTextArea();
        for (Article article : articles) {
            newsArea.append(article.getTitle() + "\n" + article.getContent() + "\n\n");
        }

        JButton exitButton = new JButton("Exit");
        exitButton.addActionListener(e -> cardLayout.show(mainPanel, "MainMenu"));

        panel.add(newsArea, BorderLayout.CENTER);
        panel.add(exitButton, BorderLayout.SOUTH);

        return panel;
    }

    // Check if user is valid
    private boolean isValidUser(String username, String password) {
        for (User user : users) {
            if (user.getUsername().equals(username) && user.getPassword().equals(password) && user.getRole().equals("User")) {
                return true;
            }
        }
        return false;
    }

    // Check if admin is valid
    private boolean isValidAdmin(String username, String password) {
        for (User user : users) {
            if (user.getUsername().equals(username) && user.getPassword().equals(password) && user.getRole().equals("Admin")) {
                return true;
            }
        }
        return false;
    }

    // Check if user exists
    private boolean isUserExists(String username) {
        for (User user : users) {
            if (user.getUsername().equals(username)) {
                return true;
            }
        }
        return false;
    }
}

// User Class
class User {
    private String username;
    private String password;
    private String role;

    public User(String username, String password, String role) {
        this.username = username;
        this.password = password;
        this.role = role;
    }

    public String getUsername() {
        return username;
    }

    public String getPassword() {
        return password;
    }

    public String getRole() {
        return role;
    }
}

// Article Class
class Article {
    private String category;
    private String title;
    private String content;

    public Article(String category, String title, String content) {
        this.category = category;
        this.title = title;
        this.content = content;
    }

    public String getCategory() {
        return category;
    }

    public String getTitle() {
        return title;
    }

    public String getContent() {
        return content;
    }
}
