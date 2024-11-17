To-Do List Project Report
1. Project Overview
The To-Do List application is a simple Java program that allows users to manage their daily tasks. Users can add tasks, mark them as completed, delete tasks, and view the list of all tasks. The project provides a basic user interface for interacting with tasks and stores the data temporarily within the program during runtime.

Key Features:

Add a new task
Display all tasks
Update a task status (mark as complete/incomplete)
Delete a task
Save tasks to a file (Optional feature)
2. Technologies Used
Java (Programming Language): The main language for the application.
Java Collections Framework: Used to store tasks dynamically (List, ArrayList).
File I/O (Optional): To store and retrieve tasks (if implementing data persistence).
Java Swing/Console: For the user interface (text-based in the console or graphical using Swing).
3. Functional Requirements
The To-Do List application must support the following features:

3.1 Task Operations
Add Task: The user can input a task description, and it will be added to the list.
View Tasks: The application will display a list of tasks with their statuses (e.g., Pending, Completed).
Update Task: The user can mark a task as completed or set it back to incomplete.
Delete Task: The user can remove a task from the list.
3.2 Task Data
Each task will have:
Task Name: A string description of the task.
Status: Either "Pending" or "Completed".
Priority (Optional): Could be a value like Low, Medium, or High.
Due Date (Optional): If a date is set, it would help the user track deadlines.
3.3 User Interface
The application will allow users to interact with the system via:
Console Input/Output: The basic way to input and view tasks.
Optional GUI: A graphical interface using Swing (buttons, text fields, etc.) for interaction.
4. Design and Architecture
The design of the To-Do List application involves a modular approach that divides the project into various components:

Task Class: Represents a task with attributes like task name, status, and due date.
ToDoList Class: Manages a collection of tasks and implements methods to add, update, delete, and view tasks.
User Interface: Either a Console or Swing-based GUI for user interaction.
File Handling (Optional): For storing and retrieving tasks from a file.
4.1 Task Class Design
The Task class models the attributes and behavior of a task. It has the following fields and methods:

java
Copy code
public class Task {
    private String taskName;
    private boolean isCompleted;
    private String dueDate; // Optional
    private String priority; // Optional

    public Task(String taskName, String dueDate, String priority) {
        this.taskName = taskName;
        this.dueDate = dueDate;
        this.priority = priority;
        this.isCompleted = false;
    }

    public String getTaskName() {
        return taskName;
    }

    public boolean isCompleted() {
        return isCompleted;
    }

    public void markAsCompleted() {
        this.isCompleted = true;
    }

    public void markAsIncomplete() {
        this.isCompleted = false;
    }

    @Override
    public String toString() {
        return taskName + " - Status: " + (isCompleted ? "Completed" : "Pending");
    }
}
4.2 ToDoList Class Design
This class manages all tasks and provides methods for adding, updating, deleting, and displaying tasks. It may look something like:

java
Copy code
import java.util.*;

public class ToDoList {
    private List<Task> tasks;

    public ToDoList() {
        tasks = new ArrayList<>();
    }

    public void addTask(String taskName, String dueDate, String priority) {
        tasks.add(new Task(taskName, dueDate, priority));
    }

    public void deleteTask(int index) {
        if (index >= 0 && index < tasks.size()) {
            tasks.remove(index);
        } else {
            System.out.println("Task not found.");
        }
    }

    public void markTaskAsCompleted(int index) {
        if (index >= 0 && index < tasks.size()) {
            tasks.get(index).markAsCompleted();
        }
    }

    public void viewTasks() {
        for (int i = 0; i < tasks.size(); i++) {
            System.out.println((i + 1) + ". " + tasks.get(i).toString());
        }
    }
}
4.3 User Interface (Console or GUI)
In this version of the project, we can start with a simple console interface and later upgrade to a GUI with Java Swing if needed.

Console Menu Example:

java
Copy code
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        ToDoList toDoList = new ToDoList();

        while (true) {
            System.out.println("\nTo-Do List Application");
            System.out.println("1. Add Task");
            System.out.println("2. View Tasks");
            System.out.println("3. Mark Task as Completed");
            System.out.println("4. Delete Task");
            System.out.println("5. Exit");

            int choice = scanner.nextInt();
            scanner.nextLine();  // Consume newline character

            switch (choice) {
                case 1:
                    System.out.print("Enter Task Name: ");
                    String taskName = scanner.nextLine();
                    System.out.print("Enter Due Date: ");
                    String dueDate = scanner.nextLine();
                    System.out.print("Enter Priority (Low/Medium/High): ");
                    String priority = scanner.nextLine();
                    toDoList.addTask(taskName, dueDate, priority);
                    break;
                case 2:
                    toDoList.viewTasks();
                    break;
                case 3:
                    System.out.print("Enter Task Number to Mark as Completed: ");
                    int taskNumber = scanner.nextInt();
                    toDoList.markTaskAsCompleted(taskNumber - 1);
                    break;
                case 4:
                    System.out.print("Enter Task Number to Delete: ");
                    int deleteNumber = scanner.nextInt();
                    toDoList.deleteTask(deleteNumber - 1);
                    break;
                case 5:
                    System.out.println("Exiting...");
                    scanner.close();
                    return;
                default:
                    System.out.println("Invalid choice.");
            }
        }
    }
}
5. Implementation Details
5.1 File Storage (Optional)
To persist tasks across sessions, the tasks can be saved to a file (e.g., using serialization or JSON format).

For instance, using file handling in Java:

java
Copy code
import java.io.*;
import java.util.*;

public class FileHandler {
    public static void saveToFile(List<Task> tasks, String filename) throws IOException {
        ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream(filename));
        out.writeObject(tasks);
        out.close();
    }

    public static List<Task> loadFromFile(String filename) throws IOException, ClassNotFoundException {
        ObjectInputStream in = new ObjectInputStream(new FileInputStream(filename));
        List<Task> tasks = (List<Task>) in.readObject();
        in.close();
        return tasks;
    }
}
6. Testing
Unit Testing: Tests can be written to verify the correctness of each method in the Task and ToDoList classes. For example, testing adding tasks, marking them completed, and deleting tasks.
Integration Testing: Test the interaction between the user interface and the task management system.
JUnit Tests Example:

java
Copy code
import org.junit.jupiter.api.*;
import static org.junit.jupiter.api.Assertions.*;

public class ToDoListTest {
    private ToDoList toDoList;

    @BeforeEach
    public void setUp() {
        toDoList = new ToDoList();
    }

    @Test
    public void testAddTask() {
        toDoList.addTask("Task 1", "2024-12-01", "High");
        assertEquals(1, toDoList.getTaskCount());
    }

    @Test
    public void testDeleteTask() {
        toDoList.addTask("Task 1", "2024-12-01", "Low");
        toDoList.deleteTask(0);
        assertEquals(0, toDoList.getTaskCount());
    }
}
7. Conclusion
The To-Do List project demonstrates the ability to handle task management using basic Java concepts such as object-oriented programming, collections, and file I/O. The application can be extended further with additional features like categorization, prioritization, deadlines, and more complex user interfaces.

The next steps would involve improving the user interface and implementing data persistence to store tasks permanently across application runs.

This project provides a solid foundation for understanding how to build and manage simple applications in Java, and it can be extended with more complex functionality as required.
