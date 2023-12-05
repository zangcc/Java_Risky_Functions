# Java-危险函数总结
Java危险函数汇总 / Summary of risky functions in Java


以下是一些可能存在安全风险的Java函数的使用示例：

1. **Runtime.exec()**：执行系统命令。这个函数用于执行系统命令。如果未过滤的输入传递给这个API，可能会导致任意命令执行
```java
Runtime r = Runtime.getRuntime();
r.exec("/bin/sh -c some_tool" + input);  // input should be properly sanitized
```

2. **ScriptEngine.eval()**：评估动态代码。这个函数用于评估动态代码。当动态代码被评估时，攻击者有机会影响代码，可能导致恶意代码执行，从而导致数据泄露或操作系统妥协。
```java
ScriptEngineManager factory = new ScriptEngineManager();
ScriptEngine engine = factory.getEngineByName("JavaScript");
engine.eval(script);  // script should be trusted or properly sanitized
```

3. **SpelExpressionParser()**：解析Spring表达式。这个函数用于解析Spring表达式。如果用户控制的输入被解析，可能会导致表达式语言注入。
```java
ExpressionParser parser = new SpelExpressionParser();
Expression exp = parser.parseExpression(expression);  // expression should be trusted or properly sanitized
```

4. **ClassLoader.defineClass()**：定义类。
```java
byte[] b = ...  // the byte array should be trusted
Class<?> c = myClassLoader.defineClass(className, b, 0, b.length);
```

5. **URLClassLoader**：加载代码。这个类用于加载代码，可能会导致代码执行。
```java
URL[] urls = ...  // the URLs should be trusted
URLClassLoader classLoader = new URLClassLoader(urls);
```

6. **java.beans.Introspector.getBeanInfo()**：可能会导致类加载器从不受信任的源加载类。
```java
BeanInfo info = Introspector.getBeanInfo(bean.getClass());
```

7. **java.io.File**：文件访问。这个类的构造函数以及delete、renameTo、listFiles和list方法可能会导致文件访问，包括删除/重命名文件和目录列表。
```java
File file = new File(filename);  // filename should be trusted or properly sanitized
```

8. **java.io.FileInputStream、java.io.FileOutputStream、java.io.FileReader、java.io.FileWriter、java.io.RandomAccessFile**：文件读/写访问。
```java
FileInputStream fis = new FileInputStream(filename);  // filename should be trusted or properly sanitized
```

9. **System.setProperty、System.getProperties、System.getProperty**：修改系统属性。这些函数可能会导致系统属性被修改，某些系统属性可能包含敏感信息，或者可能影响关键操作的执行。
```java
System.setProperty(key, value);  // key and value should be trusted or properly sanitized
```

10. **System.load、System.loadLibrary**：加载本地库。这些函数用于加载本地库，可能会导致任意代码执行。
```java
System.load(filename);  // filename should be trusted
```

11. **Runtime.exec、ProcessBuilder**：执行操作系统命令。
```java
ProcessBuilder pb = new ProcessBuilder("myCommand", "myArg1", "myArg2");  // the command and arguments should be trusted or properly sanitized
```

以上只是一些例子，实际上还有很多其他的函数也可能带来安全风险。在使用这些函数时，应确保对输入进行适当的过滤和检查，以防止潜在的安全问题。
