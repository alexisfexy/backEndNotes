# Client Server Communication

https://happycoding.io/tutorials/java-server/client-server



### Server Side Rendering: 

- eg. clienet requests /time.html URL from our server, and we reply with HTML content that generated using *Java Code* rather than HTML coming from an .html file 

```java
import java.io.IOException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/time.html") // tells server which URL the code shoudl run for 
public class UnixTimeServlet extends HttpServlet {

  @Override
  public void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException {
    long unixTimeSeconds = System.currentTimeMillis() / 1000;

    response.setContentType("text/html;");
    response.getWriter().println("<h1>Unix Time</h1>");
    response.getWriter().println("<p>Current Unix time: " + unixTimeSeconds + "</p>");
    response.getWriter().println("<p>(<a href=\"\">Refresh</a>)</p>");
  }
}
```



### Client Side Rendering: 

**AJAX**: Use JS to request content from a server

Request Flow: 

- types url
- Sends request to server
- response with some *inital content* (mostly static: eg. Nav bar), + js code for building dynamic parts of page
- js code makes *another request* for more content 
- server replies with requested content (usually dynamic content: XTML, JSON, HTML, plain text, etc.)
- Reponse comes back to JS code, and JS code parses the content to build out the erst of the UI using JS functions
- User sees inital content first, maybe a loading bar while dynamic is requested then populated

#### Server Endpoints

- **Endpoint: ** url that repsonses with data on our server 

- Example continued: this endpoint needs to return current Unix time 

  - ```java
    import java.io.IOException;
    import javax.servlet.annotation.WebServlet;
    import javax.servlet.http.HttpServlet;
    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpServletResponse;
    
    @WebServlet("/time")
    public class UnixTimeServlet extends HttpServlet {
    
      @Override
      public void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException {
        long unixTimeSeconds = System.currentTimeMillis() / 1000;
    
        response.setContentType("text/html;");
        response.getWriter().println(unixTimeSeconds);
      }
    }
    ```

- essentially the same as above but does not reply with HTML, instead is just the single value of Unix Time. 
  
  - usually you will see more complex JSON back



#### Fetch

- jaascript code that requests the data from the server and uses it to build teh HMTL of the page: 

- ```html
  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="UTF-8">
      <title>Unix Time</title>
      <script>
        function requestUnixTime() {
          const resultContainer = document.getElementById('result');
          resultContainer.innerText = 'Loading...';
  
          fetch('/time') // request data from the server
          .then(response => response.text()) // convert the response to raw text
          .then((unixTime) => {
            // build the HTML of the page
            resultContainer.innerText = unixTime;
          });
        }
      </script>
    </head>
    <body onload="requestUnixTime();">
      <h1>Unix Time</h1>
      <p>Current Unix time: <span id="result"></span></p>
      <p>(<a href="">Refresh</a>)</p>
    </body>
  </html>
  ```



### Sending Data to Server



#### Query Parameters

- key=value parameters appended at end of URL after a ? question mark character

  - can separte multiple key values with & sign

- example: 

  - ```
    https://www.youtube.com/watch?v=PBslkj&t=42
    ```

  - in this example, youtuble loads the video & skips ahead 42 seconds

- on server side: 

  - ```java
    import javax.servlet.annotation.WebServlet;
    import javax.servlet.http.HttpServlet;
    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpServletResponse;
    
    @WebServlet("/hello")
    public class HelloServlet extends HttpServlet {
    
      @Override
      public void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException {
        String name = request.getParameter("name");
        response.setContentType("text/html;");
        response.getWriter().println("<h1>Hello " + name + "!</h1>");
      }
    }
    ```

    - handels requests for /hello URL: gets value of name paramter, and uses it to response with some HTML content 

#### URL Path

- Query parameters not part of the URL path, they come *after* the URL

  - you can pass data in URL too!

- Setup handler on server of URL

  - e.g . set up handler for any URL starting with /hello

    - so /hello/ada, /hello/Grace, etc all trigger same code
    -  this code is parsed to see what name is passed through: 

  - ```java
    import java.io.IOException;
    import javax.servlet.annotation.WebServlet;
    import javax.servlet.http.HttpServlet;
    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpServletResponse;
    
    @WebServlet("/hello/*") // uses wildcard URL to match any url starting with /hello/
    public class HelloServlet extends HttpServlet {
    
      @Override
      public void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException {
    
        // name is whatever comes after /hello/
        String name = request.getRequestURI().substring("/hello/".length());
        // gets name after /hello/ 
    
        response.setContentType("text/html;");
        response.getWriter().println("<h1>Hello " + name + "!</h1>");
      }
    }
    ```



#### POST Requests

Downfalls of URL paths & query parameters: 

- url show up in your history & server logs
- limit for how long url can be
-  url is character of strings, but if you need to send binary data like an image file you cant 



All requests above are GET requests: 

- client wants to GET content form server 
- doGet()

POST: client wants to SEND (post) content to a server,

- eg. filling out a form, when you hit submit



```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>Form Submission</title>
  </head>
  <body>
    <h1>Hello Form Submission</h1>
    <p>Type your name and press Submit:</p>

    <form method="POST" action="/hello"> <!-- this tells browser to make POST request to /hello when submit is pressed--> 
      <input name="name" />
      <br/>
      <button>Submit</button>
    </form>

  </body>
</html>
```



Server Side: 

```java
import java.io.IOException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/hello")
public class HelloServlet extends HttpServlet {

  @Override
  public void doPost(HttpServletRequest request, HttpServletResponse response) throws IOException {
    // the name parameter comes from the form
    String name = request.getParameter("name");
    response.setContentType("text/html;");
    response.getWriter().println("<h1>Hello " + name + "!</h1>");
  }
}

```



### Local Servers

- usually will deply to *local* server - meaning you are running the server & client on your computer 
- when you are done then you publish to a remoe server for people to interacti with it

 
