# Tonni_java_project
EmployeeManagementSystem.java
import java.io.*;
import java.util.*;

class Employee implements Serializable {
    int id;
    String name;
    String desgn;
    String gender;
    String branch;
    double sal;
    String psaddr;
    String prtaddr;
    String phone;
    String mail;

    public Employee(int id, String name, String desgn, String gender, String branch, double sal, String psaddr, String prtaddr, String phone, String mail) {
        this.id = id;
        this.name = name;
        this.desgn = desgn;
        this.gender = gender;
        this.branch = branch;
        this.sal = sal;
        this.psaddr = psaddr;
        this.prtaddr = prtaddr;
        this.phone = phone;
        this.mail = mail;
    }

    @Override
    public String toString() {
        return String.format("ID: %d\nNAME: %s\nDESIGNATION: %s\nGENDER: %s\nBRANCH: %s\nSALARY: %.2f\nPRESENT ADDRESS: %s\nPERMANENT ADDRESS: %s\nPHONE: %s\nE-MAIL: %s\n",

                              id, name, desgn, gender, branch, sal, psaddr, prtaddr, phone, mail);
    }
}

public class EmployeeManagementSystem {
    private static void printHead() {
        // Implement your header printing logic here
    }

    private static List<Employee> readEmployees(File file) {
        List<Employee> employees = new ArrayList<>();
        if (file.exists()) {
            try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream(file))) {
                while (true) {
                    Employee e = (Employee) ois.readObject();
                    employees.add(e);
                }
            } catch (EOFException eof) {
                // End of file reached
            } catch (IOException | ClassNotFoundException e) {
                e.printStackTrace();
            }
        }
        return employees;
    }

    private static void writeEmployees(File file, List<Employee> employees) {
        try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(file))) {
            for (Employee e : employees) {
                oos.writeObject(e);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    private static void addEmployee(File file) {
        List<Employee> employees = readEmployees(file);
        Scanner scanner = new Scanner(System.in);

        System.out.print("Enter ID: ");
        int id = scanner.nextInt();
        scanner.nextLine(); // consume newline

        System.out.print("Enter Name: ");
        String name = scanner.nextLine();

        System.out.print("Enter Designation: ");
        String desgn = scanner.nextLine();

        System.out.print("Enter Gender: ");
        String gender = scanner.nextLine();

        System.out.print("Enter Branch: ");
        String branch = scanner.nextLine();

        System.out.print("Enter Salary: ");
        double sal = scanner.nextDouble();
        scanner.nextLine(); // consume newline

        System.out.print("Enter Present Address: ");
        String psaddr = scanner.nextLine();

        System.out.print("Enter Permanent Address: ");
        String prtaddr = scanner.nextLine();

        System.out.print("Enter Phone: ");
        String phone = scanner.nextLine();

        System.out.print("Enter E-Mail: ");
        String mail = scanner.nextLine();

        Employee e = new Employee(id, name, desgn, gender, branch, sal, psaddr, prtaddr, phone, mail);
        employees.add(e);

        writeEmployees(file, employees);

        System.out.println("Employee added successfully!");
    }

    private static void displayList(File file) {
        printHead();
        System.out.println("\n\t\t\t\t\t\t\t\t\t List of Employees");
        System.out.println("\t\t\t\t\t\t\t\t      -----------------------");
        List<Employee> employees = readEmployees(file);
        for (Employee e : employees) {
            System.out.println(e);
            System.out.println("\t\t\t\t\t\t\t     ===============================================");
        }
        System.out.println("\n\n\n\t");
        System.out.println("\n\n\t\t\t\t\t\t\t     Enter any key to continue...");
        try (Scanner tempScanner = new Scanner(System.in)) {
            tempScanner.nextLine();
        }
    }

    private static void searchRecord(File file) {
        Scanner scanner = new Scanner(System.in);
        char another = 'y';

        while (another == 'y' || another == 'Y') {
            System.out.println("\n\t\t\t\t\t\t\t\t\tSearch Employee");
            System.out.println("\t\t\t\t\t\t\t\t     ---------------------");
            System.out.print("\n\n\t\t\t\t\t\t       Enter ID Number of Employee to search the record: ");
            int tempid = scanner.nextInt();

            boolean found = false;
            List<Employee> employees = readEmployees(file);
            for (Employee e : employees) {
                if (e.id == tempid) {
                    System.out.println(e);
                    System.out.println("\t\t\t\t\t\t\t\t========================");
                    found = true;
                    break;
                                    
                }
            }

            if (!found) {
                System.out.println("\n\n\t\t\t\t\t\t Record Not Found...!");
            }

            System.out.print("\n\n\t\t\t\t\t\tDo you want to search another record? (y/n): ");
            another = scanner.next().charAt(0);
        }
    }

    public static void main(String[] args) {
        File file = new File("employees.dat");
        try (Scanner scanner = new Scanner(System.in)) {
            int choice;

            do {
                System.out.println("\n========= Employee Management System =========");
                System.out.println("1. Add Employee");
                System.out.println("2. Display All Employees");
                System.out.println("3. Search Employee by ID");
                System.out.println("4. Exit");
                System.out.print("Enter your choice: ");
                choice = scanner.nextInt();

                switch (choice) {
                    case 1:
                        addEmployee(file);
                        break;
                    case 2:
                        displayList(file);
                        break;
                    case 3:
                        searchRecord(file);
                        break;
                    case 4:
                        System.out.println("Exiting... Goodbye!");
                        break;
                    default:
                        System.out.println("Invalid choice! Please try again.");
                }
            } while (choice != 4);
        }
    }
}
