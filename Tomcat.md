# TOMCAT

#### Directories and Files
These are some of the key tomcat directories:

- /bin - Startup, shutdown, and other scripts. The *.sh files (for Unix systems) are functional duplicates of the *.bat files (for Windows systems). Since the Win32 command-line lacks certain functionality, there are some additional files in here.
- /conf - Configuration files and related DTDs (document type definition). The most important file in here is server.xml. It is the main configuration file for the container.
- /logs - Log files are here by default.
- /webapps - This is where your webapps go.

---

Throughout the documentation, there are references to the two following properties:

- CATALINA_HOME: Represents the root of your Tomcat installation, for example /home/tomcat/apache-tomcat-9.0.10 or C:\Program Files\apache-tomcat-9.0.10.
- CATALINA_BASE: Represents the root of a runtime configuration of a specific Tomcat instance. If you want to have multiple Tomcat instances on one machine, use the CATALINA_BASE property.
If you set the properties to different locations, the CATALINA_HOME location contains static sources, such as .jar files, or binary files. The CATALINA_BASE location contains configuration files, log files, deployed applications, and other runtime requirements.
---

### Standard Directory Layout
To facilitate creation of a Web Application Archive file in the required format, it is convenient to arrange the "executable" files of your web application (that is, the files that Tomcat actually uses when executing your app) in the same organization as required by the WAR format itself. To do this, you will end up with the following contents in your application's "document root" directory:

- *.html, *.jsp, etc. - The HTML and JSP pages, along with other files that must be visible to the client browser (such as JavaScript, stylesheet files, and images) for your application. In larger applications you may choose to divide these files into a subdirectory hierarchy, but for smaller apps, it is generally much simpler to maintain only a single directory for these files.

- /WEB-INF/web.xml - The Web Application Deployment Descriptor for your application. This is an XML file describing the servlets and other components that make up your application, along with any initialization parameters and container-managed security constraints that you want the server to enforce for you. This file is discussed in more detail in the following subsection.

- /WEB-INF/classes/ - This directory contains any Java class files (and associated resources) required for your application, including both servlet and non-servlet classes, that are not combined into JAR files. If your classes are organized into Java packages, you must reflect this in the directory hierarchy under /WEB-INF/classes/. For example, a Java class named com.mycompany.mypackage.MyServlet would need to be stored in a file named /WEB-INF/classes/com/mycompany/mypackage/MyServlet.class.

- /WEB-INF/lib/ - This directory contains JAR files that contain Java class files (and associated resources) required for your application, such as third party class libraries or JDBC drivers.


---

### Web Application Deployment Description (/WEB-INF/web.xml)
- Contains the web application deployment descriptor for the application. 
- It is an XML document.
- defines everything about the application that a server needs to know except the context path.


---

### Filters
- A filter is an object that is invoked at the preprocessing and postprocessing of a request. It is mainly used to perform filtering tasks such as conversion, logging, compression, encryption and decryption, input validation etc.

-  Typically, filters do not generate content themselves, although, the filter will generate some minimal content. 

- A filter implements the javax.servlet.Filter interface. The primary filter functionality is implemented by the doFilter() method of the filter.

- They intercepts the requests from client before they try to access the resource or the servlet.

- The servlet filter is pluggable, i.e. its entry is defined in the web.xml file, if we remove the entry of filter from the web.xml file, filter will be removed automatically and we don't need to change the servlet.

#### Following are the filter methods:

> Public void doFilter(ServletRequest,ServletResponse, FilterChain)

This is called everytime when a request/response is passed from every client when it is requested from a resource.

> Public void init(FilterConfig)
This is to indicate that filter is placed into service

> Public void destroy()
> This to indicate the filter has been taken out from service.



![Diagram of filter-to-servlet mapping with filters F1-F3 and servlets S1-S3. F1 filters S1-S3, then F2 filters S2, then F3 filters S1 and S2.](https://docs.oracle.com/cd/E19879-01/819-3669/images/web-filterMapping.gif)



Recall that a filter chain is one of the objects passed to the `doFilter` method of a filter. This chain is formed indirectly by means of filter mappings. The order of the filters in the chain is the same as the order in which filter mappings appear in the web application deployment descriptor.

When a filter is mapped to servlet S1, the web container invokes the `doFilter` method of F1. The `doFilter` method of each filter in S1’s filter chain is invoked by the preceding filter in the chain by means of the `chain.doFilter` method. Because S1’s filter chain contains filters F1 and F3, F1’s call to `chain.doFilter` invokes the `doFilter` method of filter F3. When F3’s `doFilter` method completes, control returns to F1’s `doFilter` method.



- filter

  ```java
  import java.io.IOException;  
  import java.io.PrintWriter;  
    
  import javax.servlet.*;  
    
  public class MyFilter implements Filter{  
    
  public void init(FilterConfig arg0) throws ServletException {}  
        
  public void doFilter(ServletRequest req, ServletResponse resp,  
      FilterChain chain) throws IOException, ServletException {  
            
      PrintWriter out=resp.getWriter();  
      out.print("filter is invoked before");  
            
      chain.doFilter(req, resp);//sends request to next resource  
            
      out.print("filter is invoked after");  
      }  
      public void destroy() {}  
  }  
  ```

  



- Filter Mapping

```java
<web-app>  
  
<filter>  
<filter-name>...</filter-name>  
<filter-class>...</filter-class>  
</filter>  
   
<filter-mapping>  
<filter-name>...</filter-name>  
<url-pattern>...</url-pattern>  
</filter-mapping>  
  
</web-app>  
```

---

### StackOverflowError in Java
A JVM stack is created for each JVM thread. Whenever a method is invoked a new frame is created and pushed into the JVM stack for the thread. Each frame stores data related to the method like local variables, operand stack and a reference to the run-time constant pool of the class of the current method. Once the method execution is completed stack frame (for that method) is popped out of the stack.

If execution of any method requires a larger stack than is permitted, the Java Virtual Machine throws a StackOverflowError. You may see StackOverflowError when you have a recursive method with no terminating condition.

```java
public void  printHello(){
	// No terminating condition
	System.out.println("hello world");
	printHello();
}
```

> ```
> Exception in thread "main" java.lang.StackOverflowError
> ```



---



### OutOfMemoryError in Java

- Whenever you create a new object memory for that object is allocated on the heap. Instance variables and arrays are also stored on the heap.

- Once the object stored on the heap is not having any reference, memory for that object is reclaimed by garbage collector. If there are references to the object then GC can’t remove those objects, if you have large number of such referenced objects and JVM tries to allocate heap memory for new object, JVM throws java.lang.OutOfMemoryError because there is no sufficient heap memory left.

- Trying to allocate an array that is larger than the heap size also results in OutOfMemoryError.

```java
public static void main(String[] args) {
	Integer[] array = new Integer[1000*1000*1000];
}
```

> ```
> Exception in thread "main" java.lang.OutOfMemoryError: Java heap space
> ```



---

#### StackOverflowError Vs OutOfMemoryError in Java

|                    **StackOverflowError**                    |                     **OutOfMemoryError**                     |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
|    StackOverflowError is thrown when the *stack is full*.    |  OutOfMemoryError is thrown when the *heap space is full*.   |
| Stack is used to store method related data when any method is invoked. So, StackOverflowError is thrown when there is no space left for storing method data. | Heap memory is used to store objects, instance variables and arrays. So, OutOfMemoryError is thrown when no space left for creating new objects, arrays. |
| Recursive methods with out terminating condition causes StackOverflowError. | Lots of objects with live references so GC can’t deallocate those objects results in OutOfMemoryError. |
| To avoid StackOverflowError ensure that methods are executing as per logic and terminating so that the stack frames for the executed methods can be taken out of the stack. | To avoid OutOfMemoryError ensure objects which are no longer required are not reference from any where and can be garbage collected. Also ensure not to create very large objects or arrays. |

