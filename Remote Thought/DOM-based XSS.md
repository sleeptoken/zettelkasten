
2024-06-05 12:47

Source: #tryhackme #web 

Tags: [[XSS]] 

- **DOM-based XSS**: This attack exploits vulnerabilities within the Document Object Model (**DOM**) to manipulate existing page elements without needing to be reflected or stored on the server. This vulnerability is the least common among the three.
DOM-based XSS is completely browser-based and does not need to go to the server and back to the client

 The DOM is a programming interface representing a web document as a tree. The DOM makes it possible to programmatically access and manipulate the different parts of a website using JavaScript.
We can view the DOM tree using the web browser’s built-in Web Developer’s Tools.
Using [[JavaScript]], you can manipulate the DOM tree -
- create a new element using`document.createElement()`
- add a child to any element using `element.append()`

avoiding adding user input directly with `document.write()`. Instead, we first escaped the user input using `encodeURIComponent()` and then added it to `textContent`.


### References
