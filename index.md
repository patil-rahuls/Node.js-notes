import express from "express";
import data from "./data/data.json" assert { type: "json" };
const app = express(); // instantiate express.
const PORT = 9999;

app.listen(PORT, "0.0.0.0", () => {
    console.log(`Listening on port# ${PORT}`);
});

// Routes
// *app.get()*
// syntaxt - app.get(<route>, <handler for this route-- which is basically a callback fn having 2 parameters- req and res.>)
app.get(`/`, (req, res) => {
    // querying db and fetching data.
    res.json(data);
});

app.post(`/create`, (req, res) => {
    res.send("this is a post req.");
});
app.put(`/edit`, (req, res) => {
    res.send("this is a put req.");
});
app.delete(`/del`, (req, res) => {
    res.send("this is a delete req.");
});

// How to serve static files.
// *app.use()*
// e.g. using the public folder at the root of the project.
app.use(express.static('public')); // no '/' here
// ########IMP########## express.static is a built-in middleware function.
// this means, you can now access anything inside 'public' folder directly from the "root" url. i.e. localhost:9969/<that_file>
// i.e. localhost:9969/a.txt
// usefull for static files css images etc.

// using image at the route '/img'. observe that '/img' doesn't actually existson our server. We just created a prefix.
// localhost:9969/images/...
app.use('/img', express.static('images'));
// this will take all the contents of 'images' folder and route it to "localhost:9969/img" url.

// Routing Parameters
// localhost:9969/class/2
app.get(`/class/:id`, (req, res) => {
    // console.log(req.params); // req.params hold the routing parameters. i.e. id in our case.
    // all routes parameters are passed as strings.
    const stuId = Number(req.params.id);
    const stu = data.filter(stu => stu.id === stuId);
    res.json(stu);
    console.log(stu);
});

// Route Handlers-
// Additional callbacks to the routes
// get with next
// next is a callback, we can name it anything, but just to avoid confusions, keep is named as 'next'.
app.get(`/xxx`, (req, res, next) => {
    console.log(`response will be sent by the next function.`);
    next(); // called next.
}, // remember nto to close the app.get here. it is just a comma.
// definition for next()
(req, res)=> {
    res.send('I just set up a route with a 2nd callback.');
});

// Common Response Methods.
// .json() -> sends a JSON response.
// .send() -> sends a HTTP response.
// .download() -> transfers a file as an attachment.
// .redirect
app.get(`/redir`, (req, res) => {
    res.redirect('https://google.com');// any url
});
// these methods are applied on the 'response' object i.e. response parameter of the callback of app.get().

// Route Chaining.
// app.route()
// allows us to chain all methods together for a particular route. in this case - "/class"
app.route("/test")
.get((req, res) => {
    // querying db and fetching data.
    res.json(data);
})
.post((req, res) => {
    res.send("this is a post req.");
})
.put((req, res) => {
    res.send("this is a put req.");
})
.delete((req, res) => {
    res.send("this is a delete req.");
});

// ######################
// Middleware
//  functions having access to res, req and next().
// using express js built in middleware
app.use(express.json());

// Built-in middleware functions of express
// express.static
// express.json
// express.urlencoded

// Handling errors: Utilizing the middleware for errors.
// usefull for manually throwing error.
app.route("/err")
.get((req, res) => {
    // querying db and fetching data.
    throw new Error();
});
// this shows an error not usesull and logical for user. 

// Custom error handler functions.
// Should be declared beofre app.listen();
app.use((err, req, res, next) => {
    console.error(err.stack);
    res.status(500).send('something went wrong.');
});
// Now when you hit localhost:9999/err -> this shows this string 'something went wrong' to the end user. and error stack trace to the server logs.

// Third Party middleware for Express.
// express documentation.
// body-parser
// npm install body-parser
// import body-parser from "body-parser"



