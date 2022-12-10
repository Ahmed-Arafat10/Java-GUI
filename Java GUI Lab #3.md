# GUI Lab #3
> _**By TA Ahmed Arafat ^-^**_

### Topics To Be Discussed:

1. New syntax of `ActionListener` & Performing click `Event`
2. Better way of creating `JFrame` objects
3. Code Re-usability
4. `isempty()` method with `textfield`
5. Getting data from `JRadioButton`/`JCheckBox` Using `isSelected()` method
6. Creating a Table in `PhpMyAdmin`
7. Dropdown list using `JComboBox` class 
8. Send data from one `JFrame` object to another 
9. Comprehensive example so far 
10. Perform `CRUD` operations on `MySQL Database`
11. Get a single record (row) from `MySQL Database`
12. Get multiple records (rows) from `MySQL Database`

### 1. New syntax of `ActionListener` & Performing click `Event`
- We have learned in the previous lab that we can use `ActionListener` interface
by `implemeting` its function `public void actionPerformed(ActionEvent e)`
Or by creating an `Inner Class` that `implemets` it
- But we can use an easier syntax by using `Lambda Syntax` as in the following code :
````java
 SignInBTN.addActionListener((ActionEvent e)->
    {
        HomePage.dispose(); 
        WelcomePage();
    });
````
- Or you can use `Anonymous Class` Syntax, like this:
````java
SignInBTN.addActionListener(new AcionListener() {
        @Override
        public void actionPerformed (ActionEvent e){
            home.dispose();
            welcome();
        }
    });
````

### 2. Better way of creating `JFrame` objects
- Previously we have created all our components in a single `JFrame` Object
, but now for each Page (`Method`) we will create a separate `GUI` object (it contains all `JFrame` methods as class `GUI` `extends` from `JFrame`)
like this :
````java
class GUI extends JFrame {
  public GUI() {
  }

  public GUI(String title) {
    setTitle(title);
    setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    setLocation(200, 200);
    setVisible(true);
  }
  
  public void HomePage() {
    GUI home = new GUI("Home");
    home.dispose();
    welwome();
  }
}
````
> Note: We have created a `constructor` that set basic property of `JFrame`, this style is used rather
typing these statements every single time while creating an object from class `JFrame`

> Also Note: We created a default constructor because we have created an object from
`JFrame` in `main()` function, don't forget this is our starting point when the program runs
````java
public class Main {
    public static void main(String args[]) {
        GUI Obj1 = new GUI();
        Obj1.HomePage(); // Calling the home page (our start point)
    }
}
```` 
### 3- Code Re-usability
- During developing our project, some statement will be used many times, so it is
better to try to shorten the code that is repeated many times, for example
statement like `JOptionPane.showMessageDialog(null,text);` to show a message to the user, this function
will be called many times ,so it is better to create a function called
`PrintMess(String text)` or any other name in our `class`, and pass a text parameter to that function
then printing this text in a message like this :
````java
    public void PrintMess(String text)
    {
        JOptionPane.showMessageDialog(null,text);
    }
````
> Now all what we have to do is to call `PrintMess("Your Account Is Created Successfully")` with desired text 

> Code Re-usability is achieved in this case :)

### 4. `isEmpty()` method with `JTextField` objects
- We have always to validate the data, we have to check first is the 
`JTextField` object if empty or not, observe the following code :
````java
ClickMeButton.addActionListener((ActionEvent e)->
    {
        String name, gender;
        if (nameI.getText().isEmpty()) PrintMess("Text field cannot be empty");
        else {
        name = nameI.getText();
        }
    });
````

### 5. Get data from `JRadioButton`/`JCheckBox` Using `isSelected()` method

````java
btn1.addActionListener((ActionEvent e) -> {
            String name,gender;
            if(nameI.getText().isEmpty()) home.PrintMess("Text field cannot be empty");
            else if(!maleRadio.isSelected() && !femaleRadio.isSelected()) PrintMess("please enter your gender");
            else {
                name = nameI.getText();
                gender = maleRadio.isSelected() ? "male" : "female";
                }
        });
````
> `!maleRadio.isSelected() && !femaleRadio.isSelected()` check if both radiobuttons are not selected, then we have to print a message to the user to select any one of them

> `gender = maleRadio.isSelected() ? "male" : "female";` is called trinary operator, it is used instead of writing this:
````java
if(maleRadio.isSelected())
gender = "Male";
else
gender = "Female";
````

### 6. Creating a Table in `PhpMyAdmin`


### 7. dropdown list using `JComboBox` class
- To create a `JComboBox` object, we have to pass an `array` to its
`constructor`, so we  will create first an `array` wih desired values
````java
String[] PetsArr = { "Bird", "Cat", "Dog", "Rabbit", "Pig" };
JComboBox PetComboBox = new JComboBox(PetsArr);
PetComboBox.setSelectedIndex(1);
PetComboBox.setBounds(300,400,100,25);
p1.add(PetComboBox);
````


````java
ClickMeButton.addActionListener((ActionEvent e)->
    {
        String SelectedItem = PetComboBox.getSelectedItem().toString();
        System.out.println(SelectedItem);
    });
````
> `getSelectedItem()` method return an object with selected item, to convert it to a string we use `toString()`


### 8. Send data from one `JFrame` object to another
- We can observe that in the following code, we have created  a method called `WelcomePage(String name,String gender)`, but this time we have passed two `parameters`
first one is `name` & other is `gender` this parameters will be sent when we call this `Method`
, so we can print their values in `WelcomePage` like this : 
````java
public void WelcomePage(String name,String gender)
    {
        GUI Welcome = new GUI("Welcome");// see we have created an object
        // then printing the values that come from Home page
        PrintMess("Your name is " + name +", your gender is " + gender);
    }
````

### 9. Comprehensive example so far
- Now Lets see All codes we have written so far :

````java
package GUI;

import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.*;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;
import java.sql.*;

class DBConnetion {
    private static String name = "root";
    private static String password = "";
    private static String add = "jdbc:mysql://localhost:3306/my_cms";
    static String query;
    static Statement stmt;

    public Connection Connect() throws SQLException {
        Connection con = DriverManager.getConnection(add, name, password);
        return con;
    }
}

class GUI extends JFrame {

    public GUI() {
    }

    public GUI(String title) {
        setTitle(title);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocation(200, 200);
        setVisible(true);
        setSize(500, 500);
    }

    public void PrintMess(String text) {
        JOptionPane.showMessageDialog(null, text);
    }

    public void HomePage() {
        JTextField nameI = new JTextField();
        JButton btn1 = new JButton("Click Me");
        ButtonGroup Group1 = new ButtonGroup();
        JRadioButton maleRadio = new JRadioButton("Male");
        JRadioButton femaleRadio = new JRadioButton("Female");
        JCheckBox checkBox1 = new JCheckBox("dd");
        String[] petStrings = {"Bird", "Cat", "Dog", "Rabbit", "Pig"};
        JComboBox petlist = new JComboBox(petStrings);
        petlist.setSelectedIndex(1);
        Group1.add(maleRadio);
        Group1.add(femaleRadio);
        maleRadio.setBounds(200, 200, 100, 50);
        femaleRadio.setBounds(300, 200, 100, 50);
        checkBox1.setBounds(300, 300, 100, 50);
        petlist.setBounds(300, 400, 100, 25);
        GUI home = new GUI("Home");
        JPanel p1 = new JPanel();
        p1.setBackground(Color.RED);
        p1.setLayout(null);
        home.add(p1);
        btn1.setBounds(50, 100, 100, 50);
        nameI.setBounds(50, 50, 100, 50);
        p1.add(nameI);
        p1.add(btn1);
        p1.add(maleRadio);
        p1.add(femaleRadio);
        p1.add(checkBox1);
        p1.add(petlist);
        btn1.addActionListener((ActionEvent e) -> {
            String SelectedItem = petlist.getSelectedItem().toString();
            System.out.println(SelectedItem);
            String name, gender;
            if (nameI.getText().isEmpty()) home.PrintMess("Text field cannot be empty");
            else if (!maleRadio.isSelected() && !femaleRadio.isSelected()) home.PrintMess("please enter your gender");
            else {
                name = nameI.getText();
                gender = maleRadio.isSelected() ? "male" : "female";
                home.dispose();
                WelcomePage(name, gender);
            }
        });

    }

    public void WelcomePage(String name, String gender) {
        GUI Welcome = new GUI("Welcome");
        PrintMess("Your name is " + name + ", your gender is " + gender);
    }
}

public class Main {

    public static void main(String args[]) throws SQLException {
        GUI obj1 = new GUI();
        obj1.HomePage();
    }
}
````

> `VIP Note` : Always try to write statements of creating object from their corresponding classes (`ClassName ObjName = new ClassName()`), in thw first lines of each function


### 10. Perform `CRUD` operations on `MySQL Database`
- Perform `Insert`operation :
````java
DBConnetion DB = new DBConnetion();
Connection Con = DB.Connect();
String Name = "Ahmed Arafat";
String Query = "INSERT INTO photos values (null,'"+Name+"',1,'app',null,null)";
DBConnetion.stmt = Con.prepareStatement(Query);
DBConnetion.stmt.execute(Query);
````
> Don't forget you must add string values that are going to be `inserted`/`updated` in database in single quote `'value'`

-  Let's create `Sign In Page` to practice `Insert` command :
````java
class GUI extends JFrame {
    DBConnetion MySQL_DB = new DBConnetion();
    Connection Con;

    public GUI() throws SQLException {
        Con = MySQL_DB.Connect();
    }

    public GUI(String title) {
        setTitle(title);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocation(200, 200);
        setVisible(true);
        setSize(500, 500);
    }

    public void PrintMess(String text) {
        JOptionPane.showMessageDialog(null, text);
    }

    public void HomePage() {
        JPanel p1 = new JPanel();
        JTextField NameField = new JTextField();
        JPasswordField PasswordField = new JPasswordField();
        JPasswordField PasswordFieldConfirm = new JPasswordField();
        JButton SignInButton = new JButton("Click Me");
        ButtonGroup RadioGroup1 = new ButtonGroup();
        JRadioButton MaleRadio = new JRadioButton("Male");
        JRadioButton FemaleRadio = new JRadioButton("Female");

        RadioGroup1.add(MaleRadio);
        RadioGroup1.add(FemaleRadio);

        MaleRadio.setBounds(120, 300, 100, 30);
        FemaleRadio.setBounds(220, 300, 100, 30);
        SignInButton.setBounds(150, 400, 100, 30);
        NameField.setBounds(150, 50, 150, 30);
        PasswordField.setBounds(150, 120, 150, 30);
        PasswordFieldConfirm.setBounds(150, 190, 150, 30);

        GUI home = new GUI("Home");
        p1.setBackground(Color.RED);
        p1.setLayout(null);
        home.add(p1);

        p1.add(NameField);
        p1.add(SignInButton);
        p1.add(MaleRadio);
        p1.add(FemaleRadio);
        p1.add(PasswordField);
        p1.add(PasswordFieldConfirm);

        SignInButton.addActionListener((ActionEvent e) -> {
            String Name, Password = String.valueOf(PasswordField.getPassword()), Confpass = String.valueOf(PasswordFieldConfirm.getPassword()), Gender;
            if (NameField.getText().isEmpty()) PrintMess("Text field cannot be empty");
            else if (PasswordField.getPassword().length == 0) PrintMess("password cannot be empty");
            else if (PasswordFieldConfirm.getPassword().length == 0) PrintMess("password cannot be empty");
            else if (!Password.equals(Confpass)) PrintMess("Walahy ?!");
            else if (!MaleRadio.isSelected() && !FemaleRadio.isSelected()) PrintMess("please enter your gender");
            else {
                Name = NameField.getText();
                Gender = MaleRadio.isSelected() ? "Male" : "Female";
                String Query = "INSERT INTO user VALUES (null,'" + Name + "','" + Password + "','" + Gender + "')";
                try {
                    DBConnetion.stmt = Con.prepareStatement(Query);
                } catch (SQLException ex) {
                    throw new RuntimeException(ex);
                }
                try {
                    DBConnetion.stmt.execute(Query);
                } catch (SQLException ex) {
                    throw new RuntimeException(ex);
                }
                PrintMess("Your Account Is Created");
            }
        });
    }

    public void WelcomePage(String name, String gender) {
        GUI Welcome = new GUI("Welcome");
        PrintMess("Your name is " + name + ", your gender is " + gender);
    }
}
````
> We have to observe some statements :

- As we will interact with `MySQL Database` in almost all the pages, we will use `DBConnetion` class that is created by us many times
, so rather creating an object from it in each `method` , we just create an object from it just one time
````java
DBConnetion MySQL_DB = new DBConnetion();
Connection Con;
````
- You can observe from above code that we created an object from class `Connection` without assigning to it any object, so we have just declared an object from it,
but in `default constructor`, we have assigned it with object that returns from function `Connect()` from object `MySQL_DB`
, Also we have added `throws SQLException` in the header of the constructor as when we interact with `MySQL Database` we have always
to `Throw Execptions` that indicates the failure of executing such command, for example if your database in called `cic` but you have specified that Database name is `cic2` then it will `Throw an Execption`  
````java
 public GUI() throws SQLException {
        Con = MySQL_DB.Connect();
    }
````
- Note that both `DBConnetion.stmt = Con.prepareStatement(Query);` & `DBConnetion.stmt.execute(Query);` statements must be added in `try/catch blocks` 
````java
String Query = "INSERT INTO user VALUES (null,'" + Name + "','" + Password + "','" + Gender + "')";
    try {
        DBConnetion.stmt = Con.prepareStatement(Query);
    } catch (SQLException ex) {
        throw new RuntimeException(ex);
                }
                try {
                    DBConnetion.stmt.execute(Query);
                } catch (SQLException ex) {
                    throw new RuntimeException(ex);
                }
````
> VIP note: same concept is applied for `update`,`delete` statements


### 11. Get a single record (row) from `MySQL Database`

````java
class GUI extends JFrame {
DBConnetion MySQL_DB = new DBConnetion();
Connection Con;

    public GUI() throws SQLException {
        Con = MySQL_DB.Connect();
    }

    public GUI(String title) {
        setTitle(title);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocation(200, 200);
        setVisible(true);
        setSize(500, 500);
    }

    public void PrintMess(String text) {
        JOptionPane.showMessageDialog(null, text);
    }

    public void HomePage() {
        JPanel p1 = new JPanel();
        JTextField NameField = new JTextField();
        JPasswordField PasswordField = new JPasswordField();
        JButton SignInButton = new JButton("Click Me");
        ButtonGroup RadioGroup1 = new ButtonGroup();
        JRadioButton MaleRadio = new JRadioButton("Male");
        JRadioButton FemaleRadio = new JRadioButton("Female");

        MaleRadio.setBounds(120, 300, 100, 30);
        FemaleRadio.setBounds(220, 300, 100, 30);
        SignInButton.setBounds(150, 400, 100, 30);
        NameField.setBounds(150, 50, 150, 30);
        PasswordField.setBounds(150, 120, 150, 30);

        GUI home = new GUI("Home");
        p1.setBackground(Color.RED);
        p1.setLayout(null);
        home.add(p1);

        p1.add(NameField);
        p1.add(SignInButton);
        p1.add(PasswordField);

        SignInButton.addActionListener((ActionEvent e) -> {
            String Name, Password = String.valueOf(PasswordField.getPassword());
            if (NameField.getText().isEmpty()) PrintMess("Text field cannot be empty");
            else {
                Name = NameField.getText();
                String Query = "Select * FROM `user` WHERE `name` = '" + Name + "' AND `password` = '" + Password + "' ";
                try {
                    DBConnetion.stmt = Con.prepareStatement(Query);
                } catch (SQLException ex) {
                    throw new RuntimeException(ex);
                }
                try {
                    ResultSet r = DBConnetion.stmt.executeQuery(Query);
                    //System.out.println(r.getRow());
                    if (r.next()) {
                        int id = r.getInt("ID");
                        String gender = r.getString("gender");
                        String name = r.getString("name");
                        String password = r.getString("password");
                        System.out.println(id);
                        System.out.println(name);
                        System.out.println(password);
                        System.out.println(gender);
                    } else PrintMess("User Does Not Exists");
                } catch (SQLException ex) {
                    throw new RuntimeException(ex);
                }
            }
        });

    }

    public void WelcomePage(String name, String gender) {
        GUI Welcome = new GUI("Welcome");
        PrintMess("Your name is " + name + ", your gender is " + gender);
    }
}
````
> Following part is what we want :
````java
ResultSet r = DBConnetion.stmt.executeQuery(Query);
        if(r.next()){
        int id = r.getInt("ID");
        String gender = r.getString("gender");
        String name = r.getString("name");
        String password = r.getString("password");
        System.out.println(id);
        System.out.println(name);
        System.out.println(password);
        System.out.println(gender);
        } else PrintMess("User Does Not Exists");
````
> `executeQuery()`/`next()`/`getInt()`/`getString()` are all methods related to class `ResultSet `

> `getInt()`/`getString()` functions take name of `Column` of database's table as `parameter` or the index of the `Column` (its order, it starts counting from 1)
### 12. Get multiple records (rows) from `MySQL Database`
- We want to view all users , so we will write the following select command :
````java
String Query = "Select * FROM `user`";
````
````java
while(r.next()) {
    int id = r.getInt("ID");
    String gender = r.getString("gender");
    String name = r.getString("name");
    String password = r.getString("password");
    System.out.println(id);
    System.out.println(name);
    System.out.println(password);
    System.out.println(gender);
}
````
> In above code, we have just iterated on each record resulted from `executeQuery()` method 

<hr>

> Good Practicing Tip: try to follow a same convenient while naming your variables

> I'm using `Pascal Case`, Google It :)
