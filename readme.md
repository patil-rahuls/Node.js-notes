Express JS and Node JS in actions. What is Middleware.
Client ---> sends a request to our NodeJS http server ---> relays that Request to express JS ----> express generates appropriate response with the help of middleware -> that response is sent through thr node JS http server to the client.

Express Application Generator.

Setup project -> 
1. npm init - cmd used to scaffold a project. it shows a series of question that u can answer inorder to help setup your project.
Result - a package.json file is created.

2. nodemon - restarts the server whenever any changes occur in the code. ~hot reload
npm install express nodemon (we can add n number of packages separated by space in this command.)

3. package.json -> dependencies vs dev dependencies (--save-dev)
dev dependency -> required for development.
dependency -> required for your application.
"type": "module" -> so that we can use es6 modules. Previous versions used to have 'require'
// to use es6 with json modules we also need to use this in the scripts section of package.json
"scripts": {
    "start": "nodemon --experimental-json-modules index.js"
  },

4. HTTP request methods and Routes-
    Routes - How an app responds to a client's request to a particular endpoint. It is basically a routing path and an HTTP method.
        Routes are simply  - Paths & HTTP Methods
    HTTP Methods - actions taken on a specific resouce. CRUD. e.g. GET, POST, PUT, DELETE.
    GET - retrieves data from the server.
    POST - sends data to the server and creates a resouce.
    PUT - updates an existing resouce on the server.
    Delete - deletes an existing resource on the server.
    HTTP Messages - 
        Requests and
        Responses.

5. Routes -  Paths & HTTP Methods
    app.get(`/animals`, (req, res) => {
        res.json(data);
    });
    // Here the 1st argument of app.get() method 
        "/animals"
        is the path which the client has requested to with an HTTP method (app.get, app.post etc.)
    // 2nd arg, method - which will be return some data/functionality.

6. Routing Parameters -
    Parts/segments of url that are used to capture values specified at positions within the url. (query)
    app.get(`/class/:id/student/:stu_id`, (req, res) => {
        res.send(req.params);
    });
    req.params returns the key value pairs of the parameters in the url.
    
7. Route Handlers - 


Response Method - 
    .json
    .send
    .download - trqansfers a file as an attachment.
    .redirect

Route Chaning- 
    chaining several methods wiuth the same route to one single method.
    app.get('/class', callbkGET);
    app.post('/class', callbkPOST);
    app.put('/class', callbkPUT);
    // these can be chained using "app.route" method like this -
    app.route('/class').get(callbkGET).post(callbkPOST).put(callbkPUT);


###############################
Middleware-
    see the express js definition.
    Functions that have access to the request and response and the next() method in the application's req response lifecycle.
    Built in Middleware - Express JS
        1. express.static - serves static assets
        2. express.json - parses incoming req with JSON payload.
        3. express.urlencoded - parses incoming req with url encoded payload.


Security Best practices.
    keep updating the node version as well as all the packages and express.
    use TLS.
    use Helmet - protects your app. sets http headers appropriately and securely. collection of middleware functions that does that.
    use cookies securely.
    prevent brute force attack.
    rate-limiter-flexible. to rate limit
    ensure your dependencies are secure. npm audit.
