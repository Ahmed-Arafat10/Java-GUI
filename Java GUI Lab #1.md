> _**By TA/ Ahmed Arafat :)**_
# GUI Part #1
### GUI Elements
 - **Components**
   - buttons
   - menu
   - text field
   - radio
   - checkbox

 - **Container**
   - Frame
   - Panel

 - **Helper**
   - events
   - color
   - font
   - layout


- **Swing Library**
  - JFrame
  - JButton
  - JLabel

- **Awt Library**
  - layout
  - color
  - font
  - button - label - frame (Old style)


### 1- Drag & Drop (Not recommended AT ALL)

### 2- Let's write the code with ourselves 

2.1 - Create a class that contains an object from JFrame class

- JFrame Class Methods :
````java
class GUI
{
    JFrame frame1 = new JFrame();
    public GUI()
    {
        frame1.setTitle("cic");
        frame1.setResizable(false);
        frame1.setSize(500,500);
        // without it, JFrame WILL NOT BE SHOWN
        frame1.setVisible(true);
        frame1.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        // must import `awt` class first
        // wont work (must add panel)
        frame1.setBackground(Color.RED);
        frame1.setLocation(300,100);
    }
}
public class Main {
    public static void main(String args[]) {
        GUI test  = new GUI();
    }
````

2.2 - Create a class that INHERITS from JFrame class
then write your methods in the constructor
``````java
class GUI extends JFrame
{
    public GUI()
    {
        setTitle("cic");
        setResizable(false);
        setSize(500,500);
        setVisible(true);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setBackground(Color.RED);
        setLocation(300,100);
    }
}
public class Main {
    public static void main(String args[]) {
        System.out.println("hello world");
        GUI test  = new GUI();
    }
}
``````

- You must add a Panel inside a Frame to be able to 
  - Add background
  - Add image

> VIP NOTE : to prevent any error try to add new objects from classes like Panel/JButons/etc 
  inside the class itself not inside a specific function

- To add a new panel to an existing JFrame, create an object
from ``JPanel`` Class inside your ``GUI`` class
````java
JPanel panel1 = new JPanel();
````
- Then in the constructor add
````java
panel1.setBackground(Color.red); // you gave the color to panel not to JFrame
add(panel1);
````
- Then create two buttons & one textfield inside Our Class
````java
JButton but1 = new JButton("Button1");
JButton but2 = new JButton("Button2");
JTextField field1 = new JTextField();
````
- Then to add the previous components
````java
add(but1);
add(but2);
add(field1);
````

> the problem here is that first we didn't set the layout of FRAME yet so, the LAST added element
  will occupy the WHOLE FRAME

- To prevent this add the following line:
````java
// Anonymous Object 
setLayout(new FlowLayout());
````

- but the problem here is that all components become in the same
line (even the panel itself)

- to solve this I want to add the components to the Panel itself
not to the frame
````java
add(panel1);
panel1.add(but1);
panel1.add(but2);
panel1.add(field1);
````
> by default the layout of a `panel` is `Flowlayout`

- To change Layout of a panel from default to `Absolute Layout`
````java
panel1.setLayout(null);
````

> in this case all of your components will not be shown, WHY??

> Because you must specify both `setLocation()` & `setSize()` FOR EACH COMPONENT

- So, if I'm going to add a `JButton`
````java
but1.setLocation(100,100); // set the ABSOLUTE Location
but1.setSize(100,100);
add(but1);
````


- This will introduce the layout concept in `Java GUI`
    - FlowLayout
    - GridLayout
    - BorderLayout
    - AbsoluteLayout


### FlowLayout

- first you have to import
````java
import java.awt.FlowLayout;
````
- In the `Frame` itself, we will specify its layout then add new components
````java
this.setLayout(new FlowLayout());
add(but1);
add(but2);
````

> buttons will be aligned in the same line horizontally

> also Note the arrangement of adding each element differs in their order

- You can pass a parameter to the constructor to control its location
````java
this.setLayout(new FlowLayout(FlowLayout.LEFT))
````
> Can Pass -> `LEFT`/`CENTER`/`RIGHT`

>Note: Default constructor is `CENTER`

- Also You can add two parameters to the constructor to control its `x` & `y` distance
````java
this.setLayout(new FlowLayout(FlowLayout.CENTER,10,30));
````


### GridLayout
- It Separates the `frame`/`panel` into `Raws` & `Columns`
````java
this.setLayout(new GridLayout());// will automatically create ONE ROW and as columns as needed
add(but1);
add(but2);
````

- To specify the `Raws` & `Columns`, pass two parameters to its constructor
````java
this.setLayout(new GridLayout(2,3));// will automatically create ONE ROW and as columns as needed
add(but1);
add(but2);
add(but3);
````

- Also You can add two parameters to the constructor to control its `hgap` & `vgap`

````java
this.setLayout(new GridLayout(2,3,10,30));// will automatically create ONE ROW and as columns as needed
add(but1);
add(but2);
add(but3);
````
- Why `GridLayout` is used?
- Ans: `GridLayout` is used when you want to set the layout of WHOLE frame, as you may need to separate the frame into multiple panels
- In the following example we will create a
````java
panel1.setBackground(Color.RED);
panel2.setBackground(Color.BLUE);
this.setLayout(new GridLayout(2,1));
add(panel1);
add(panel2);
panel1.add(but1);
panel1.add(but2)
````
> Note: we set the Layout of the `Frame` itself
> So, we are separating the `Frame` horizontally into to `panels`
> then adding two `buttons` in `panel1` (layout of `panel1` is FlowLayout by `default`)

### BorderLayout
-  `BorderLayout` used to set `Frame`/`Panel` into `EAST`/`WEST`/`NORTH`/`SOUTH`/`CENTER` Directions
````java
setLayout(new BorderLayout());
add(but1,BorderLayout.EAST);
add(but2,BorderLayout.WEST);
````
> can pass add(but1,BorderLayout.`EAST`/`WEST`/`NORTH`/`SOUTH`/`CENTER`)

- Also You can add two parameters to the constructor to control its `hgap` & `vgap`
````java
setLayout(new BorderLayout(10,30));
````

### AbsoluteLayout
- This will add the components in an `Absolute` position inside the `Frame`/`Panel`
````java
setLayout(null);
but1.setBounds(300,100,100,50);
add(but1);
````
