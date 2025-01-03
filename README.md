
import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;

   public class Registration {
        public static void main(String[] args) {
            // Create the main frame
            JFrame frame = new JFrame("Sign-Up Form");
            frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            frame.setSize(500, 400);
            frame.setLayout(new BorderLayout());

            // Create a panel with padding and background color
            JPanel panel = new JPanel();
            panel.setLayout(new GridBagLayout());
            panel.setBackground(new Color(240, 248, 255)); // Light blue background
            panel.setBorder(BorderFactory.createEmptyBorder(20, 20, 20, 20)); // Padding around the panel

            // Set layout constraints
            GridBagConstraints gbc = new GridBagConstraints();
            gbc.insets = new Insets(10, 10, 10, 10); // Margins between components
            gbc.fill = GridBagConstraints.HORIZONTAL;

            // Add components with labels
            JLabel nameLabel = new JLabel("Name:");
            nameLabel.setFont(new Font("Arial",Font.BOLD,20));
            nameLabel.setForeground(new Color(0, 102, 204)); // Set label color
            gbc.gridx = 0;
            gbc.gridy = 0;
            panel.add(nameLabel, gbc);

            JTextField nameField = new JTextField();
            gbc.gridx = 1;
            gbc.gridy = 0;
            panel.add(nameField, gbc);

            JLabel emailLabel = new JLabel("Email:");
            emailLabel.setForeground(new Color(0, 102, 204)); // Set label color
            gbc.gridx = 0;
            gbc.gridy = 1;
            panel.add(emailLabel, gbc);

            JTextField emailField = new JTextField();
            gbc.gridx = 1;
            gbc.gridy = 1;
            panel.add(emailField, gbc);

            JLabel genderLabel = new JLabel("Gender:");
            genderLabel.setForeground(new Color(0, 102, 204)); // Set label color
            gbc.gridx = 0;
            gbc.gridy = 2;
            panel.add(genderLabel, gbc);

            // Create radio buttons for gender
            JRadioButton maleButton = new JRadioButton("Male");
            JRadioButton femaleButton = new JRadioButton("Female");
            JRadioButton otherButton = new JRadioButton("Other");
            ButtonGroup genderGroup = new ButtonGroup();
            genderGroup.add(maleButton);
            genderGroup.add(femaleButton);
            genderGroup.add(otherButton);

            JPanel genderPanel = new JPanel(new FlowLayout(FlowLayout.LEFT));
            genderPanel.setBackground(new Color(240, 248, 255)); // Match panel background
            genderPanel.add(maleButton);
            genderPanel.add(femaleButton);
            genderPanel.add(otherButton);

            gbc.gridx = 1;
            gbc.gridy = 2;
            panel.add(genderPanel, gbc);

            JLabel passwordLabel = new JLabel("Password:");
            passwordLabel.setForeground(new Color(0, 102, 204)); // Set label color
            gbc.gridx = 0;
            gbc.gridy = 3;
            panel.add(passwordLabel, gbc);

            JPasswordField passwordField = new JPasswordField();
            gbc.gridx = 1;
            gbc.gridy = 3;
            panel.add(passwordField, gbc);

            // Add the sign-up button
            JButton signUpButton = new JButton("Sign Up");
            signUpButton.setBackground(new Color(0, 102, 204)); // Button background color
            signUpButton.setForeground(Color.WHITE); // Button text color
            signUpButton.setFocusPainted(false); // Remove focus border
            gbc.gridx = 0;
            gbc.gridy = 4;
            gbc.gridwidth = 2; // Span across two columns
            gbc.fill = GridBagConstraints.CENTER;
            panel.add(signUpButton, gbc);

            // Add panel to frame
            frame.add(panel, BorderLayout.CENTER);
            frame.setVisible(true);

            // Add action listener for the button
            signUpButton.addActionListener(new ActionListener() {
                @Override
                public void actionPerformed(ActionEvent e) {
                    String name = nameField.getText();
                    String email = emailField.getText();
                    String password = new String(passwordField.getPassword());

                    String gender = null;
                    if (maleButton.isSelected()) {
                        gender = "Male";
                    } else if (femaleButton.isSelected()) {
                        gender = "Female";
                    } else if (otherButton.isSelected()) {
                        gender = "Other";
                    }

                    if (gender == null) {
                        JOptionPane.showMessageDialog(frame, "Please select a gender!");
                        return;
                    }

                    try {
                        Connection connection = DriverManager.getConnection(
                                "jdbc:mysql://localhost:3306/signup", "root", "");
                        String query = "INSERT INTO user (Name, Email, Gender, Password) VALUES (?, ?, ?, ?)";
                        PreparedStatement statement = connection.prepareStatement(query);
                        statement.setString(1, name);
                        statement.setString(2, email);
                        statement.setString(3, gender);
                        statement.setString(4, password);

                        statement.executeUpdate();
                        JOptionPane.showMessageDialog(frame, "Sign-Up Successful!");
                    } catch (Exception ex) {
                        JOptionPane.showMessageDialog(frame, "Error: " + ex.getMessage());
                    }
                }
            });
        }
    }

