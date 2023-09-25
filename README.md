# JavaFX Components Setup Guide

This guide demonstrates how to set up and use various JavaFX components, including TableView, ListView, TreeView, ColorPicker, DatePicker, FileChooser, and ProgressBar.

## Prerequisites

Make sure you have Java JDK and JavaFX SDK properly installed and configured.
> [Click here to set up JavaFX on Eclipse.](https://youtu.be/_7OM-cMYWbQ?si=3zCQW_okQbDViw5i)

> [Click here to set up JavaFX on IntelliJ.](https://youtu.be/Ope4icw6bVk?si=2Yvw1qQecSq64Bma)

# Components 

* ## TableView
### Import Statements for TableView

```java
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.*;
import javafx.stage.Stage;
import javafx.collections.FXCollections;
import javafx.collections.ObservableList;
import javafx.scene.control.cell.PropertyValueFactory;
```

### Initialize TableView and Define Data Model
Assuming you defined a class `Student` in your src folder:
```java
// Initialize TableView
TableView<Student> tableView = new TableView<>();

// Define a data model containing Object instances
ObservableList<Student> students = FXCollections.observableArrayList();
students.add(new Student("John", "Doe", 25));
students.add(new Student("Alice", "Smith", 22));

// Define table columns and set Cell Value Factory
TableColumn<Student, String> firstNameColumn = new TableColumn<>("First Name");
firstNameColumn.setCellValueFactory(new PropertyValueFactory<>("firstName"));

TableColumn<Student, String> lastNameColumn = new TableColumn<>("Last Name");
lastNameColumn.setCellValueFactory(new PropertyValueFactory<>("lastName"));

TableColumn<Student, Integer> ageColumn = new TableColumn<>("Age");
ageColumn.setCellValueFactory(new PropertyValueFactory<>("age"));

// Add columns to the table
tableView.getColumns().addAll(firstNameColumn, lastNameColumn, ageColumn);
tableView.setItems(students);
```
> **_Q: Why do I need to set up Cell Value Factory for each TableColumn?_**
> 
> **A:** When you're using a TableView in JavaFX, you need to set up the Cell Value Factory for each TableColumn so that the table knows how to display data from your data source (like a list or database). 
> 
> Think of it as telling the table where to find the values for each column.
> 
> Without this setup, the table won't know what to show in each cell, and your data won't display correctly. So, it's like giving the table a map to find its way to the data!

* ## ListView
### Import Statements for ListView
```java
import javafx.scene.control.ListView;
import javafx.collections.FXCollections;
import javafx.collections.ObservableList;
```

### Initialize ListView
```java 
ListView<String> listView = new ListView<>();
ObservableList<String> items = FXCollections.observableArrayList("Item 1", "Item 2", "Item 3");
listView.setItems(items);
```
Alternatively, you could populate the ListView with Objects: 
```java
// Assuming you defined an Object Person(String name, Set<String> responsibilities)
Person nour = new Person("Nour", nourResponsibilities);
Person kevin = new Person("Kevin", kevinResponsibilities);
Person salim = new Person("Salim", salimResponsibilities);

// Populate a ListView and add Person objects to it
listView = new ListView<>();
ObservableList<Person> people = FXCollections.observableArrayList(nour, kevin, salim);
listView.setItems(people);
```

* ## TreeView
### Import Statements for TreeView
```java
import javafx.scene.control.TreeView;
import javafx.scene.control.TreeItem;
import javafx.scene.control.cell.TextFieldTreeCell;
import javafx.scene.control.cell.TreeItemPropertyValueFactory;
import javafx.scene.input.MouseEvent;
```

### Initialize TreeView
```java
// Initialize TreeView
TreeView<String> treeView = new TreeView<>();

// Initialize root node
TreeItem<String> rootItem = new TreeItem<>("Root Node");

// Initialize Children and link them to root
rootItem.getChildren().addAll(new TreeItem<>("Child 1"), new TreeItem<>("Child 2"));
treeView.setRoot(rootItem);
```
To edit values inside a TreeView Object:
```java
// Enable editing of tree items
treeView.setEditable(true);
// Creates a cell that displays a text field to edit the label of a tree item
treeView.setCellFactory(TextFieldTreeCell.forTreeView());
// Update value with new value from user input
treeView.setOnEditCommit(event -> {
    TreeItem<String> editedItem = event.getTreeItem();
    editedItem.setValue(event.getNewValue());
});
```

* ## ColorPicker
### Import Statements for ColorPicker
```java
import javafx.scene.control.ColorPicker;
import javafx.scene.paint.Color;
```

### Initialize ColorPicker
```java
// Initialize ColorPicker
ColorPicker colorPicker = new ColorPicker();
colorPicker.setValue(Color.RED);
colorPicker.setOnAction(event -> {
    Color selectedColor = colorPicker.getValue();
    // Handle the selected color
    // Ex: Assuming you defined a Class Course in src folder
    Course course1 = new Course();
    course1.setColor(selectedColor);
});
```

* ## DatePicker
### Import Statements for ColorPicker
```java
import javafx.scene.control.DatePicker;
import java.time.LocalDate;
```

### Initialize DatePicker
```java
// Initialize DatePicker
DatePicker datePicker = new DatePicker();
datePicker.setOnAction(event -> {
    LocalDate selectedDate = datePicker.getValue();
    // Handle the selected date
});
```

* ## FileChooser
### Import Statements for FileChooser
```java
import javafx.stage.FileChooser;
import java.io.File;
```    

### Initialize FileChooser
```java
// Initialize FileChooser
FileChooser fileChooser = new FileChooser();
// Label for the button
Button uploadButton = new Button("Upload Files");
    {
    HBox hbox = new HBox(uploadButton);
    hbox.setSpacing(5);
    setGraphic(hbox);

    uploadButton.setOnAction(event -> {
        // Assuming you already defined a Class Course in src folder
        Course course = new Course();
        // Open the "Select Files" window on the primary stage
        File selectedFile = fileChooser.showOpenDialog(primaryStage);
        // Example
        if (selectedFile != null) {
            course.getFiles().addAll(selectedFile);
        }
    });
    }
```

* ## Progress Bar
### Import Statements for ProgressBar
```java
import javafx.scene.control.ProgressBar;
import javafx.animation.KeyFrame;
import javafx.animation.Timeline;
import javafx.util.Duration;
```

### Initialize ProgressBar
```java
// Initialize ProgressBar
ProgressBar progressBar = new ProgressBar();
progressBar.setProgress(0.5); // Set the initial progress (0.0 to 1.0)
        
// Change progress color
progressBar.getStyleClass().add("custom-progress-bar");

/* CustomProgress.css in src folder */
.custom-progress-bar .bar {
-fx-background-color: #00FF00; /* Green color */
}
```

To create a sample Timeline to simulate progress: 
```java
Duration duration = Duration.seconds(5); // Total duration of the progress simulation
int numKeyFrames = 100; // Number of keyframes (steps) to complete the progress
double increment = 1.0 / numKeyFrames; // Increment to update the progress
// Create a sequence of `KeyFrame` objects that represent the progress at each step of the simulation. 
// The progress value should be incremented gradually.
KeyFrame[] keyFrames = new KeyFrame[numKeyFrames];
for (int i = 0; i < numKeyFrames; i++) {
    Duration frameTime = duration.multiply((double) i / numKeyFrames);
    keyFrames[i] = new KeyFrame(frameTime, event -> {
        double newProgress = progressBar.getProgress() + increment;
        progressBar.setProgress(newProgress);
});
}
// Create a Timeline and add the keyframes to it. 
// Then, play the timeline to simulate the progress
Timeline timeline = new Timeline(keyFrames);
timeline.setCycleCount(1); // Run the timeline once
timeline.play();
```

* ## Pagination
### Import Statements for Pagination
```java
import javafx.scene.Scene;
```

### Initialize Pagination
```java
// Create Pagination with 4 pages
Pagination pagination = new Pagination(4);
pagination.setPageFactory(this::createPage);

BorderPane root = new BorderPane();
// Set pagination at the center of the displayed root on stage
root.setCenter(pagination);
// Outside the scope of start() method, inside Application()
private VBox createPage(int pageIndex) {
    VBox pageContent = new VBox();
    pageContent.setSpacing(10);

    if (pageIndex == 0) {
    // Page 1: ListView
    pageContent.getChildren().addAll(listView);

    } else if (pageIndex == 1) {
    // Page 2: TableView
    pageContent.getChildren().addAll(tableView);
    } else if (pageIndex == 2) {
    // Page 3: TreeView
    pageContent.getChildren().addAll(treeView, datePickerBox);
    } else if (pageIndex == 3){
    // Page 4: HTMLEditor for HTML Manipulation
    HTMLEditor htmlEditor = new HTMLEditor();
    pageContent.getChildren().addAll(htmlEditor);
    }

    return pageContent;
    }
```

* ## HTML Editor
### Import Statements for HTML Editor
```java
import javafx.scene.web.HTMLEditor;
```

### Initialize an HTML Editor
```java
VBox pageContent = new VBox();
HTMLEditor htmlEditor = new HTMLEditor();
pageContent.getChildren().addAll(htmlEditor);
```

---

# Displaying Components
### Create a JavaFX Application
```java
public class MainApp extends Application {
    @Override
    public void start(Stage primaryStage) {
        // Create layouts to display components
        VBox vBox = new VBox(tableView, listView, treeView, colorPicker, datePicker, progressBar);
        Scene scene = new Scene(vBox, 800, 600);
        
        // Set the scene and show the stage
        primaryStage.setScene(scene);
        primaryStage.setTitle("JavaFX Components Example");
        primaryStage.show();
    }
    
    public static void main(String[] args) {
        launch(args);
    }
}
```
--- 

> Look at you, you've come this far! </br>
> Here, have a cookie 🍪