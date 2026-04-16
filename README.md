import java.util.*;

public class InfixToPostfix {
    
    // Method to check if character is an operator
    private static boolean isOperator(char ch) {
        return ch == '+' || ch == '-' || ch == '*' || ch == '/' || ch == '^';
    }
    
    // Method to get precedence of operators
    private static int getPrecedence(char ch) {
        switch(ch) {
            case '^': return 3;
            case '*': 
            case '/': return 2;
            case '+': 
            case '-': return 1;
            default: return 0;
        }
    }
    
    // Method to check if character is an operand (letter or digit)
    private static boolean isOperand(char ch) {
        return Character.isLetterOrDigit(ch);
    }
    
    // Main conversion method
    public static String infixToPostfix(String infix) {
        Stack<Character> stack = new Stack<>();
        StringBuilder postfix = new StringBuilder();
        
        for (int i = 0; i < infix.length(); i++) {
            char ch = infix.charAt(i);
            
            // Case 1: If operand, add to output
            if (isOperand(ch)) {
                postfix.append(ch);
            }
            
            // Case 2: If '(', push to stack
            else if (ch == '(') {
                stack.push(ch);
            }
            
            // Case 3: If ')', pop until '('
            else if (ch == ')') {
                while (!stack.isEmpty() && stack.peek() != '(') {
                    postfix.append(stack.pop());
                }
                if (!stack.isEmpty() && stack.peek() == '(') {
                    stack.pop(); // Discard '('
                }
            }
            
            // Case 4: If operator
            else if (isOperator(ch)) {
                // Pop operators with higher or equal precedence
                while (!stack.isEmpty() && stack.peek() != '(' && 
                       getPrecedence(stack.peek()) >= getPrecedence(ch)) {
                    // Special case: '^' is right-associative
                    if (ch == '^' && getPrecedence(stack.peek()) > getPrecedence(ch)) {
                        break;
                    }
                    postfix.append(stack.pop());
                }
                stack.push(ch);
            }
        }
        
        // Pop remaining operators from stack
        while (!stack.isEmpty()) {
            postfix.append(stack.pop());
        }
        
        return postfix.toString();
    }
    
    // Test the implementation
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        System.out.println("=== Infix to Postfix Converter ===");
        System.out.println("Enter infix expression (e.g., A+B*C): ");
        String infix = scanner.nextLine();
        
        // Remove spaces if any
        infix = infix.replaceAll("\\s+", "");
        
        String postfix = infixToPostfix(infix);
        
        System.out.println("\nInfix Expression: " + infix);
        System.out.println("Postfix Expression: " + postfix);
        
        scanner.close();
    }
}
