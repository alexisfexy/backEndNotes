# AJAX and JSON

Following: https://happycoding.io/tutorials/javascript/ajax

## AJAX

Defintion: AJAX stands for Asynchronous JavaScript and XML

- comes with JavaScript
- object allows you to load an external file and add its content to your webpage

- a way to access form your webpage (not file system, it has to be on the internet)

### The XMLHttpRequest Object

Call the `XMLHttpRequest` constructor using the `new` keyword and storing the resulting object in a variable:

```javascript
var ajaxRequest = new XMLHttpRequest();
```

`onreadystatechange` points to a function that will be called as the external file is loaded

```javascript
ajaxRequest.onreadystatechange = function(){
	console.log("Ready state changed!");
	//more on this in a second
}
```

Then you call the `open()` function and pass it 3 parameters:

- The first parameter is what type of request this should be. We want to get some content, so we’ll use `"GET"` for now. Later you might use `"POST"` if you want to post content to a server.
- The second parameter is the URL that contains the stuff you want to load.
- The third parameter is a boolean: `true` if you want your code to keep going while the content is loaded, `false` if you want the code to stop and wait for the loading to finish. At this point you should always use `true`, otherwise your page can become unresponsive while the URL is loaded.

All of that to explain this line of code:

```java
ajaxRequest.open("GET", "http://happycoding.io/tutorials/javascript/example-ajax-files/simple-welcome.txt", true);
```

Call `send()` function to send the request to the URL you specified. The browser will fetch the contents and then call the `onreadystatechange` function you set.

```javascript
ajaxRequest.send();
```

```javascript
var ajaxRequest = new XMLHttpRequest();
ajaxRequest.onreadystatechange = function(){
	console.log("Ready state changed!");
	//more on this in a second
}
ajaxRequest.open("GET", "http://happycoding.io/tutorials/javascript/example-ajax-files/simple-welcome.txt", true);
ajaxRequest.send();
```



**The onreadystatechange Function:**

A-JAX: A is for *asyncrhonous*: which means that your code can keep running while the request is sent off 

- request is sent off, code keeps running, and **onstatechange is called with updates to let you know what is going on** 



**The readyState Variable**

We usually call teh onreadystatechanged message 4 times: 

- When AJAX establishes a connection with the server. *Hey I got directions to the file.*
- When the server receives the request. *Hey I’m at the server.*
- When the request is processed. *Hey I’m picking up the file now.*
- When the file is loaded. *Here’s your file!*

You can check which state AJAX is in using the readystate variable.

- it is an integer 1-4 for eeach of the above states (or 0 if somethign is wrong)



inside the `onreadystatechanged` function, you can check the `readyState` variable:

```javascript
ajaxRequest.onreadystatechange = function(){
	if(ajaxRequest.readyState == 1){
		console.log("Established server connection.");
	}
	else if(ajaxRequest.readyState == 2){
		console.log("Request received by server.");
	}
	else if(ajaxRequest.readyState == 3){
		console.log("Processing request.");
	}
	else if(ajaxRequest.readyState == 4){
		console.log("Done loading!");
	}
	else{
		console.log("Something went wrong. :(");
	}
}
```

^^ we usually only care if it is 4!! 



Once you know it is sucessful , you probably want to find the status: 

**The status Variable**

- tells things like: missing file, or we need a username & password, etc.
- holds the HTTP status code of the request 

*Checking for Success:*

```javascript
ajaxRequest.onreadystatechange = function(){
	if(ajaxRequest.readyState == 4){
		//the request is completed, now check its status
		if(ajaxRequest.status == 200){
			//our file has loaded!
		}
		else{
			console.log("Status error: " + ajaxRequest.status);
		}
	}
	else{
		console.log("Ignored readyState: " + ajaxRequest.readyState);
	}
}
```



**The responseText Variable:**

- holds content of file
- string value, we can do whatever we want 

For example, we could just set the content of one of the elements in our webpage to whatever is in the file:

```javascript
document.getElementById("welcome").innerHTML = ajaxRequest.responseText;
```



<u>Full Example</u>

```html
<!DOCTYPE html>
<html>
<head>
	<title>AJAX Example</title>
	
	<script>
		function getWelcome(){
			var ajaxRequest = new XMLHttpRequest();
			ajaxRequest.onreadystatechange = function(){
			
				if(ajaxRequest.readyState == 4){
					//the request is completed, now check its status
					if(ajaxRequest.status == 200){
						document.getElementById("welcome").innerHTML = ajaxRequest.responseText;
					}
					else{
						console.log("Status error: " + ajaxRequest.status);
					}
				}
				else{
					console.log("Ignored readyState: " + ajaxRequest.readyState);
				}
			
			
			}
			ajaxRequest.open('GET', 'http://happycoding.io/tutorials/javascript/example-ajax-files/simple-welcome.txt');
			ajaxRequest.send();
		}
	
	</script>	
</head>
<body onload="getWelcome()"> <!-- could also be onclick -->
	<p id="welcome"></p>
	<p>This is an example website.</p>
</body>
</html>
```



## JSON

Defintion: JSON stands for JavaScript Object Notation, and it’s a way to store data in a way that’s easy for JavaScript to understand. 

```javascript
var jsonString = '{"x": 7, "y": 42}';
var point = JSON.parse(jsonString);

console.log("The point is at " + point.x + ", " + point.y);
```

use JSON.parse to turn JSON nto Javascript object 



### JSON Arrays

Use square brackets `[ ]` and a comma-separated list of JSON objects, like this:

```javascript
var jsonArrayString = '[ {"x": 7, "y": 42}, {"x": 0, "y": 0}, {"x": 2.7, "y": 3.14} ]';
```

Example: array that contains 3 objects with `x` and `y` properties.

```javascript
var jsonArrayString = '[ {"x": 7, "y": 42}, {"x": 0, "y": 0}, {"x": 2.7, "y": 3.14} ]';
var pointArray = JSON.parse(jsonArrayString);
for(var i = 0; i < pointArray.length; i++){
	var point = pointArray[i];		
	console.log("The point is at " + point.x + ", " + point.y);
}
```



### Nested JSON

```json
'[
	{
		"name": "Example One",
		"numbers": [1, 2, 3],
		"point": {"x":7, "y":42}
	},
	
	{
		"name": "Example Two",
		"numbers": [4, 5, 6],
		"point": {"x":2.7, "y":3.14}
	}
]'
```

Example: array that contains two objects. Each object has three properties: a `name` property that points to a string value, a `numbers` property that points to an array, and a `point` property that points to an object.



To print out first object:

```javascript
var parsedArray = JSON.parse(jsonArray);
var firstPoint = parsedArray[0].point;
console.log("The first point is at " + firstPoint.x + ", " + firstPoint.y);
```



## Using AJAX + JSON

AJAX can be used to fetch JSON

```json
{"randomMessages": [
	{"message": "Welcome to my site!", "color": "red"},
	{"message": "Thanks for visiting!", "color": "blue"},
	{"message": "Hello visitor!", "color": "green"},
	{"message": "Meow!", "color": "orange"},
	{"message": "Bienvenido!", "color": "purple"}
]}
```

AJAX fetches the data:

```javascript
//turn JSON into object
var jsonObj = JSON.parse(ajaxRequest.responseText);

//get random object from array
var randomMessagesArray = jsonObj.randomMessages;
var randomIndex = Math.floor(Math.random()*randomMessagesArray.length);
var messageObj = randomMessagesArray[randomIndex];

//use that object to set content and color
document.getElementById("welcome").innerHTML = messageObj.message;
document.getElementById("welcome").style.color = messageObj.color;
```

- turns the JSON into an object that we can work with in our code
-  takes that object and gets a random `message` object from i
- uses that object to set the content and color of our message.





AJAX was developed to deal with these issues. Using AJAX, a website can work like this:

- A visitor goes to a URL that points to a particular `.html` file.
- That `.html` file contains the “core content” of a page: that might just be its layout and some JavaScript for fetching additional content.
- Then the webpage makes a bunch of smaller AJAX requests to fetch the additional content.
- To visit another page on the website, the visitor clicks a button that triggers AJAX to load just that section, without reloading stuff like the navigation bar. (Note that JavaScript can change what’s in the URL bar, so it might even look like you’re visiting another page!)



## JQuery

JQuery  provides shortcuts for using AJAX!! 

First example that displays a simple welcome message that it fetches using AJAX can be shortened to a single line of code using the JQuery library:

```javascript
function getWelcome(){
	$("#welcome").load('http://happycoding.io/tutorials/javascript/example-ajax-files/simple-welcome.txt');
}
```

*To Do: Review JQuery if going to use AJAX*