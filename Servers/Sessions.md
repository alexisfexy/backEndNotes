# Sessions

Following Along: https://happycoding.io/tutorials/java-server/sessions

Servelts can store data that is shared along *all* users. 



If we want USER specific data stored on the server: we use SESSIONS



### Button Example: 

Stores how many times a button is pressed:

```html
<!DOCTYPE html>
<html>
<head>
	<title>Click the Button!</title>
</head>
<body onload="getChats()">
	<h1>Click the button!</h1>
	<form action="/click" method="POST">
		<input type="submit" value="Click me!">
	</form>
</body>
</html>
```



Servlet for this: 

```java
import java.io.IOException;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;


public class ButtonServlet extends HttpServlet {
  private int clickCount = 0;
  @Override
 	clickCount++;
		
	public void doPost(HttpServletRequest request, HttpServletResponse response) throws IOException, ServletException {
		clickCount++;
		response.getOutputStream().println("<h1>You clicked the button " + clickCount + " times </h1>");
		response.getOutputStream().println("<p>Click <a href=\"/index.html\">here</a> to go back to the button.</p>");
	}	
}

```

Servlet class handles POST request by incrementing a clickCount variable - it then renders the value along with link back 

**You alwasy need an .xml that maps the URL that a client calls to your servlet**

Here you have your: 

```xml
<web-app
	xmlns="http://xmlns.jcp.org/xml/ns/javaee"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
		http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
	version="3.1">

	<servlet>
		<servlet-name>ButtonServlet</servlet-name>
		<servlet-class>ButtonServlet</servlet-class>
	</servlet>

	<servlet-mapping>
		<servlet-name>ButtonServlet</servlet-name>
		<url-pattern>/click</url-pattern>
	</servlet-mapping>

</web-app>
```



If you have MULTIPLE users, you could do it by storing cookies but this is the **hard** way to do it. 

Another approach would be to store a unique identifier, like a random 128-digit number, in the cookie instead. 

We’d then send that number with every request, so the server would be able to tell which user each request came from. The server could also keep something like a `Map<String, UserData>` where the keys are the 128-digit numbers and `UserData` is an object that contains all of the stuff we want to track for each user.



### Sessions

defintion: tracking a user from one request to the next: **session**

- starts when you visit webpage ( a unique id is stored in cookie on computer)
- when you make requests, the identifier is sent with yoru request and so the server can store data specific to you 
- session ends when cookies expires

Server Side Code: 

You get session data from a servlet class: request.getSession

```java
HttpSession session = request.getSession(); 
```

The `HttpSession` class contains functions for setting and getting user-specific data. For example we might store a value using the `setAttribute()` function:

```java
session.setAttribute("message", "Happy Coding!");
```

And then in subsequent requests, we could fetch that data using the `getAttribute()` function:

```java
String message = (String) session.getAttribute("message");
```



Button Example using sessions:

```java
import java.io.IOException;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

public class ButtonServlet extends HttpServlet{

	@Override
	public void doPost(HttpServletRequest request, HttpServletResponse response) throws IOException, ServletException {
		
		int clickCount;
		
		//get the session, which contains user-specific data
		HttpSession session = request.getSession();
		
		if(session.getAttribute("clickCount") != null){
			//we've already stored the clickCount in a previous request, so get it
			clickCount = (int) session.getAttribute("clickCount");
		}
		else{
			//we haven't stored the clickCount for this user yet, start it at zero
			clickCount = 0;
		}
		
		clickCount++;
		
		//store the new clickCount in the session
		session.setAttribute("clickCount", clickCount);
		
		//output the clickCount for this user
		response.getOutputStream().println("<h1>You clicked the button " + clickCount + " times.</h1>");
		response.getOutputStream().println("<p>Click <a href=\"/index.html\">here</a> to go back to the button.</p>");
	}	
}
```

Now instead of using a `clickCount` variable in our servlet class that’s shared among all users, we use a session attribute that’s specific to each user!!

### Without Cookies

Remember that by default, session logic is built using a cookie that keeps track of a unique identifier for each user. But keep in mind that users can disable cookies! In this case, the default setup will no longer work.

To get around this, you’ll have to append the unique identifier (called the **session ID**) to each URL in your web app.

I won’t go into too much detail here because this isn’t really a common case anymore, but if you do encounter this issue just know that there are workarounds available.



### Login 

```java
import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

	@Override
	public void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException, ServletException {
		//in real life you would probably use JSP to render the login
		response.getOutputStream().println("<h1>Login</h1>");
		response.getOutputStream().println("<hr/>");
		response.getOutputStream().println("<form action=\"/login\" method=\"POST\">");
		response.getOutputStream().println("<span>Username: </span>");
		response.getOutputStream().println("<input type=\"text\" name=\"username\">");
		response.getOutputStream().println("<span>Password: </span>");
		response.getOutputStream().println("<input type=\"password\" name=\"password\">");
		response.getOutputStream().println("<input type=\"submit\" value=\"Submit\">");
		response.getOutputStream().println("</form>");
	}
	

@Override
	public void doPost(HttpServletRequest request, HttpServletResponse response) throws IOException, ServletException {
	
		String username = request.getParameter("username");
		String password = request.getParameter("password");
		
		//this is just pseudocode!
		if(username and password are correct){
			request.getSession().setAttribute("user", username);
			response.sendRedirect("/home"); // sets user session attribute to username
		}
		else{
			//in real life you'd probably just render the login screen again
			response.getOutputStream().println("<h1>Wrong username or password.</h1>");
		}

		
	}
}
```

- The `doGet()` function just renders a login form. In real life we’d probably use a JSP file to render it, but I wanted to keep this example as short as possible.

- If they are, the code sets a `user` session attribute to the username. 





Example continued: Login Servlet: 

```java
import java.io.IOException;
import java.util.HashMap;
import java.util.Map;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class LoginServlet extends HttpServlet { 


	/**
	 * Map of usernames to their passwords.
	 * Note that storing passwords like this is a bad idea. This is just a simple example!
	 */
	private Map<String, String> usernamePasswordMap = new HashMap<>();
	public LoginServlet(){
		usernamePasswordMap.put("Ada", "bad password one");
		usernamePasswordMap.put("Grace", "bad password two");
		usernamePasswordMap.put("Stanley", "bad password three");
	}
  
  
	@Override
	public void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException, ServletException {
		response.getOutputStream().println("<h1>Login</h1>");
		response.getOutputStream().println("<hr/>");
		response.getOutputStream().println("<form action=\"/login\" method=\"POST\">");
		response.getOutputStream().println("<span>Username: </span>");
		response.getOutputStream().println("<input type=\"text\" name=\"username\">");
		response.getOutputStream().println("<span>Password: </span>");
		response.getOutputStream().println("<input type=\"password\" name=\"password\">");
		response.getOutputStream().println("<input type=\"submit\" value=\"Submit\">");
		response.getOutputStream().println("</form>");
	}
	
	@Override
	public void doPost(HttpServletRequest request, HttpServletResponse response) throws IOException, ServletException {
	
		String username = request.getParameter("username");
		String password = request.getParameter("password");
		
		if(usernamePasswordMap.containsKey(username) && usernamePasswordMap.get(username).equals(password)){
			request.getSession().setAttribute("user", username);
		}
		
		response.sendRedirect("/index.jsp");
	}

}
```



JSP index file:

```html
<!DOCTYPE html>
<html>
	<head>
		<title>Login Example Home</title>
	</head>
	<body>
	
		<% if(request.getSession().getAttribute("user") != null){ %>
			<p>Hello <%= request.getSession().getAttribute("user") %>!</p>
		<% } else{ %>
			<p><a href="/login">Login</a></p>
		<% } %>
	
		<h1>Users</h1>
		<ul>
			<li><a href="/users/Ada">Ada</a></li>
			<li><a href="/users/Grace">Grace</a></li>
			<li><a href="/users/Stanley">Stanley</a></li>
		</ul>
	</body>
</html>
```

The only interesting thing here is the `if` statement at the top of the `<body>` tag. It checks whether the session contains the `user` attribute, which is how we know a user is logged in. If so, it renders a greeting to the user, and if not, it renders a link to the login page.



We also have to map the `/login` URL to our servlet using a `web.xml` file:

```xml
<web-app
	xmlns="http://xmlns.jcp.org/xml/ns/javaee"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
		http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
	version="3.1">

	<servlet>
		<servlet-name>LoginServlet</servlet-name>
		<servlet-class>LoginServlet</servlet-class>
	</servlet>

	<servlet-mapping>
		<servlet-name>LoginServlet</servlet-name>
		<url-pattern>/login</url-pattern>
	</servlet-mapping>

</web-app>
```



Viewing and editing profiles:



Now that we have that, let’s create the servlet that handles viewing and editing user profiles:

```java
import java.io.IOException;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class UserServlet extends HttpServlet {
	
	/**
	 * Map of usernames to their descriptions.
	 */
	private Map<String, String> usernameDescriptionMap = new HashMap<>();
	
	public UserServlet(){
		usernameDescriptionMap.put("Ada", "I love poetical science!");
		usernameDescriptionMap.put("Grace", "I love nanoseconds!");
		usernameDescriptionMap.put("Stanley", "I love cat food!");
	}

	@Override
	public void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException, ServletException {
		
		String loggedInUser = (String) request.getSession().getAttribute("user");

		String requestUrl = request.getRequestURI();
		String requestedUsername = requestUrl.substring(requestUrl.lastIndexOf("/") + 1);
		
		if(loggedInUser == null){
			//user is not logged in
			response.getOutputStream().println("<h1>You have to be logged in to do that.</h1>");
		}
		else if(!loggedInUser.equals(requestedUsername)){
			//user is viewing somebody else's page
			response.getOutputStream().println("<h1>" + requestedUsername + "'s Page</h1>");
			response.getOutputStream().println("<p>" + usernameDescriptionMap.get(requestedUsername) + "</p>");
		}
		else{
			//user is viewing their own page, show form
			response.getOutputStream().println("<h1>Your Page</h1>");
			response.getOutputStream().println("<hr/>");
			response.getOutputStream().println("<form action=\"\" method=\"POST\">");
			response.getOutputStream().println("<p>Enter some info about yourself:</p>");
			response.getOutputStream().println("<textarea name=\"description\">" + usernameDescriptionMap.get(requestedUsername) + "</textarea>");
			response.getOutputStream().println("<input type=\"submit\" value=\"Submit\">");
			response.getOutputStream().println("</form>");
		}
	}
	
	@Override
	public void doPost(HttpServletRequest request, HttpServletResponse response) throws IOException, ServletException {
		
		String loggedInUser = (String) request.getSession().getAttribute("user");
		
		String requestUrl = request.getRequestURI();
		String requestedUsername = requestUrl.substring(requestUrl.lastIndexOf("/") + 1);

		if(loggedInUser == null){
			//user is not logged in
			response.getOutputStream().println("<h1>You have to be logged in to do that.</h1>");
		}
		else if(!loggedInUser.equals(requestedUsername)){
			//user is submitting somebody else's page
			response.getOutputStream().println("<h1>You can't change somebody else's user page.</h1>");
		}
		else{
			//user is submitting their own page
			String description = request.getParameter("description");
			usernameDescriptionMap.put(loggedInUser, description);
		
			//just redirect to a GET request
			response.sendRedirect(request.getRequestURI());
		}
	}
}
```

- First, this code creates a `Map`that contains a description for each user. In real life you’d probably read these from a file or a database, but for now we’re just hardcoding everything.

- The `doGet()` function detects both the logged-in user (from the session attribute) and the requested username (from the URL).
  -  If the user is not logged in, an error message is shown. 
  - If the user is viewing a user other than themselves, the page just shows the requested user’s description. 
  - If the user is viewing their own page, the page shows a form that allows them to change their description.

- The `doPost()` function also gets both the logged-in user and the requested username.
  -  If the user is not logged in or they’re trying to edit another user’s page, they see an error message. 
  - But if the user is editing their own page, then the `usernameDescriptionMap` is updated to store the new description.

We also need to update our `web.xml` file:

```xml
<web-app
	xmlns="http://xmlns.jcp.org/xml/ns/javaee"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
		http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
	version="3.1">

	<servlet>
		<servlet-name>LoginServlet</servlet-name>
		<servlet-class>LoginServlet</servlet-class>
	</servlet>

	<servlet-mapping>
		<servlet-name>LoginServlet</servlet-name>
		<url-pattern>/login</url-pattern>
	</servlet-mapping>
	
	<servlet>
		<servlet-name>UserServlet</servlet-name>
		<servlet-class>UserServlet</servlet-class>
	</servlet>

	<servlet-mapping>
		<servlet-name>UserServlet</servlet-name>
		<url-pattern>/users/*</url-pattern>
	</servlet-mapping>

</web-app>
```