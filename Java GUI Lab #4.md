

- Let's see how to update a record (row) that already exists
in table `user` in `PhpMyAdmin`

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
    public static Statement stmt;//statement

    // return type
    public Connection Connect() throws SQLException {
        Connection con = DriverManager.getConnection(add, name, password);
        return con;
    }
}

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

    Vector<String> GetIDFromDB() {
        Vector<String> myarr = new Vector<String>();
        String Query = "Select ID FROM `user`";
        try {
            DBConnetion.stmt = Con.prepareStatement(Query);
        } catch (SQLException ex) {
            throw new RuntimeException(ex);
        }
        try {
            ResultSet r = DBConnetion.stmt.executeQuery(Query);
            while (r.next()) {
                int id = r.getInt("ID");
                myarr.add(String.valueOf(id));
            }
        } catch (SQLException ex) {
            throw new RuntimeException(ex);
        }
        return myarr;
    }

    public void HomePage() {
        JPanel p1 = new JPanel();
        Vector<String> myarr = GetIDFromDB();
        JComboBox mylist = new JComboBox(myarr);
        mylist.setSelectedItem(null);
        JTextField NameField = new JTextField();
        JPasswordField PasswordField = new JPasswordField();
        JButton SignInButton = new JButton("Click Me");
        SignInButton.setBounds(150, 400, 100, 30);
        NameField.setBounds(150, 50, 150, 30);
        PasswordField.setBounds(150, 120, 150, 30);
        mylist.setBounds(300, 400, 100, 25);
        GUI home = new GUI("Home");
        p1.setBackground(Color.RED);
        p1.setLayout(null);
        home.add(p1);
        p1.add(mylist);
        p1.add(NameField);
        p1.add(SignInButton);
        p1.add(PasswordField);

        SignInButton.addActionListener((ActionEvent e) -> {

            if (NameField.getText().isEmpty()) PrintMess("Text Field Cannot Be Empty");
            else if (PasswordField.getPassword().length == 0) PrintMess("Password Field Cannot Be Empty");
            else if (mylist.getSelectedItem() == null) PrintMess("Empty");
            else {
                int ChoosenID = Integer.valueOf(mylist.getSelectedItem().toString());
                String username = NameField.getText() , password = String.valueOf(PasswordField.getPassword());
                String Query = "UPDATE `user` set name = '" + username+ "', password = '" + password + "' WHERE ID = '" + ChoosenID + "' ";
                System.out.println(Query);
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
                PrintMess("Your Data Is Updated Successfully");
            }
        });

    }
}

public class Main {
    
    public static void main(String args[]) throws SQLException {
        GUI obj1 = new GUI();
        obj1.HomePage();
    }
}
````

- The most important part that we want to get `IDs` of all records in the database
and show them in our `JComboBox` object, so that the user can select `ID` from a list not entering an `ID` that may or may not exist in the `Database`
- The following function is responsible for that task :
````java
Vector<String> GetIDFromDB() {
        Vector<String> myarr = new Vector<String>();
        String Query = "Select ID FROM `user`";
        try {
            DBConnetion.stmt = Con.prepareStatement(Query);
        } catch (SQLException ex) {
            throw new RuntimeException(ex);
        }
        try {
            ResultSet r = DBConnetion.stmt.executeQuery(Query);
            while (r.next()) {
                int id = r.getInt("ID");
                myarr.add(String.valueOf(id));
            }
        } catch (SQLException ex) {
            throw new RuntimeException(ex);
        }
        return myarr;
    }
````
> The return type of `GetIDFromDB()` is `Vector<String>`
, you can see we didn't return an array as we don't know how many `IDs`
exist in the `Database`, also we didn't use `ArrayList` as the constructor of `JComboBox` class
takes `Vector` not `ArrayList`, `Vector` & `ArrayList` are almost same but there are some differences (advanced to till you), What I wan from you is to know we use both when we want to create a `Dynamic Array` & it means when we create it we don't specify a size for that it

> Also I have done the following code in a function so that I can call it in any page I want (Global to all pages not for a specific page) 

> in this line : `myarr.add(String.valueOf(id));` , as the `IDs` that we got from `Database` are `int`, and our `Vector` stores `String` so we have to
change it from `int` to `String` using `String.valueOf()` function


### Delete from the `Database`

- The same concept as above code, but different parts is that I want just a `JButton` & a `JComboBox` in the page
& the other thing is that I will execute the `Delete Statement`
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
    public static Statement stmt;//statement

    // return type
    public Connection Connect() throws SQLException {
        Connection con = DriverManager.getConnection(add, name, password);
        return con;
    }
}

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

    Vector<String> GetIDFromDB() {
        Vector<String> myarr = new Vector<String>();
        String Query = "Select ID FROM `user`";
        try {
            DBConnetion.stmt = Con.prepareStatement(Query);
        } catch (SQLException ex) {
            throw new RuntimeException(ex);
        }
        try {

            ResultSet r = DBConnetion.stmt.executeQuery(Query);
            while (r.next()) {
                int id = r.getInt("ID");
                myarr.add(String.valueOf(id));
            }
        } catch (SQLException ex) {
            throw new RuntimeException(ex);
        }
        return myarr;
    }
    public void HomePage() {
        JPanel p1 = new JPanel();
        Vector<String> myarr = GetIDFromDB();
        JComboBox mylist = new JComboBox(myarr);
        mylist.setSelectedItem(null);
        JButton SignInButton = new JButton("Click Me");
        SignInButton.setBounds(150, 300, 100, 30);
        mylist.setBounds(150, 150, 100, 25);
        GUI home = new GUI("Home");
        p1.setBackground(Color.RED);
        p1.setLayout(null);
        home.add(p1);
        p1.add(mylist);
        p1.add(SignInButton);
        SignInButton.addActionListener((ActionEvent e) -> {
            if (mylist.getSelectedItem() == null) PrintMess("Empty");
            else {
                int ChoosenID = Integer.valueOf(mylist.getSelectedItem().toString());
                String Query = "DELETE FROM `user` WHERE ID = '" + ChoosenID + "' ";
                System.out.println(Query);
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
                PrintMess("Your Data Is Deleted Successfully");
            }
        });

    }
}

public class Main {

    public static void main(String args[]) throws SQLException {
        GUI obj1 = new GUI();
        obj1.HomePage();
    }
}
````
> `mylist.setSelectedItem(null);` to make nothing selected by default in the List

> `String Query = "DELETE FROM user WHERE ID = '" + ChoosenID + "' ";`
               
> Tip: It is a good practicing to name `SQL Keywords` in Uppercase & put table name inside ``

# `Jtable` Class
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
    public static Statement stmt;//statement

    // return type
    public Connection Connect() throws SQLException {
        Connection con = DriverManager.getConnection(add, name, password);
        return con;
    }
}

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
        String[][] data = {
                { "1","Ahmed", "123" },{"2","Arafat","999"}
        };
        String[] ColName = {"ID","Name","Password"};
        JTable mytable = new JTable(data,ColName);
        JScrollPane myScroll = new JScrollPane(mytable);
        myScroll.setBounds(30, 40, 200, 300);
        GUI home = new GUI("Home");
        p1.setBackground(Color.RED);
        p1.setLayout(null);
        home.add(p1);
        p1.add(myScroll);
    }
}

public class Main {

    public static void main(String args[]) throws SQLException {
        GUI obj1 = new GUI();
        obj1.HomePage();
    }
}
````
> The problemm here is that we have to get rows Dynamically from `Database`

- To do so we will add `GetTableDataFromDB()` method and change some lines in `HomePage()` method 
````java
DefaultTableModel GetTableDataFromDB() {
        String[] ColName = {"ID","Name","Password"};
        DefaultTableModel tableModel = new DefaultTableModel(ColName, 0);
        String Query = "Select * FROM `user`";
        try {
            DBConnetion.stmt = Con.prepareStatement(Query);
        } catch (SQLException ex) {
            throw new RuntimeException(ex);
        }
        try {
            ResultSet r = DBConnetion.stmt.executeQuery(Query);
            while (r.next()) {
                Vector<String> Inner = new Vector<String>();
                Inner.add(String.valueOf(r.getInt("ID")));
                Inner.add(r.getString("name"));
                Inner.add(r.getString("password"));
                tableModel.addRow(Inner);
            }
        } catch (SQLException ex) {
            throw new RuntimeException(ex);
        }
        return tableModel;
    }
    public void HomePage() {
        JPanel p1 = new JPanel();
        DefaultTableModel mytablemodel = GetTableDataFromDB();
        JTable mytable = new JTable(mytablemodel);
        JScrollPane myScroll = new JScrollPane(mytable);
        mytable.getColumnModel().getColumn(0).setPreferredWidth(100);
        mytable.getColumnModel().getColumn(1).setPreferredWidth(100);
        mytable.getColumnModel().getColumn(2).setPreferredWidth(100);
        myScroll.setBounds(30, 40, 300, 300);
        GUI home = new GUI("Home");
        p1.setBackground(Color.RED);
        p1.setLayout(null);
        home.add(p1);
        p1.add(myScroll);
    }
````
> `DefaultTableModel mytablemodel = GetTableDataFromDB();`

> `mytable.getColumnModel().getColumn(0).setPreferredWidth(100);`
> 
