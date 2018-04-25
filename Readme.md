# SQL Express API Seed (SQLXApi)

It's pretty handy to access existing databases via API, especially when building UI or other interfaces. Thie project provides a base for a simple REST API to SQL Server using Node.js, Express (a Web framework for Node.js) and mssql (MS SQL Server client for Node.js). 

The intent is specifically to allow fast deploy of basic CRUD functions for testing and dev so we ignore authentication and permissioning (for now). Results are returned in JSON format .

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes. See deployment for notes on how to deploy the project on a live system.

### Prerequisites

#### node.js
I'm assuming you already have node.js installed and accessible. If not, you can download it [here](https://nodejs.org/en/). 
For installation instructions, click [here](https://nodejs.org/en/download/package-manager/#windows)

#### Express 

Express is one of (if not) the most popular Node web framework. It is designed for building web applications and APIs and has been called the de facto standard server framework for Node.js[2] and provides mechanisms to:

* Write handlers for requests with different HTTP verbs at different URL paths (routes).
* Integrate with "view" rendering engines in order to generate responses by inserting data into templates.
* Set common web application settings like the port to use for connecting, and the location of templates that are used for rendering the response.
* Add additional request processing "middleware" at any point within the request handling pipeline.

While Express itself is fairly minimalist, developers have created compatible middleware packages to address almost any web development problem. There are libraries to work with cookies, sessions, user logins, URL parameters, POST data, security headers, and many more. You can find a list of middleware packages maintained by the Express team at Express Middleware (along with a list of some popular 3rd party packages).

we will be installing the express generator so no requirement to have this pre-installed. 


### Installing

We will be using the application generator tool, express-generator, to quickly create an application skeleton.

The express-generator package installs the express command-line tool. Use the following command to do so:

```
$ npm install express-generator -g
```

the following script then creates an Express app named SQLXApi. The app will be created in a folder named SQLXApi in the current working directory and the view engine will be set to Pug:


```
$ express --view=pug SQLXApi
```

Then install dependencies:

```
$ cd SQLXApi
$ npm install
```

Finally install [mssql](https://www.npmjs.com/package/mssql) (MS SQL Server client for Node.js):

**Note** the latest version requires Node.js 4 or newer.

```
npm install mssql --save
```


## Running the API

On MacOS or Linux, run the app with this command:

```
$ DEBUG=express:* npm start
```

On Windows, use this command (often fails in powershell):

```
> set DEBUG=express:* & npm start
```

This often fails in powershell. If this is the case you can run in two separate commands as follows.

```
> set DEBUG=express:*
> npm start
```

Then load http://localhost:3000/ in your browser to access the app.

if you'd like to run another port, you can do this by setting the environment variable first like so

```
$ PORT=8080
```

Or in Windwos

```
> set PORT=8080
```

## Common API  CRUD Calls

### Load Full Record Set (GetAll)

let's create a call that returns all the orders via a GET call to http://localhost:8081/orders/

```javascsript
app.get('/order', function (req, res) {
    sql.connect(sqlConfig, function() {
        var request = new sql.Request();
        request.query('select * from Orders', function(err, recordset) {
            if(err) console.log(err);
            res.end(JSON.stringify(recordset)); // Result in JSON format
        });
    });
})
```

### Load Specific Record (GetOne)

let's create a call that returns a simple Select query with a where clause on the OrderID field via a GET call to http://localhost:8081/orders/10

```javascsript
app.get('/orders/:orderID/', function (req, res) {
    sql.connect(sqlConfig, function() {
        var request = new sql.Request();
        var stringRequest = 'select * from Orders where orderID = ' + req.params.orderID;
        request.query(stringRequest, function(err, recordset) {
            if(err) console.log(err);
            res.end(JSON.stringify(recordset)); // Result in JSON format
        });
    });
})
```


## Deployment

in live.. not yet

## Built With

* [nodejs](https://nodejs.org/en/) - The web framework used
* [express](https://expressjs.com/) - minimal and flexible Node.js web application framework
* [mssql](https://www.npmjs.com/package/mssql) MS SQL Server client for Node.js:

## Versioning

Current version is 1.0.0-beta
[SemVer](http://semver.org/) is used for versioning. For the versions available, see the [tags on this repository](https://github.com/your/project/tags). 

## Authors

* **Nathan Sasto** - *Initial work* (https://github.com/nsasto)


## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details


## References

[2]. [Case study: How & why to build a consumer app with Node.js](https://venturebeat.com/2012/01/07/building-consumer-apps-with-node/). VentureBeat.com.