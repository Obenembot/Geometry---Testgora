# Geometry---Testgora



You are requiered to implement a chatbot that helps childresn with basic geometry problems. You must support the following commands.

1. add <Sharp-type> <id> <param1> <param2> 
adds the sharp type name <id> with the given parameters to the list of sharps .
2. area <id>: returns the area of the sharp named<id> rounded down to the nearest interger, should return "error" if the given <id> doesn't exist.


The sharp-types you must support are

1. rectangle <width> <height>
2triangle <base> <height>

However, you should write the code so that is easily support more types and operations in the future.


The input will be formatted as an array of commands e.g ["add triangle t1 4 5", "area t1", "add rectangle r1 33", area r1"].  The ouput should be comma delimited String of all output. e.g "10,9".

You cab assume that the inout syntax will be a well formatted and that all area, width, height values will be a 32 bit integers.

solution should be in java
import java.util.HashMap;
import java.util.Map;

abstract class Shape {
    abstract int calculateArea();
}

class Rectangle extends Shape {
    private final int width;
    private final int height;

    public Rectangle(int width, int height) {
        this.width = width;
        this.height = height;
    }

    @Override
    public int calculateArea() {
        return width * height;
    }
}

class Triangle extends Shape {
    private final int base;
    private final int height;

    public Triangle(int base, int height) {
        this.base = base;
        this.height = height;
    }

    @Override
    public int calculateArea() {
        return (base * height) / 2;
    }
}

public class GeometryChatbot {
    private final Map<String, Shape> shapes;

    public GeometryChatbot() {
        this.shapes = new HashMap<>();
    }

    public String processCommands(String[] commands) {
        StringBuilder output = new StringBuilder();

        for (String command : commands) {
            String[] parts = command.split(" ");

            switch (parts[0]) {
                case "add":
                    addShape(parts[1], parts[2], Integer.parseInt(parts[3]), Integer.parseInt(parts[4]));
                    break;
                case "area":
                    int area = getArea(parts[1]);
                    output.append(area != -1 ? area : "error").append(",");
                    break;
            }
        }

        // Remove the trailing comma
        return output.length() > 0 ? output.substring(0, output.length() - 1) : "";
    }

    private void addShape(String type, String id, int param1, int param2) {
        Shape shape;
        switch (type) {
            case "rectangle":
                shape = new Rectangle(param1, param2);
                break;
            case "triangle":
                shape = new Triangle(param1, param2);
                break;
            // Add more shape types and their respective classes here
            default:
                return; // Unsupported shape type
        }

        shapes.put(id, shape);
    }

    private int getArea(String id) {
        Shape shape = shapes.get(id);
        return shape != null ? shape.calculateArea() : -1;
    }

    public static void main(String[] args) {
        GeometryChatbot chatbot = new GeometryChatbot();
        String[] commands = {"add triangle t1 4 5", "area t1", "add rectangle r1 3 3", "area r1"};
        String output = chatbot.processCommands(commands);
        System.out.println(output); // Output: "10,9"
    }
}






