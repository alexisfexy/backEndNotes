# Servlets

Following along: https://happycoding.io/tutorials/java-server/servlets

### Servlet Classes

<u>Defintion</u>: Java class that runs certain functions when a user requests a URL from a server

- contain code that reacts to users actions (such as saving data to a databse, executing logic, returning info to render a page)

```java
import java.io.PrintWriter;
import java.io.IOException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class HelloWorldServlet extends HttpServlet {
  // build a class that extends httpservlet and defines request handler function 
  
  @Override // overrides the doGet() function 
  public void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException {
    PrintWriter out = response.getWriter(); // get a PrintWriter from the response 
    out.println("<h1>Hello World!</w>"); 
    out.println("<p>This is our first service.</p>"); // out put some HTML content to the printwriter 
  }
}
```

^^ This is saved as a .java file.

- you need to compile it into a .class file (like always!)

- Make sure teh servlet API is in our class path: 

  - the Java servlet API is a library comes with any server (like Jetty)

    `javac -cp C:/Users/alexi474/Desktop/jetty/lib/servlet-api-3.1.jar HelloWorldServlet.java`



### The web.xml File

- HelloWorldServlet does not have main() function - so how do we run it? WE DONT. 

- Tell our server to run it when the user requests a certain URL: we do this by saving an XML file: 

- ``` XML
  <web-app>
    <!--defining properties to set up a web app on server -->  
  
  	<servlet>
       <!-- we wrote a servlet class:  --> 
  		<servlet-name>MyHelloWorldServlet</servlet-name>  <!-- name of servlet, we coudl have named it anything since it is only used in the .xml file --> 
  		<servlet-class>HelloWorldServlet</servlet-class>
       <!-- which class it should use (if in package include package name too) --> 
  	</servlet>
  
  	<servlet-mapping>  <!-- tells server which URL should trigger which servlet classes--> 
  		<servlet-name>MyHelloWorldServlet</servlet-name>  <!--refernces name defined in servlet tag --> 
  		<url-pattern>/index.html</url-pattern>  <!--says what URL should trigger the servlet - relative to our web app  --> 
  	</servlet-mapping>
  
  </web-app>
  ```

- Aka: when user requests index.html URL, the HelloWorldServlet class should be triggered. 



### Dynamic Web App Directory Structure

- bundle the xml and servlet.class files into a web app
- directory structure: 
- ![Screen Shot 2020-08-11 at 1.56.17 PM](/Users/alexi474/Library/Application Support/typora-user-images/Screen Shot 2020-08-11 at 1.56.17 PM.png)



you will see a http://localhost:8080/HelloWorldWebApp/index.html,

- localhost:8080 location of Jetty Server
- HelloWorldWebApp/ = name of web app, we can eliminate this if we had named the web app root 
- index.html = name of "file" that we request from our web app 
  - there isnt a index.html file anywhere! our web.xml just knows that a request for this shoudl be send to our HelloWorldServlet class 



The HelloWorldServlet calls the doGet() fucntion because the request is asking us to <u>get</u> the index.hmtl file. 

- the out.println() stamtents in our doGet() function builds HTML that is sent back to the client 
- you can still view the "index.html" but!! it was generated from Java code (so cool!)



### WEB-INF

Everyting in this directory is **invisible** to the user!!! Users can NOT get to it



### Query Parameters

We have been mapping a single URL to a servlet. But we can do more advanced stuff!

 ``` java
import java.io.PrintWriter;
import java.io.IOException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class HelloWorldServlet extends HttpServlet {
  @Override 
  public void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException {
    String name = request.getParameter("name"); // this gets the name parameter from the request! 
    PrintWriter out = response.getWriter();
    out.println("<h1> Hello " + name + "</h1>");
    out.println("<p> Nice to meet you </p>");
  }
}
 ```



### URL Paths

Remember that the `<url-patten>` tag inside `web.xml` file defines a URL to map to a servlet class. 

 You can use * to map to multiple URLs to the same servlet 

- use getRequestURI() and then do a substring of it so you can get the specifics tha you need 

```java
import java.io.PrintWriter;
import java.io.IOException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class HelloWorldServlet extends HttpServlet {

	@Override
	public void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException {

		String requestUrl = request.getRequestURI(); 
    
		String name = requestUrl.substring("/HelloWorldWebApp/hello/".length());

		PrintWriter out = response.getWriter();
		out.println("<h1>Hello " + name + "</h1>");
		out.println("<p>Nice to meet you!</p>");
	}
}
```



### Multiple Servlets

you can have more than one servlet: 

```java
import java.io.PrintWriter;
import java.io.IOException;
import java.util.Date;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class DateServlet extends HttpServlet {

	@Override
	public void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException {

		Date now = new Date();

		PrintWriter out = response.getWriter();
		out.println("<p>Current time: " + now.toString() + "</p>");
	}
}
```

in the web.xml: 

```xml
<web-app>

	<servlet>
		<servlet-name>MyHelloWorldServlet</servlet-name>
		<servlet-class>HelloWorldServlet</servlet-class>
	</servlet>

	<servlet> <!-- NEW -->
		<servlet-name>MyDateServlet</servlet-name>
		<servlet-class>DateServlet</servlet-class>
	</servlet>
	
	<servlet-mapping>
		<servlet-name>MyHelloWorldServlet</servlet-name>
		<url-pattern>/hello/*</url-pattern>
	</servlet-mapping>

	<servlet-mapping> 	<!-- NEW -->
		<servlet-name>MyDateServlet</servlet-name>
		<url-pattern>/date.html</url-pattern>
	</servlet-mapping>

</web-app>
```



### Outputting HTML 

You can output a full HTML page from the a servlet. Just know this is an option! 

