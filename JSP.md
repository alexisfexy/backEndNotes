# JSP

Following along: https://happycoding.io/tutorials/java-server/jsp



In the past we have had HTML outputted one line at a time using `out.println()`

Here are the downfalls of HTML inside Java:

- hard to edit 
- hard to format with proper indentation
- hard to debug

The better way to do it is JSP: JavaServer Pages aka Java inside HTML 

### JSP Files 

Instead of HTML in Java you are having Java inside HTML 

- you have to upload .jsp files to a Java Servet (not jsut any file host)
  - they are compiled into servlets automatically!! 



<u>Ways to include Java code in HTML :</u> 

**Scriplets** <% %>

A scriptlet is Java code that **does something** and is placed between opening `<%` and closing `%>`

- You can split the Java code into multiple scriptlets. For example one scriptlet can contain the opening of the `if` statement, and another scriptlet contains the closing bracket.



**Expressions** <%= %> 

An expression is Java code that **evaluates to a value** and is placed between opening `<%=` and closing `%>` expression tags. 

For example: 

```html
<p>The current Unix time is: <%= System.currentTimeMillis() %></p>
```



**Directives** <%@ %>

Directives are placed between opening `<%@` and closing `%>` tags, and they contain code that **tells the page itself what to do.**

- things like import statements! 

- ```html
  <%@ page import="java.util.Date" %>
  ```

- also can be used to include content frm other files:

  ```html
  <!DOCTYPE html>
  <html>
  	<head>
  		<title>Header Example</title>
  	</head>
  	<body>
  		<%@ include file = "header.html" %> <!-- this copies the content from the html file -->
  		<p>Content goes here.</p>
  	</body>
  </html>
  ```

  

### JSP & Servlets Together 

```java
import java.io.PrintWriter;
import java.io.IOException;
import java.text.SimpleDateFormat;
import java.util.Date;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class DateServlet extends HttpServlet {

	@Override
	public void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException, ServletException {

		// 5:12 PM on Saturday, June 3, 2017
		SimpleDateFormat dateFormat = new SimpleDateFormat("hh:mm aa 'on' EEEE, MMMM dd, yyyy");
		Date now = new Date();
		String formattedDate = dateFormat.format(now);

		request.setAttribute("date", formattedDate);
		request.getRequestDispatcher("dateView.jsp").forward(request,response);
	}
}
```

This code uses the `SimpleDateFormat` class to format the current time. It then uses the `request.setAttribute()` function to include the formatted date in the request, and then it uses the `request.getRequestDispatcher("path/to/view.jsp").forward(request, response)` function to send the request to a JSP file.

- the java then INCLUDES The date in the new request to the JSP file : 

  ```html
  <!DOCTYPE html>
  <html>
  	<head>
  		<title>Current Time</title>
  	</head>
  	<body>
  		<h1>Current Time</h1>
  		<p>The current time is <%= request.getAttribute("date") %></p> 
  	</body>
  </html>
  ```

- `<%= request.getAttribute("date") %>` gets the `date` attribute that the servlet added.



You need an XML file to map the request to the servlet: 

```xml
<web-app>

	<servlet>
		<servlet-name>CurrentTimeServlet</servlet-name>
		<servlet-class>DateServlet</servlet-class>
	</servlet>

	<servlet-mapping>
		<servlet-name>CurrentTimeServlet</servlet-name>
		<url-pattern>/current-time</url-pattern>
	</servlet-mapping>

</web-app>
```



### Expression Language

For more advanced features of xml you have to use a **deployment descriptor**

```xml
<web-app
	xmlns="http://xmlns.jcp.org/xml/ns/javaee"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
		http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
	version="3.1">
  
</web-app>
```

These tell the server that we want to use the latest features, including **expression language** in our JSP files.

Expression language (or EL) gives us a simplified syntax for doing things like accessing attributes. So now we can use `${date}` instead of `<%= request.getAttribute("date") %>`



### MVC 

MVC = Model View Controller: This is the idea of separating your data from your logic from your rendering

- Your model is the data structures that hold your site’s data. This can be things like a database or `ArrayList` objects that hold data.
- Your controller is your servlet classes. These read from your model and execute logic.
- Your view is your JSP files. These take parameters from your controller to render a page.

