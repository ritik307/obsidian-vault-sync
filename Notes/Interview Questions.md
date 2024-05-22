 
- **jwt token vs normal token** (normal token cant store extra data but in jwt we can store as much as we want)
    
- **cookies,local storage,sessions**
    
    - **Cookies**:
        - **Storage:** Cookies are small pieces of data (usually text) stored in a user's browser. Each cookie has a maximum size limit of around 4KB.
        - **Persistence:** Cookies can be either persistent (stored on the user's device for a specified duration) or session cookies (which expire when the user closes the browser).
        - **Usage:** Cookies are often used to store small amounts of data like user preferences, session identifiers, and tracking information.
        - **Security:** Cookies have some security concerns, such as being **vulnerable to** **cross-site scripting (XSS) attacks**. Developers can mitigate this by using **HttpOnly** and Secure flags.
    - **Local Storage**:
        - **Storage:** Local Storage is a part of the Web Storage API that allows web applications to store data as key-value pairs in the user's browser. It provides a much larger storage capacity compared to cookies, typically around 5-10MB per domain.
        - **Persistence:** Data stored in Local Storage persists even after the user closes the browser and can be long-lasting unless explicitly cleared.
        - **Usage:** Local Storage is typically used for caching data, saving user preferences, and offline web applications.
        - **Security:** Local Storage is more secure than cookies regardin**g XSS attacks because the data is not sent with every HTTP request**, but it's still vulnerable to other security threats like **cross-site request forgery (CSRF).**
    - **Sessions**:
        - **Storage:** Sessions are a server-side mechanism for storing data on the server about a specific user's session. In a typical web application, a unique session identifier is stored in a cookie on the client's side, and the server associates that identifier with the user's data.
        - **Persistence:** Sessions are typically short-lived, expiring after a period of inactivity or when the user logs out.
        - **Usage:** Sessions are used to maintain user state and are often utilized for tasks like user authentication and authorization.
        - **Security:** Sessions are more secure in terms of data storage because sensitive information is kept on the server, but developers must be careful to prevent session hijacking and protect against session fixation attacks.
- **Webpacks vs babel**
    
    - **Webpack**:
        1. **Purpose**: Webpack is a powerful module bundler. It is primarily used for bundling and optimizing JavaScript, CSS, and other assets for web applications.
        2. **Features**:
            - Module Bundling: Webpack can take various JavaScript modules, along with their dependencies, and bundle them into a single or multiple output files. This is essential for optimizing performance and reducing the number of HTTP requests.
            - Code Splitting: Webpack allows you to split your code into smaller chunks that can be loaded on demand, reducing the initial load time of your application.
            - Asset Management: It can handle various assets like images, fonts, and styles, processing them and including them in your bundle.
            - Extensibility: Webpack is highly extensible through plugins and loaders, making it adaptable to different project requirements.
            - Development Server: Webpack provides a development server that supports features like live reloading and hot module replacement for a smoother development experience.
        3. **Use Case**: Webpack is primarily used to bundle and optimize JavaScript and other assets for web applications. It's essential for modern front-end development, where modular code and performance optimization are key.
    - **Babel**:
        1. **Purpose**: Babel is a JavaScript compiler that primarily focuses on enabling developers to write and use the latest ECMAScript features in their code, ensuring it's compatible with older browsers.
        2. **Features**:
            - Transpilation: Babel takes modern JavaScript code (ES6+ or JSX) and transpiles it into an older version of JavaScript (typically ES5) that is compatible with a wide range of browsers.
            - Plugin System: Babel is highly customizable and extensible through a plugin system. Developers can add or create plugins to tailor the compilation process to their specific needs.
            - React JSX Transformation: Babel is commonly used for transforming JSX code (used in React) into standard JavaScript.
        3. **Use Case**: Babel is primarily used to enable the use of the latest JavaScript language features and syntax, especially for projects targeting older browsers or environments where those features are not natively supported.
- **JWT (if I copy your JWT token and your token hasn't expired yet how will you protect me from using your account.)**
    
    - JWT (JSON Web Tokens) are a popular method for securely transmitting information between parties in a compact, self-contained manner. They are often used for user authentication and authorization. While JWTs can be a secure way to manage user sessions, there are certain security considerations to keep in mind, including the scenario you've described.
        1. **Short Token Lifetimes**: Keep the expiration time (the "exp" claim) of JWTs relatively short. This means that even if someone obtains your token, it won't be valid for a long time.
        2. **Refresh Tokens**: Instead of relying solely on long-lived access tokens, use refresh tokens in combination with short-lived access tokens. When the access token expires, the client can use a refresh token to obtain a new access token without requiring the user to log in again.
        3. **Secure Transmission**: Ensure that JWTs are transmitted securely. Use HTTPS to protect data in transit to prevent interception and theft of tokens.
        4. **Implement Token Revocation**: Have mechanisms in place to revoke tokens if they are compromised or are no longer needed. This can include maintaining a list of blacklisted tokens or implementing OAuth 2.0 token revocation mechanisms.
        5. **Implement IP Checks**: You can tie tokens to specific IP addresses, making it more difficult for someone to use a token from a different location.
        6. **One-Time Use Tokens**: Some systems implement one-time use tokens, meaning the token can only be used once, rendering any stolen tokens useless after their first use.
        7. **User Authentication Challenges**: Implement additional challenges, like asking the user for their password or another factor, for sensitive operations or when re-authentication is required.
        8. **Audit Trails**: Keep a detailed audit trail of token usage. This can help identify suspicious activity and provide evidence if needed.
        9. **Use Good Practices for Storing Tokens**: If you're storing tokens on the client-side, use secure storage mechanisms, like HttpOnly cookies for cookies or browser storage with proper security measures.
- **secure vs httponly flag in cookies**
    
    - **Secure Flag**:
        - **Purpose**: The **`Secure`** flag is used to ensure that a cookie is only transmitted over secure, encrypted connections, typically using HTTPS.
        - **Usage**: When a cookie is set with the **`Secure`** flag, it can only be transmitted to the server if the connection is made over HTTPS. This provides protection against data interception and eavesdropping in transit.
        - **Security Benefit**: It prevents the transmission of cookies over non-secure (HTTP) connections, which can help protect sensitive information like session tokens from being intercepted by attackers.
    - **HttpOnly Flag**:
        - **Purpose**: The **`HttpOnly`** flag is used to prevent client-side scripts (e.g., JavaScript) from accessing the cookie. It enhances security by protecting cookies from client-side attacks, such as Cross-Site Scripting (XSS).
        - **Usage**: When a cookie is set with the **`HttpOnly`** flag, JavaScript running on the page cannot read or manipulate the cookie's value. This mitigates the risk of attackers stealing sensitive cookie data via XSS attacks.
        - **Security Benefit**: It helps protect against XSS attacks by limiting the ability of malicious scripts to access or modify cookies.
    
    ```jsx
    Set-Cookie: sessionId=ABC123; Secure; HttpOnly
    ```
    
    - In practice, it's common to use both the **`Secure`** and **`HttpOnly`** flags together, especially for cookies that contain sensitive information, such as session tokens or authentication credentials. This combination ensures that the cookie is transmitted only over secure connections and that it is protected from client-side attacks.
- **Can we make a constructor using () ⇒**
    
    ```jsx
    function Person(name) {
      this.name = name;
    }
    
    const person1 = new Person("Alice");
    console.log(person1.name); // "Alice"
    ```
    
    - In this example, **`Person`** is a constructor function created using the **`function`** keyword. When you use the **`new`** keyword to instantiate an object from this constructor, it creates a new instance of **`Person`** with its own **`this`** context.
    
    ```jsx
    const Person = (name) => {
      this.name = name;
    };
    
    // This won't work as expected and will likely throw an error.
    const person2 = new Person("Bob");
    ```
    
    - Arrow functions, unlike regular functions, do not have their own **`this`** context. They capture the **`this`** value of the surrounding context. This makes them unsuitable for use as constructors because they cannot create new instances with their own **`this`** context.
    - In this example, attempting to use an arrow function as a constructor results in an error because it cannot create a new instance with its own **`this`** context.
- Ways to make objects in JS
    
    - **Object Literal**: You can create an object using object literal notation, which is a comma-separated list of key-value pairs enclosed in curly braces:
    
    ```jsx
    const person = {
      name: "Alice",
      age: 30,
    };
    ```
    
    - **Constructor Function**: You can create objects using constructor functions. Constructor functions are a way to create multiple objects with the same structure and methods. Here's an example:
    
    ```jsx
    function Person(name, age) {
      this.name = name;
      this.age = age;
    }
    
    const person1 = new Person("Alice", 30);
    const person2 = new Person("Bob", 25);
    ```
    
    - **Object.create()**:
        
        The ‘`Object.create()`’ method allows you to create a new object with the specified prototype object. This is often used for creating objects with a specific prototype chain:
        
        ```jsx
        const personPrototype = {
          introduce() {
            console.log(`Hi, I'm ${this.name} and I'm ${this.age} years old.`);
          },
        };
        
        const person = Object.create(personPrototype);
        person.name = "Alice";
        person.age = 30;
        ```
        
    - **Class Syntax (ES6)**: ES6 introduced class syntax for creating objects with constructors and methods. It's a more structured and readable way to define object blueprints:
        
    
    ```jsx
    class Person {
      constructor(name, age) {
        this.name = name;
        this.age = age;
      }
      introduce() {
        console.log(`Hi, I'm ${this.name} and I'm ${this.age} years old.`);
      }
    }
    
    const person1 = new Person("Alice", 30);
    const person2 = new Person("Bob", 25);
    ```
    
    - **Factory Functions**: A factory function is a function that returns a new object. This approach is flexible and allows you to encapsulate the creation logic:
    
    ```jsx
    function createPerson(name, age) {
      return {
        name,
        age,
        introduce() {
          console.log(`Hi, I'm ${this.name} and I'm ${this.age} years old.`);
        },
      };
    }
    
    const person1 = createPerson("Alice", 30);
    const person2 = createPerson("Bob", 25);
    ```
    
    - **Prototype**: You can use the prototype chain to create objects by extending existing ones. This is useful when you want to create objects that inherit properties and methods from a prototype object:
    
    ```jsx
    function Person() {}
    Person.prototype.name = "Alice";
    Person.prototype.age = 30;
    
    const person = new Person();
    ```
    
    - **Using the Spread Operator**: You can use the spread operator to create new objects by merging the properties of existing objects:
    
    ```jsx
    const defaults = { color: "red", size: "medium" };
    const customOptions = { size: "large" };
    const mergedOptions = { ...defaults, ...customOptions };
    ```
    
- **relative vs absolute vs fixed in CSS**
    
    - **Relative Positioning (`position: relative;`)**:
        
        - With relative positioning, an element is positioned relative to its normal position within the document flow.
        - It allows you to use properties like **`top`**, **`right`**, **`bottom`**, and **`left`** to shift the element from its original position without affecting the layout of other elements on the page.
        - The element remains in the document flow, and other elements are not affected by its new position.
        
        ```jsx
        .relative-box {
          position: relative;
          top: 20px;
          left: 30px;
        }
        ```
        
    - **Absolute Positioning (`position: absolute;`)**:
        
        - With absolute positioning, an element is positioned relative to the nearest ancestor with a position other than **`static`** (which is the default position for most elements).
        - It is removed from the normal document flow, and its position is determined by specifying **`top`**, **`right`**, **`bottom`**, and **`left`** properties.
        - It can overlap other elements if not properly managed, and it won't affect the layout of surrounding elements.
        
        ```jsx
        .container {
          position: relative;
        }
        
        .absolute-box {
          position: absolute;
          top: 0;
          left: 0;
        }
        ```
        
    - **Fixed Positioning (`position: fixed;`)**:
        
        - Fixed positioning is similar to absolute positioning, but the element is positioned relative to the viewport (the browser window).
        - It is also removed from the normal document flow and won't scroll with the page. It remains fixed in its position as the user scrolls the page.
        - It is often used for elements like navigation bars or "back to top" buttons that should stay visible as the user scrolls.
        
        ```jsx
        .fixed-box {
          position: fixed;
          top: 10px;
          right: 10px;
        }
        ```
        
- **what is CORS**
    
    - CORS, or Cross-Origin Resource Sharing, is a security feature implemented by web browsers to control and manage requests made to resources on different domains (origins) in web applications. It is a critical security mechanism that helps prevent certain types of web-based attacks and ensures that web applications can safely make requests to external resources while maintaining the same-origin policy.
    - Here's an overview of how CORS works and why it is important:
        - **Same-Origin Policy**: By default, web browsers enforce the same-origin policy, which means that web pages can only make requests to resources on the same domain (origin) from which the web page was served. This policy is in place to protect against potential security vulnerabilities. However, it can be restrictive when you need to fetch resources from different origins.
        - **CORS Implementation**: CORS is a way to relax the same-origin policy when necessary. It allows a web server to specify who can access its resources and under what conditions. The CORS mechanism involves the use of HTTP headers to specify which domains are allowed to access a resource. Here are some key CORS headers:
            1. **Origin Header**: When a browser makes a cross-origin request, it sends an **`Origin`** header to the target server, indicating the origin of the requesting site.
            2. **Access-Control-Allow-Origin Header**: The server, in response to the request, can include the **`Access-Control-Allow-Origin`** header to specify which origins are allowed to access the resource. This header can be a specific origin or a wildcard (**``**) to allow any origin.
            3. **Other Access Control Headers**: Additional headers like **`Access-Control-Allow-Methods`**, **`Access-Control-Allow-Headers`**, and **`Access-Control-Allow-Credentials`** can further define what methods, headers, and credentials are allowed in cross-origin requests.
    - **Preflight Requests**: For certain types of cross-origin requests (e.g., when using HTTP methods other than GET or POST or when sending custom headers), browsers may send a preflight request using the HTTP OPTIONS method to check with the server if the actual request is allowed. The server must respond to this preflight request with appropriate CORS headers.
- **what is a CDN(Content delivery network)**
    
    - A **Content Delivery Network (CDN)** is a distributed network of servers strategically located in various data centers around the world. CDNs are designed to improve the delivery and performance of web content, such as web pages, images, videos, stylesheets, scripts, and other assets, to end-users.
        1. **Content Distribution**: CDNs store and cache content, making it readily available to users from the server that is geographically closest to them. This reduces the physical distance between the user and the server, resulting in faster content delivery.
        2. **Load Balancing**: CDNs distribute traffic across multiple servers, which helps balance the load and ensures that web servers are not overwhelmed with requests during traffic spikes. This load balancing improves the availability and responsiveness of websites and applications.
        3. **Improved Latency and Performance**: By delivering content from servers that are strategically placed near end-users, CDNs reduce latency and improve the overall performance of websites and applications. This is especially beneficial for global audiences.
        4. **Distributed DDoS Protection**: Many CDNs include DDoS (Distributed Denial of Service) protection features. By distributing traffic across multiple servers and filtering malicious traffic, CDNs can help mitigate DDoS attacks and ensure the availability of web resources.
        5. **Bandwidth Savings**: CDNs help reduce the load on the origin server by caching content and serving it directly to users. This can result in substantial bandwidth savings, particularly for large files like videos and software downloads.
        6. **Content Optimization**: Some CDNs offer content optimization features, such as image compression, minification of scripts and styles, and adaptive video streaming. These optimizations improve the performance of websites and reduce data transfer costs.
        7. **SSL/TLS Offloading**: CDNs can offload SSL/TLS encryption and decryption, reducing the computational burden on the origin server and improving the security and performance of encrypted connections.
        8. **Global Reach**: CDNs have servers in numerous locations worldwide, allowing websites and applications to deliver content efficiently to a global audience, regardless of their geographic location.
        9. **Redundancy and High Availability**: CDNs offer redundancy and high availability, as content is replicated across multiple servers in different locations. This reduces the risk of single points of failure and ensures that content remains accessible even in the event of server outages.
- data format in CSS
    
- How will you optimise your React application from developing to deploying
    
- display: none vs visibility: hidden
    
- Aggregation in DB
    
- Design MongoDB schema for school(student, marks, class)
    
- box model
    

### React

- contextAPI in react

### Nodejs

- **ways to export in nodejs.**
    
    - **CommonJS Modules (module.exports and require)**: CommonJS is the module system used in Node.js by default.
    
    ```jsx
    //-----EXPORT-----
    const someFunction = () => {
      // ...
    };
    
    module.exports = {
      someFunction,
      someVariable: "Hello, World!",
    };
    
    //-----IMPORT-----
    const exampleModule = require('./example');
    
    console.log(exampleModule.someVariable);
    exampleModule.someFunction();
    ```
    
    - **ES6 Modules (export and import)**:
    
    ```jsx
    //------EXPORT-----
    const someFunction = () => {
      // ...
    };
    
    export const someVariable = "Hello, World!";
    export { someFunction };
    
    //----IMPORT-----
    import { someVariable, someFunction } from './example.mjs';
    
    console.log(someVariable);
    someFunction();
    
    ```
    
    - **Exports and Imports with `require` and `import` Combined**:
    
    ```jsx
    //-----EXPORT----
    const someFunction = () => {
      // ...
    };
    
    module.exports = {
      someVariable: "Hello, World!",
      someFunction,
    };
    
    //----IMPORT----
    const { someVariable, someFunction } = require('./example');
    import { anotherFunction } from './another-module.mjs';
    
    console.log(someVariable);
    someFunction();
    anotherFunction();
    ```
    
    - **Exporting and Requiring Class Instances**:
    
    ```jsx
    //-----EXPORT
    class MyClass {
      // ...
    }
    
    module.exports = new MyClass();
    
    //----IMPORT
    const myInstance = require('./example');
    myInstance.someMethod();
    ```
    
- middleware
    
- **process.nextTick() vs setImmediate()**
    
    - **`process.nextTick()`**:
        
        1. **Timing**: **`process.nextTick()`** callbacks are executed immediately after the current operation completes and before the event loop continues. They are placed at the beginning of the next tick or phase of the event loop.
        2. **Use Cases**: It is often used when you want to defer the execution of a function until the next tick of the event loop. It's useful for tasks like deferring heavy computations, breaking up long-running processes, or ensuring that certain code runs before any I/O operations.
        
        ```jsx
        process.nextTick(() => {
          console.log('This will be executed before the event loop continues.');
        });
        ```
        
    - **`setImmediate()`**:
        
    
    1. **Timing**: **`setImmediate()`** callbacks are executed in the next iteration of the event loop, after the current phase completes. They are placed at the end of the current tick/phase of the event loop.
    2. **Use Cases**: It is used when you want to ensure that a function is executed in a non-blocking way, but you don't need it to run immediately after the current operation. It's suitable for tasks that can yield to I/O operations or other events.
    
    ```jsx
    setImmediate(() => {
      console.log('This will be executed in the next iteration of the event loop.');
    });
    ```
    
- what is webhook
    
- what are streams in Node js
    
- **next() vs return next()**
    
    - **`next()`**:
        
        - **`next()`** is a function used to pass control to the next middleware function in the stack.
        - It is typically used to indicate that the current middleware has completed its task and that the next middleware in line should be executed.
        - The execution continues through the subsequent middlewares, and eventually, the response is sent to the client (or the route handler is executed).
        
        ```jsx
        app.use((req, res, next) => {
          // Do some work
          next(); // Pass control to the next middleware
        });
        
        app.get('/route', (req, res) => {
          // Handle the request
        });
        ```
        
    - **`return next()`**:
        
        - **`return next()`** is used when you want to terminate the execution of the current middleware and immediately pass control to the next middleware. It's often used to exit a middleware early if certain conditions are met.
        - It ensures that the subsequent code in the current middleware doesn't run, and control is immediately handed over to the next middleware.
        
        ```jsx
        app.use((req, res, next) => {
          if (someCondition) {
            return next(); // Exit this middleware and pass control to the next
          }
          // Continue with the current middleware
        });
        
        app.get('/route', (req, res) => {
          // Handle the request
        });
        ```
        

### Kafka

- kafka broker
- if all the kafka brokers are down for 1 sec how will you handle this
- kafka broker (partition,key)
- how does the broker know which partition topic data will be consumed
- we cannot use same consumer group id , what happen if we use single consumer group id in kafka
- what could have gone wrong if we have 1 topic and 1 partition and that message takes 10 min to consume

### Database

- transactions
    
- Sql vs NoSql
    
- Aggregation in DB
    
- Design MongoDB schema for school(student, marks, class)
    
- write a sql query of user table and order tables
    
    users -user_id, username, created_at
    
    Orders- order_id, user_id, order_status, created_at monthly acquired users for last 20 months
    
    acquired users that have registered and placed an order in the same month
    
    month , count
    
    Aprile-23, 50000
    

### DSA

- **Next greater element**
    
- **56. Merge Intervals(LEETCODE)**
    
- binary tree vs bst
    
- how to delete a node in BST
    
- **222. Count Complete Tree Nodes(LEETCODE)**
    
- **there is an infinitely long fence containing post.On each day of the face painted from post [pi, pj].There is N such days on which the fence will be painted >find the number of posts containing more than or equal to k layers of colours**
    
    **input -[[2,4],[1,5],[1,9],[7,9]]; k=2**
    
    **output - 8**
    
    **write a program in javascript**