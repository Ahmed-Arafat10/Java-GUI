> _**By TA/ Ahmed Arafat :)**_
# GUI Lab #2

### Topics To Be Discussed:

1. `setForeground()` Method
2. `JPasswordField` Class
3. `JRadioButton` class
4. `ButtonGroup` Class  
5. `JCheckBox` Class
6. `JMenuBar`/`JMenu`/`JMenuItem` Classes
7. `Events` in Java GUI & `ActionListener`/`ActionEvent` Classes
    1. Use `inner class` with `Events`
8. Navigate Between `Frames`
9. Connect To `MySQL` Database
10. Show A Message Using `JOptionPane` Class

<hr>
<hr>
<hr>

- Let's continue talking about `JAVA GUI`
- Let's observe the following code
````
class GUI extends JFrame
{
    JButton signup_btn = new JButton("Sign Up");
    JTextField field1 = new JTextField();
    public GUI()
    {
        setTitle("CIC '22");
        setSize(300,300);
        setVisible(true);
        setResizable(false);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocation(200,200);
        setLayout(null);
        signup_btn.setBounds(100,200,100,50);
        signup_btn.setForeground(Color.blue); // new line
        this.add(signup_btn);
    }
}
````
> we created a class that inherits from `JFrame` class
>, then inside the `constructor` we called some methods that are
> discussed in previous lab. the only new method here is; 
````
ButtonObject.signup_btn.setForeground(<Object Of Color Class>);
````
> In above example, `signup_btn.setForeground(Color.blue);` we changed the
> `font color` of button `signup_btn` to `blue`
> <br> again note we have to import `Color` class from `awt` package like this `import java.awt.Color;`


### -`JPasswordField` Class

- to have a text filed that hide the text that is written by the user
, then create an object from class `JPasswordField` like this
````
JPasswordField passwordField1 = new JPasswordField();
````
> we assume that the layout is `absoulte`, all you have to do is to add this object in `Panel p1` object
````
passwordField1.setBounds(100,50,100,25);
p1.add(passwordField1);
````

````
JTextField username_field = new JTextField(10);
````

### - `JRadioButton` class
````
p1.add(male);
p1.add(female);
````

````
    JRadioButton male = new JRadioButton("Male");
    JRadioButton female = new JRadioButton("Female");
````

### - `ButtonGroup` Class

````
ButtonGroup group1 = new ButtonGroup();
````

````
group1.add(male);
group1.add(female);
````

### - `JCheckBox` Class

````
    JCheckBox ch1 = new JCheckBox("married");
    JCheckBox ch2 = new JCheckBox("single");
````
> no need for `buttongroup`

````
        p1.add(ch1);
        p1.add(ch2);
````

### Menu

````

    JMenu file = new JMenu("file");
    JMenu submenu = new JMenu("submenu");
    JMenuItem menubtn1 = new JMenuItem("edit");
    JMenuItem menubtn2 = new JMenuItem("save");
    JMenuItem menubtn3 = new JMenuItem("close");
    JMenuItem menubtn4 = new JMenuItem("inside sub1");
    JMenuItem menubtn5 = new JMenuItem("inside sub2");
    JMenuBar menu = new JMenuBar();

````

````
  public GUI()
    {
        setTitle("CIC '22");
        setVisible(true);
        setResizable(false);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocation(200,200);

        //this.setLayout(new GridLayout(2,1));

        p1.setBackground(Color.RED);
        p1.setLayout(null);
        this.add(p1);
        file.add(menubtn1);
        file.add(menubtn2);
        file.add(menubtn3);

        submenu.add(menubtn4);
        submenu.add(menubtn5);

        file.add(submenu);
        menu.add(file);
        setJMenuBar(menu);
        setSize(500,500);
    }
````
> must add `setSize(500,500);` after `setJMenuBar(menu);` to prevent any error


### 7. `Events` in Java GUI & `ActionListener`/`ActionEvent` Classes


to be able to use `events` in `java GUI` you have to import
````
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
````
> `ActionListener` is an interface

> `ActionEvent` is a class 

````
class GUI extends JFrame implements ActionListener
{
    public GUI()
        {
            setTitle("CIC '22");
            setVisible(true);
            setResizable(false);
            setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            setLocation(200,200);
    
            //this.setLayout(new GridLayout(2,1));
    
            p1.setBackground(Color.RED);
            //p1.setLayout(null);
            this.add(p1);
            p1.add(nameLabel);
            p1.add(username_field);
            p1.add(signinBTN);
            p1.add(password_field);
            setSize(500,500);
            signinBTN.addActionListener(this);
        }
    
        @Override
        public void actionPerformed(ActionEvent e) {
            if(e.getSource() == signinBTN)
            {
                System.out.println(username_field.getText());
                password_field.setText(username_field.getText());
            }
        }
    }
````

### Use `inner class` with `Events`
- this method is used to better organize the code

````
class GUI extends JFrame {
    JButton signinBTN;
    public void home() {

        JPanel p1 = new JPanel();
        JPanel p2 = new JPanel();

        JLabel nameLabel = new JLabel("Username");
        JLabel passwordLabel = new JLabel("Password");

        JTextField username_field = new JTextField(10);
        JPasswordField password_field = new JPasswordField(10);

        signinBTN = new JButton("Sign In");
        JButton signupBTN = new JButton("Sign Up");

        JRadioButton male = new JRadioButton("Male");
        JRadioButton female = new JRadioButton("Female");
        ButtonGroup group1 = new ButtonGroup();

        setTitle("CIC '22");
        setVisible(true);
        setResizable(false);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocation(200, 200);

        //this.setLayout(new GridLayout(2,1));

        p1.setBackground(Color.RED);
        //p1.setLayout(null);
        this.add(p1);
        p1.add(nameLabel);
        p1.add(username_field);
        p1.add(signinBTN);
        p1.add(password_field);
        setSize(500, 500);
        signinBTN.addActionListener(new EVENTS());
    }

    class EVENTS implements ActionListener {
        @Override
        public void actionPerformed(ActionEvent e) {
            if (e.getSource() == signinBTN) {
                // System.out.println("D");
                // this.dispose();
                // this.another();
            }
        }
    }

}
````



## navigate between `Frames`
````
package GUI;

import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent; // class needed
import java.awt.event.ActionListener; // class needed

class GUI extends JFrame implements ActionListener
{
    JButton signinBTN ;
    public void home()
    {
        JPanel p1 = new JPanel();

        JLabel nameLabel = new JLabel("Username");
        JLabel passwordLabel = new JLabel("Password");

        JTextField username_field = new JTextField(10);
        JPasswordField password_field = new JPasswordField(10);

        signinBTN = new JButton("Sign In");
        JButton signupBTN = new JButton("Sign Up");

        JRadioButton male = new JRadioButton("Male");
        JRadioButton female = new JRadioButton("Female");
        ButtonGroup group1 = new ButtonGroup();

        setTitle("CIC '22");
        setVisible(true);
        setResizable(false);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocation(200,200);

        //this.setLayout(new GridLayout(2,1));

        p1.setBackground(Color.RED);
        //p1.setLayout(null);
        this.add(p1);
        p1.add(nameLabel);
        p1.add(username_field);
        p1.add(signinBTN);
        p1.add(password_field);
        setSize(500,500);
        signinBTN.addActionListener(this);
    }

    void another()
    {
        JPanel p1 = new JPanel();
        setTitle("CIC '22");
        setVisible(true);
        setResizable(false);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocation(200,200);

        //this.setLayout(new GridLayout(2,1));

        p1.setBackground(Color.BLUE);
        //p1.setLayout(null);
        this.add(p1);

    }

    @Override
    public void actionPerformed(ActionEvent e) {
        if(e.getSource() == signinBTN)
        {
            System.out.println("D");
            this.dispose();
            this.another();
        }
    }
}

public class Main {
    public static void main(String args[]) {
        GUI G = new GUI();
        G.home();
    }
}
````



### Connect To `MySQL` Database

- first you have to download `MySQL Connector/J` that enables `JAVA` & `IntelliJ` to connect to
`MySQL` database
- Link Of `MySQL Connector/J` : [CLICK ME](https://dev.mysql.com/downloads/connector/j/)
- to connect  to mysql database, first turn  on `Xampp` panel
then go to `file`>`project structure`>`modules`>`dependancies`>press `+` button then choose `JARs` option
finally navigate to following path: `C:\Program Files (x86)\MySQL\Connector J 8.0\mysql-connector-j-8.0.31.jar` 

````
class DB
{
    private static String name = "root";
    private static String password = "";
    private static String add = "jdbc:mysql://localhost:3306/my_cms";
    static String query;
    static Statement stmt;
    public Connection Connect() throws SQLException{
        Connection con = DriverManager.getConnection(add,name,password);
        return con;
    }
}
````


````
   public static void main(String args[]) throws SQLException {

        DBConnetion db = new DBConnetion();
        Connection con = db.Connect();

        String query =  "INSERT INTO photos values (null,'Ahmed Arafat',1,'app',null,null)";
        DBConnetion.stmt = con.prepareStatement(query);
        //DBConnetion.stmt = con.createStatement();
        DBConnetion.stmt.execute(query);
    }
````
> Note: must add `throws SQLException`

- to add variables in query statement
````
 DBConnetion db = new DBConnetion();
        Connection con = db.Connect();
        String name = "Ahmed Arafat";
        String query =  "INSERT INTO photos values (null,'"+name+"',1,'app',null,null)";
        DBConnetion.stmt = con.prepareStatement(query);
        //DBConnetion.stmt = con.createStatement();
        DBConnetion.stmt.execute(query);
````
> Don't forget you must add string values that are going to be inserted/updated in database in single quote `'string'`

- now all you have to do is to get text from ` textfields` object you have created
& then perform `CRUD` operations on `MySQL DataBase` like `Create`/`Read`/`Update`/`Delete`

### 10. Show A Message Using `JOptionPane` Class 
````
JOptionPane.showMessageDialog(null,"Username Is Wrong");
````