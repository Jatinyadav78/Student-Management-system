import java.io.*;
import java.util.*;

public class StudentManagementSystem {
    private static final String FILE_NAME = "students.txt";
    private Map<String, Student> studentMap;

    public StudentManagementSystem() {
        studentMap = new HashMap<>();
        loadFromFile();
    }

    public void addStudent(Student student) {
        if (!studentMap.containsKey(student.getRollNumber())) {
            studentMap.put(student.getRollNumber(), student);
            System.out.println("Student added successfully.");
        } else {
            System.out.println("A student with this roll number already exists.");
        }
    }

    public void removeStudent(String rollNumber) {
        if (studentMap.remove(rollNumber) != null) {
            System.out.println("Student removed successfully.");
        } else {
            System.out.println("Student with this roll number not found.");
        }
    }

    public void searchStudent(String rollNumber) {
        Student student = studentMap.get(rollNumber);
        if (student != null) {
            System.out.println(student);
        } else {
            System.out.println("Student with this roll number not found.");
        }
    }

    public void displayAllStudents() {
        if (studentMap.isEmpty()) {
            System.out.println("No students available.");
        } else {
            for (Student student : studentMap.values()) {
                System.out.println(student);
            }
        }
    }

    public void saveToFile() {
        try (BufferedWriter writer = new BufferedWriter(new FileWriter(FILE_NAME))) {
            for (Student student : studentMap.values()) {
                writer.write(student.getName() + "," + student.getRollNumber() + "," + student.getGrade());
                writer.newLine();
            }
        } catch (IOException e) {
            System.out.println("Error saving to file: " + e.getMessage());
        }
    }

    public void loadFromFile() {
        try (BufferedReader reader = new BufferedReader(new FileReader(FILE_NAME))) {
            String line;
            while ((line = reader.readLine()) != null) {
                String[] parts = line.split(",");
                if (parts.length == 3) {
                    Student student = new Student(parts[0], parts[1], parts[2]);
                    studentMap.put(parts[1], student);
                }
            }
        } catch (IOException e) {
            System.out.println("Error loading from file: " + e.getMessage());
        }
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        StudentManagementSystem sms = new StudentManagementSystem();
        boolean exit = false;

        while (!exit) {
            System.out.println("\nStudent Management System");
            System.out.println("1. Add Student");
            System.out.println("2. Remove Student");
            System.out.println("3. Search Student");
            System.out.println("4. Display All Students");
            System.out.println("5. Exit");
            System.out.print("Enter your choice: ");

            int choice = getValidIntInput(scanner);

            switch (choice) {
                case 1:
                    addStudent(scanner, sms);
                    break;
                case 2:
                    removeStudent(scanner, sms);
                    break;
                case 3:
                    searchStudent(scanner, sms);
                    break;
                case 4:
                    sms.displayAllStudents();
                    break;
                case 5:
                    sms.saveToFile();
                    System.out.println("Exiting. Data saved.");
                    exit = true;
                    break;
                default:
                    System.out.println("Invalid choice. Please try again.");
                    break;
            }
        }
        scanner.close();
    }

    private static int getValidIntInput(Scanner scanner) {
        while (!scanner.hasNextInt()) {
            System.out.println("Invalid input. Please enter a number.");
            scanner.next(); // Clear invalid input
        }
        return scanner.nextInt();
    }

    private static void addStudent(Scanner scanner, StudentManagementSystem sms) {
        System.out.print("Enter student name: ");
        String name = scanner.next();
        System.out.print("Enter student roll number: ");
        String rollNumber = scanner.next();
        System.out.print("Enter student grade: ");
        String grade = scanner.next();
        Student student = new Student(name, rollNumber, grade);
        sms.addStudent(student);
    }

    private static void removeStudent(Scanner scanner, StudentManagementSystem sms) {
        System.out.print("Enter roll number of student to remove: ");
        String rollNumber = scanner.next();
        sms.removeStudent(rollNumber);
    }

    private static void searchStudent(Scanner scanner, StudentManagementSystem sms) {
        System.out.print("Enter roll number of student to search: ");
        String rollNumber = scanner.next();
        sms.searchStudent(rollNumber);
    }
}
