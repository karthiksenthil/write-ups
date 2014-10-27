# Login Please
**Points:** 200
**Category:** Web
**Author:** [skarthik](https://github.com/karthiksenthil)
> Can you get the flag ?
> http://212.71.235.214:3000

> Hint: Favicon ?

## Write up
The site consists of a simple login form, and from the question it is evident that one needs to just bypass this login to get the flag. As a first step we can send a request by submitting the form with any values and check the Response headers to identify the engine being used. We find this in the Response headers :

X-Powered-By:Express

Express.js is a web development framework for node.js applications. 

Next we examine the HTML code of the login page, particularly the form. There we find that the parameters are being passed by the unsafe GET request to a 'login' action. Trying the usual SQLi techniques on this form seems to fail persistently. However we should remember that not all databases are SQL based ;)

Node.js apps are popularly paired with [MongoDB](http://www.mongodb.org/), a NoSQL database. A search for express.js and mongodb injection gives us the [techniques](http://blog.websecurify.com/2014/08/hacking-nodejs-and-mongodb.html) to inject in MongoDB+NodeJS apps. 

Solution : http://212.71.235.214:3000/login?username[$gt]=&password[$gt]=

> flag{M0ngoes_d0nt_grow_on_tr33s}

### Note

The hint was to direct people to Express . The given favicon was the default favicon used for express.js applications.

## External Write ups
