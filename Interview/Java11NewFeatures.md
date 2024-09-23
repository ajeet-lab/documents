Java 11, released in September 2018, introduced several new features and enhancements aimed at improving performance, code readability, and developer productivity. Below are some of the key features along with examples:

### 1. **Local-Variable Syntax for Lambda Parameters**
Java 11 allows the use of `var` for the parameters of implicitly typed lambda expressions.

#### Example:
```
import java.util.List;

public class LambdaVarExample {
    public static void main(String[] args) {
        var list = List.of("Java", "Python", "JavaScript");

        list.forEach((var item) -> System.out.println(item));
    }
}
```

This allows more consistency in the way `var` is used in Java.

### 2. **String Methods Enhancements**

Java 11 added new methods to the `String` class, including `isBlank()`, `lines()`, `repeat()`, `strip()`, etc.

#### Examples:

- **`isBlank()`**: Checks if the string is empty or contains only whitespace.
  
  ```
  public class StringBlankExample {
      public static void main(String[] args) {
          String str = "   ";
          System.out.println(str.isBlank());  // true
      }
  }
  ```

- **`lines()`**: Converts a multi-line string into a stream of lines.

  ```
  public class StringLinesExample {
      public static void main(String[] args) {
          String str = "Line1\nLine2\nLine3";
          str.lines().forEach(System.out::println);
      }
  }
  ```

- **`repeat()`**: Repeats the string multiple times.

  ```
  public class StringRepeatExample {
      public static void main(String[] args) {
          String str = "Java";
          System.out.println(str.repeat(3));  // JavaJavaJava
      }
  }
  ```

- **`strip()`**: Removes leading and trailing whitespace.

  ```
  public class StringStripExample {
      public static void main(String[] args) {
          String str = "  Hello Java!  ";
          System.out.println(str.strip());    // "Hello Java!"
      }
  }
  ```

### 3. **Files Methods Enhancements**

Java 11 introduced new methods for working with files in the `java.nio.file.Files` class.

#### Examples:

- **`writeString()`**: Writes a `String` to a file.

  ```
  import java.nio.file.Files;
  import java.nio.file.Path;
  import java.nio.file.Paths;

  public class WriteStringExample {
      public static void main(String[] args) throws Exception {
          Path path = Paths.get("example.txt");
          Files.writeString(path, "Hello, Java 11!");
      }
  }
  ```

- **`readString()`**: Reads a file’s content as a `String`.

  ```
  import java.nio.file.Files;
  import java.nio.file.Path;
  import java.nio.file.Paths;

  public class ReadStringExample {
      public static void main(String[] args) throws Exception {
          Path path = Paths.get("example.txt");
          String content = Files.readString(path);
          System.out.println(content);
      }
  }
  ```

### 4. **Optional Enhancements**

Java 11 added new methods like `isEmpty()` to the `Optional` class.

#### Example:
```
import java.util.Optional;

public class OptionalExample {
    public static void main(String[] args) {
        Optional<String> optional = Optional.of("Java 11");
        
        System.out.println(optional.isEmpty());   // false
        System.out.println(optional.isPresent()); // true
    }
}
```

### 5. **HTTP Client**

Java 11 introduced a new HTTP client in the `java.net.http` package. This replaces the legacy `HttpURLConnection`.

#### Example:
```
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

public class HttpClientExample {
    public static void main(String[] args) throws Exception {
        HttpClient client = HttpClient.newHttpClient();
        HttpRequest request = HttpRequest.newBuilder()
                                         .uri(URI.create("https://api.github.com/"))
                                         .build();

        HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());
        System.out.println(response.body());
    }
}
```

### 6. **Nest-Based Access Control**

Java 11 allows inner classes (nests) to access private members of outer classes and vice versa without the need for synthetic bridge methods. This improves JVM efficiency.

#### Example:
```
public class OuterClass {
    private String message = "Hello from OuterClass";

    class InnerClass {
        void displayMessage() {
            System.out.println(message);
        }
    }

    public static void main(String[] args) {
        OuterClass outer = new OuterClass();
        OuterClass.InnerClass inner = outer.new InnerClass();
        inner.displayMessage();  // Works without synthetic methods
    }
}
```

### 7. **Pattern Matching in `instanceof` (Preview Feature)**

Java 11 didn't introduce this feature, but it started preparing the groundwork for future versions to improve `instanceof` with pattern matching, which was officially introduced in Java 14 as a preview.

### 8. **Running a Single-File Source Code Without Compilation**

Java 11 allows you to run a single `.java` file without the need for explicitly compiling it with `javac`.

#### Example:

If you have a file called `HelloWorld.java`:

```
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, Java 11!");
    }
}
```

You can run this directly in the command line:

```bash
java HelloWorld.java
```

### 9. **Deprecation of Nashorn JavaScript Engine**

The Nashorn JavaScript Engine has been deprecated in Java 11 and will be removed in future versions.

### 10. **Z Garbage Collector (ZGC)**

Java 11 introduced a new experimental garbage collector called **ZGC**. It’s a scalable low-latency garbage collector designed for applications that require large heaps.

To enable ZGC:

```bash
java -XX:+UseZGC -jar your-application.jar
```

### 11. **Flight Recorder**

Java Flight Recorder (JFR) was made available in open source in Java 11. JFR is a profiling tool used for diagnosing and monitoring the performance of Java applications.

### Conclusion

Java 11 provides numerous features and improvements that enhance developer productivity, reduce boilerplate, and offer more modern capabilities such as the new HTTP client, improved string handling, and enhancements to `Optional`. It also introduces new language features, such as `var` in lambdas, and improves JVM performance with features like ZGC and Flight Recorder.