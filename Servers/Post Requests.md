# Post Requests

Following: https://happycoding.io/tutorials/java-server/post

### GET Requests

Client getting stuff from server:

- we have been writing HTML that a client can request from a server
- learned how to write a servlet or JSP that creates HTML content tha tis then sent to the client 
- 

### URLs and Query Parameters

Parameter

```
http://localhost:8080/names.html?name=Ada
```

- request names.html and pass through the name parameter of Ada

URL:

```
http://localhost:8080/names/Ada
```

- using servlet mapped to something like `/names/*` to allow clients to request any name. We could then use server-side Java code to parse the URL and get the name.



### Post Requests

- If `GET` requests are the client asking for stuff from a server, `POST` requests are the client sending data to a server.
- Use a `<form>` tag in HTML content. 



```html
<!DOCTYPE html>
<html>
	<head>
		<title>Enter Your Name</title>
	</head>
	<body>
		<h1>What's your name?</h1>
		<form action="/hello" method="POST">
			<input type="text" name="name" value="Ada">
			<br/><br/>
			<input type="submit" value="Submit">
		</form>
	</body>
</html>
```

"form"

- action: specifies URL to send the data to
- method: specifices data that should use POST request 



**If you have a form that submits a POST request, what is the serverside code that processses that request?**

SERVLET CLASS! 

```java
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class GreetingServlet extends HttpServlet {
	
	@Override
	public void doPost(HttpServletRequest request, HttpServletResponse response) {
		String nameString = request.getParameter("name");
		response.getOutputStream().println("<h1>Hello " + name + "!</h1>")
	}

}

```

doPost() gets teh name paramater and outputs a basic HTML greeting 

- add an web.xml (always goes into WEB-INF)

  ```xml
  <web-app
  	xmlns="http://xmlns.jcp.org/xml/ns/javaee"
  	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  	xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
  		http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
  	version="3.1">
    
  <servlet> 
    <servlet-name>GreetingServlet</servlet-name>
    <servlet-class>GreetingServlet</servlet-class>
  </servlet>
  
  <servlet-mapping>
    <servlet-name>GrettingServlet</servlet-name>
    <url-pattern>/hello</url-pattern>
  </servlet-mapping>
  
  </webapp>
  ```



### Postback: 

posting data toteh same URL the form is access from = post back 

- servlet that handels both a GET and POST

Example: 

- handles a `/chat` URL
- receives a `POST`request, it adds a message to the chat
- receives a `GET` request, it renders all of the chats it has received- and adds a form that allows a user to send a `POST` request. 





```java
import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class ChatServlet extends HttpServlet {
	private List<String> messages = new ArrayList<String>();
	
	@Override
	public void doPost(HttpServletRequest request, HttpServletResponse response) throws IOException, ServletException {
		String nameString = request.getParameter("name");
		String messageString = request.getParameter("message");
		String chatMessageString = nameString + ":" + messageString; 
		
		messages.add(chatMessageString); // add the new message to your string
		response.getOutputStream().println("<p>Your message has been received.</p>");
		response.getOutputStream().println("<p>Click <a href=\"/chat\">here</a> to go back to the chat.</p>");
	}
	
	
	@Override
	public void doGet(HttpServletRequest request, HttpServletResponse response)throws IOException, ServletException {
		response.getOutputStream().println("<h1>Chat Web App</h1>");
		response.getOutputStream().println("<hr/>");
		
		for (int i = 0; i < messages.size(); i++ ) {
			response.getOutputStream().println("<p>" + messages.get(i) + "</p>");
		}
		response.getOutputStream().println("<hr/>");
		
		response.getOutputStream().println("<form action=\"/chat\" method=\"POST\">");
		response.getOutputStream().println("<input type=\"text\" name=\"name\" value=\"Ada\">");
		response.getOutputStream().println("<input type=\"text\" name=\"message\" value=\"Happy coding!\">");
		response.getOutputStream().println("<input type=\"submit\" value=\"Send\">");
		response.getOutputStream().println("</form>");
	} 
```



- doPost(): gets name + messages from posted data,
  - combines them into a chat message
  - adds it to the list of string values (messages)
  - then renders a message telling the user it was recieved & gives a link back to the chat
- doGet(): 
  - loops through list (message) and renders each message as a <p> element
  - then renders a form that contains a name & message input text box & send button 
    - the action of this form is teh URL linked to the message in the doPost() function
    - 

### Redirecting After Post 

Redirect user back to the GET request after the POST request completes using the `response.sendRedirect()`

```java
@Override
	public void doPost(HttpServletRequest request, HttpServletResponse response) throws IOException, ServletException {
		String name = request.getParameter("name");
		String message = request.getParameter("message");
		String chatMessage = name + ": " + message;
		messages.add(chatMessage);

		response.sendRedirect("/chat");
	}
```

The `"/chat"` parameter tells the client to redirect to the `/chat` URL.

Chat app now works this way: 

- The user visits `/chat` in their browser, which sends a `GET` request to the server.
- This calls the `doGet()` function in our servlet, which renders the chats (initially empty) along with the message form.
- When a user submits the form, the client sends a `POST` request to our server.
- Because the form’s `action` attribute is also the `/chat` URL, it calls the `doPost()`function in our servlet, which saves the message and tells the client to redirect back to the `/chat` URL.
- The client then sends another `GET` request to the `/chat` URL, which starts the process over.
- If the user refreshes the page, another `GET` request is sent, which is fine!





### JSP

Side note: for the servlet: 

```java
// when you are responding and your output is HMTL code (BAD PRACTICE)
response.getOutputStream().println("<h1>Hello " + name + "!</h1>");
 

// when you are passing along the info to a JSP, that will do the html
request.setAttribute("message", message);
request.getRequestDispatcher("WEB-INF/HelloWorld.jsp").forward(request,response);
```

