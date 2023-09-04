import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.ArrayList;
import java.util.List;

class Student {
    private String name;
    private int rollNumber;
    private String grade;

    public Student(String name, int rollNumber, String grade) {
        this.name = name;
        this.rollNumber = rollNumber;
        this.grade = grade;
    }

    public String getName() {
        return name;
    }

    public int getRollNumber() {
        return rollNumber;
    }

    public String getGrade() {
        return grade;
    }

    @Override
    public String toString() {
        return "Name: " + name + ", Roll Number: " + rollNumber + ", Grade: " + grade;
    }
}

class StudentManagementSystemGUI extends JFrame {
    private List<Student> students;
    private JLabel nameLabel;
    private JTextField nameField;
    private JLabel rollNumberLabel;
    private JTextField rollNumberField;
    private JLabel gradeLabel;
    private JTextField gradeField;
    private JButton addButton;
    private JButton removeButton;
    private JButton searchButton;
    private JTextArea outputArea;

    public StudentManagementSystemGUI() {
        setTitle("Student Management System");
        setSize(400, 400);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLayout(new FlowLayout());

        students = new ArrayList<>();

        nameLabel = new JLabel("Name:");
        add(nameLabel);
        nameField = new JTextField(15);
        add(nameField);

        rollNumberLabel = new JLabel("Roll Number:");
        add(rollNumberLabel);
        rollNumberField = new JTextField(5);
        add(rollNumberField);

        gradeLabel = new JLabel("Grade:");
        add(gradeLabel);
        gradeField = new JTextField(5);
        add(gradeField);

        addButton = new JButton("Add");
        add(addButton);

        removeButton = new JButton("Remove");
        add(removeButton);

        searchButton = new JButton("Search");
        add(searchButton);

        outputArea = new JTextArea(15, 30);
        add(outputArea);

        addButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                addStudent();
            }
        });

        removeButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                removeStudent();
            }
        });

        searchButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                searchStudent();
            }
        });
    }

    private void addStudent() {
        String name = nameField.getText();
        String rollNumberText = rollNumberField.getText();
        String grade = gradeField.getText();

        if (name.isEmpty() || rollNumberText.isEmpty() || grade.isEmpty()) {
            JOptionPane.showMessageDialog(this, "Please fill in all fields.", "Error", JOptionPane.ERROR_MESSAGE);
            return;
        }

        int rollNumber;
        try {
            rollNumber = Integer.parseInt(rollNumberText);
        } catch (NumberFormatException e) {
            JOptionPane.showMessageDialog(this, "Invalid roll number.", "Error", JOptionPane.ERROR_MESSAGE);
            return;
        }

        Student student = new Student(name, rollNumber, grade);
        students.add(student);

        outputArea.setText("Student added:\n" + student.toString());
        clearFields();
    }

    private void removeStudent() {
        String rollNumberText = rollNumberField.getText();

        if (rollNumberText.isEmpty()) {
            JOptionPane.showMessageDialog(this, "Please enter the roll number.", "Error", JOptionPane.ERROR_MESSAGE);
            return;
        }

        int rollNumber;
        try {
            rollNumber = Integer.parseInt(rollNumberText);
        } catch (NumberFormatException e) {
            JOptionPane.showMessageDialog(this, "Invalid roll number.", "Error", JOptionPane.ERROR_MESSAGE);
            return;
        }

        Student studentToRemove = null;
        for (Student student : students) {
            if (student.getRollNumber() == rollNumber) {
                studentToRemove = student;
                break;
            }
        }

        if (studentToRemove != null) {
            students.remove(studentToRemove);
            outputArea.setText("Student removed:\n" + studentToRemove.toString());
        } else {
            outputArea.setText("Student not found.");
        }

        clearFields();
    }

    private void searchStudent() {
        String rollNumberText = rollNumberField.getText();

        if (rollNumberText.isEmpty()) {
            JOptionPane.showMessageDialog(this, "Please enter the roll number.", "Error", JOptionPane.ERROR_MESSAGE);
            return;
        }

        int rollNumber;
        try {
            rollNumber = Integer.parseInt(rollNumberText);
        } catch (NumberFormatException e) {
            JOptionPane.showMessageDialog(this, "Invalid roll number.", "Error", JOptionPane.ERROR_MESSAGE);
            return;
        }

        Student foundStudent = null;
        for (Student student : students) {
            if (student.getRollNumber() == rollNumber) {
                foundStudent = student;
                break;
            }
        }

        if (foundStudent != null) {
            outputArea.setText("Student found:\n" + foundStudent.toString());
        } else {
            outputArea.setText("Student not found.");
        }

        clearFields();
    }

    private void clearFields() {
        nameField.setText("");
        rollNumberField.setText("");
        gradeField.setText("");
    }
}

public class StudentManagementSystem {
    public static void main(String[] args) {
        SwingUtilities.invokeLater(new Runnable() {
            @Override
            public void run() {
                StudentManagementSystemGUI gui = new StudentManagementSystemGUI();
                gui.setVisible(true);
            }
        });
    }
}
