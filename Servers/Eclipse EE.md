# Eclipse EE 

Following: https://happycoding.io/tutorials/java-server/eclipse-ee



EE: stands for *Enterprise Edition* aka you have tools for server development

- Java EE: .jar files, an dstuff taht comes with Jetty Server
  - When you add `servlet-api-3.1.jar` to your classpath so you can use the `HttpServlet` class, you’re using Java EE. 
  - When you use JSP, you’re using Jave EE!



Creating a new project in Eclipse EE: 

Go to `File > New > Project...` and in the dialog that pops up, go to `Web > Dynamic Web Project`and click `Next`.



<u>How the project is organized:</u>

- `Java Resources`: This is where servlets and other Java classes will go.
- `JavaScript Resources`: JavaScript files in your webpage will show up here so you can edit them.
- `WebContent`: This is the top-level publically available directory. This is where <u>static</u> files go.
- `WEB-INF`: Files in here are <u>hidden</u> from public access. This is where `web.xml` and other files you don’t want users to access directly will go.



### KEY POINT:

No matter how you write code, the process is the same:

- you create a `web.xml` file that maps a URL to a servlet class
- you create a servlet class that contains your logic
-  you create JSP files that render your view, which can use static files in the visible directory. 
- You then run the server and visit your web app in a browser.
