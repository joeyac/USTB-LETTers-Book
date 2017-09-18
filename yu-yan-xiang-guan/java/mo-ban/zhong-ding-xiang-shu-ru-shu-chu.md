# 重定向输入输出

```java
import java.io.FileInputStream;  
import java.io.FileNotFoundException;  
import java.io.FileOutputStream;  
import java.io.PrintStream;  
import java.util.Scanner;  

public class Redirect {  
    Scanner cin = new Scanner(new File("data.in"));
    PrintWriter cout = new PrintWriter(new File("data.out"));
    //注意，以下代码在CF GYM上ＲＥ……以上代码亲测可用

    public static void main(String args[]) throws FileNotFoundException {  

        FileInputStream fis = new FileInputStream("input.txt");  
        System.setIn(fis);  

        PrintStream ps = new PrintStream(new FileOutputStream("output.txt"));  
        System.setOut(ps);  

        Scanner sc = new Scanner(System.in);  
        while (sc.hasNextLine()) {  
            System.out.println(sc.nextLine());  
        }  

    }  
}


```



