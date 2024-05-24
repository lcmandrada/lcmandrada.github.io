# NodeJS

Node.js is a JS runtime.

Use Node Version Manager (NVM) if needed.  
Virtual environment isn't necessary as package versions are automatically tracked for each project.

```bash
curl -fsSL https://deb.nodesource.com/setup_16.z | sudo -E bash -
sudo apt-get install -y nodejs
```



## NPM
Node Package Manager  
Packages are installed locally to the project's directory by default.

`package.json` - freezes project metadata including dependencies

```bash
# creates package.json interactively
npm init

# skip interaction
npm init -y

# install a package
npm install package
npm i package
npm i -g package      # global install
npm install           # install from `package.json`

# link a global package locally
npm link package
```



## Exports

`other.js`
```js
const PI = 3.1416

exports.PI = PI;

// or

exports = {
    PI: PI,
    ...
}
```

`app.js`
```js
const other = require('./other');
console.log(other.PI)

// or

const { PI } = require('./other');
```



## Entry Point

`dir/index.js`
```js
const x = require('./x');
const y = require('./y');
const z = require('./z');

exports = [x, y, z]
```



## Web App

### Code Structure
```
app.js                      // or index.js
package.json
models                      // database models
    entity.js
views                       // webpages
    home.ejs
    error.ejs
    entity                  // resource webpages
        page.ejs
    layouts                 // general page layouts
        boilerplate.ejs
    partials                // page components
        navbar.ejs
        alert.ejs
routes                      // route groups per resource
    resource.ejs
controllers                 // business logic
    entity.js
public                      // static assets
    javascripts
        script.js
    stylesheets
        main.css
    images
        favicon
utils
    AppError.js
    catchAsync.js
schemas.js                  // schema validators
middleware.js
seed.js                     // populate db for dev purposes
.env                        // environment variables
```



### Express
Minimalist web framework

```bash
npm i express

# automatically restarts server after changes
npm i -g nodemon

# start app
node app.js
// or better
nodemon app.js

# run script inside shell
node
.load app.js
```

`app.js`
```js
const express = require('express');
const path = require('path');
const { wrapAsync } = require('./utils');

const app = express()
const port = 3000



// template directory
// always relative to the root directory
app.set('views', path.join(__dirname, 'views'));

// serve static assets from a local directory, e.g. images, js, css
app.use(express.static(path.join(__dirname, 'public')))

// parse request body sent as form data
app.use(express.urlencoded({ extended: true }))
// parse request body as json
app.use(express.json())
// fine to use both simultaneously



// get route
// func is optional; used to execute a middlware before the route, e.g. authentication
app.get('/', func, (req, res) => { 
    ...
    res.send('Hello World!')
})

// post route
app.post('/', (req, res) => {
    // request body
    const { data } = req.body;
    ...
    res.redirect('/');
})



// path parameters and query strings
app.get('/r/:subreddit/:postId', validateResource, wrapAsync(async (req, res, next) => {
    // req.params is an object containing path parameters
    const { subreddit, postId } = req.params;

    // req.query is an object containing query strings
    const { q } = req.query;

    // contains request body
    const { resource } = req.body

    ...

    if (error) {
        // important to pass the error to `next` in async functions
        // return it to stop further code; only applicable without `wrapAsync`
        // return next(throw new AppError('error', 404));

        // or
        
        // only works if wrapAsync is applied
        throw new AppError('error', 404);
    }

    ...

    // render template
    // can also pass just { var } for { var: var }
    // no need to include .ejs in filename
    res.render(template, { key: value }) 
}))



// if none of the routes matched, this 404 middleware is executed
app.all('*', (req, res, next) => {
    next(throw new AppError('not found', 404))
})

// custom error handler as middlware
app.use((err, req, res, next) => {
    const { status = 500, message = 'error'} = err;

    ...

    res.status(status).render('error', { err });

    // or

    // still call express' default error handler
    // next(err);
})



// start app
app.listen(port, () => {
    console.log(`Listening on port ${port}`)
})
```

`utils/wrapAsync.js`
```js
// wrapper function to catch errors in async functions
module.exports.wrapAsync = function (fn) {
    return function(req, res, next) {
        // returning next(e) is important to stop further code executions
        fn(req, res, next).catch(e => next(e));
    }
}
```

`utils/AppError.js`
```js
// custom error class
class AppError extends Error {
    constructor(message, status) {
        super();
        this.message = message;
        this.status = status;
    }
}

module.exports = AppError
```

#### Router
Group or separate routes

`routes/entity.js`
```js
const express = require('express');
// merge params from the prefix
const router = express.Router({mergeParams: true});

// this middleware will only affect the routes specified in this router
// e.g. authenticated routes
router.use((req, res, next) => {
    ...
})

router.get(route, (req, res) => {
    ...
})

// grouping routess
router.route('/')
    .get(middleware, callable)
    .post(middleware, callable)
    .patch(middleware, callable)
    .delete(middleware, callable)

router.route('/:id')
    .get(middleware, callable)
    .post(middleware, callable)
    .patch(middleware, callable)
    .delete(middleware, callable)

...

module.exports = router;
```

`app.js`
```js
const resourceRoutes = require('./routes/resource');

app.use('/resource', resourceRoutes);
```

### Middleware

#### Express
Functions executed between the request and the response  
Runs code on every single request

`app.js`
```js
// custom middleware
// path is optional; when not supplied, middleware will always be executed
app.use(path, (req, res, next) => {
    // can access and modify req object, e.g. attach a datetime
    ...

    // call the next middleware
    next();

    //or

    // exit this function and stop executing any code after this point within this function
    // return next();
})
```

#### Method Override
Allows HTTP verbs when the client doesn't support them (e.g. `PATCH`, `DELETE`)

```bash
npm i method-override
```

`app.js`
```js
const methodOverride = require('method-override')

...

// allows overriding http methods for when the client doesn't support them, e.g. patch
// this particular override is triggered by query string, e.g. ?_method=PATCH
app.use(methodOverride('_method'))
```

#### Morgan
Logging middleware

```bash
npm i morgan
```

`app.js`
```js
const morgan = require('morgan');

...

app.use(morgan('tiny'))
```



### Embedded JavaScript (EJS)
Common and popular  
Uses JS syntax

Templating

- defines a pattern for a webpage  
- used for dynamic webpages

```shell
npm i ejs

// extend ejs
npm i ejs-mate
```

`app.js`
```js
const ejsMate = require('ejs-mate');

...

app.set('view engine', 'ejs');
app.engine('ejs', ejsMate);
```

`template.ejs`
```js
// include partials or subtemplates (e.g. header, nav, footer) from /views/partials or custom directory
<%- include('partials/head.ejs', { ...data }) %> 

// js code with print; key is passed from `render`
<%= key %>

// js code without print
<% if (condition) { %> 
    ...
<% } %>
```

`views/layouts/boilerplate.ejs`
```js
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <% include('../partials/navbar') %>
  <% body %>
</body>
</html>
```

`views/entity/page.ejs`
```js
<% layout('layouts/boilerplate') %>
...
```



### MongoDB
NoSQL  
Document database

#### Mongoose
**ODM** - object document mapper; map MongoDB documents to usable JS objects

```bash
# mongodb odm
npm i mongoose
```

`models/entity.js`
```js
const mongoose = require('mongoose');

// connect to mongodb
mongoose.connect('mongodb://localhost:27017/test', config)
    .then(() => {
        console.log('connection open');
    })
    .catch((e) => {
        console.log('error');
        console.log(e);
    })

// document schema
const resourceSchema = new mongoose.Schema({
    field: type,
    // built-in schema validations
    field1: {
        type: Number,
        required: true,
        min: 7,
        max: [9, 'error msg'],
        ...
    }
    ...
})



// custom instance methods
// common pattern to process model data
// use default function definition so `this` refers to the instance
resourceSchema.methods.method1 = function() {
    ...
}

// custom static methods
// when reference to instance is not needed, e.g. findByName
resourceSchema.statics.static1 = function() {
    ...
}



// virtuals
// instance properties that are not reflected to the database, e.g. fullName from/to firstName and lastName

// creating a virtual for generating a virtual property from existing properties
resourceSchema.virtual('fullName').get(function() {
    return `${this.first} ${this.last}`;
})

// setting existing properties from a virtual property
resourceSchema.virtual('fullName').set(function() {
    this.first = ...;
    this.last = ...;
})



// mongoose middlewares
// executes before the method
resourseSchema.pre('save', async function () {
    ...
})
// executes after the method
resourseSchema.post('save', async function () {
    ...
})



// will create a collection called 'resources'
// automatically pluralizes the model name
const Resource = mongoose.model('Resource', resourceSchema);

// creates an instance but not yet inserted to the collection
const data1 = new Resource({ ... });
// writes the instance to the collection
data1.save();



// database operations
Resource.insertMany([
    { ... },
    ...
])
    .then(data => {
        console.log(data);
    })

Resource.find({ query }).then(data => data);

// doesn't return updated data; only metadata
Resource.updateOne({ query }, { updates }).then(result => result);
Resource.updateMany({ query }, { updates }).then(result => result);

// returns updated data
Resource.findOneAndUpdate({ query }, { updates }, {new: true, runValidators: true}).then(data => data);

Resource.remove({ query }).then(result => result);
Resource.deleteOne({ query }).then(result => result);
Resource.deleteMany({ query }).then(result => result);
Resource.findOneAndDelete({ query }).then(data => data);
```

#### Relationships
1. **One to Few** (e.g. user to saved addresses) - embed the data directly in the document
    ```js
    const { Schema } = require('mongoose');

    const userSchema = new Schema({
        first: String,
        last: String,
        // NOTE: addresses are directly embedded in user's document
        addresses: [
            {
                // mongoose will automatically treat this as a document
                // set `false` to disable
                _id: { id: false },
                street: String,
                city: String,
                state: String,
                country: {
                    type: String,
                    required: true
                }
            }
        ]
    })

    const User = new mongoose.model('User', userSchema);

    const addAddress = async id => {
        const user = await User.findById(id);
        user.addresses.push({...});
        const res = await user.save();
        return res;
    }
    ```
2. **One to Many** (e.g. user to posts, farm to produce) - store data separately and reference the id
    ```js
    const { Schema } = require('mongoose');

    const productSchema = new Schema({
        name: String,
        price: Number,
        season: {
            type: String,
            enum: ['Spring', 'Summer', 'Fall', 'Winter']
        }
    })

    const farmSchema = new Schema({
        name: String,
        city: String,
        // NOTE: product ids (children) are referenced here
        products: [{ type: mongoose.Schema.Types.ObjectId, ref: 'Product' }] 
    })

    const Product = new mongoose.model('Product', productSchema);
    const Farm = new mongoose.model('Farm', farmSchema);

    const addProduct = async (id, productData) => {
        const product = new Product(productData);
        const farm = await Farm.findById(id);
        farm.products.push(product);
        const res = await farm.save();
        return res;
    }

    Farm.findById(id)
        .populate('products') // will replace the object ids with the actual object
        .then(farm => console.log(farm))
    ```
3. **One to Bajillions** (e.g. user to tweets) - embed parent reference inside the child document
    ```js
    const { Schema } = require('mongoose');

    const userSchema = new Schema({
        username: String
    })

    const tweetSchema = new Schema({
        text: String,
        likes: Number,
        // NOTE: user id (parent) is referenced here
        user: { type: Schema.Types.ObjectId, ref: 'User' }
    })

    const User = new mongoose.model('User', userSchema);
    const Tweet = new mongoose.model('Tweet', tweetSchema);

    const makeTweet = (id, tweetData) => {
        const user = await User.findById(id);
        const tweet = new Tweet(tweetData);
        tweet.user = user;
        const res = await tweet.save();
        return res;
    }

    Tweet.findById(id)
        .populate('user', 'username') // rest of the arguments specify which data from user to retrieve
        .then(tweet => console.log(tweet))

    Tweet.findById(id)
        .populate({ // for nested populating
            path: 'replies',
            populate: {
                path: 'authors'
            }
        })
        .then(tweet => console.log(tweet))
    ```

#### Middleware

**Delete linked documents**

Automatically delete linked documents upon deleting a document

```js
// `type` is the middlware type or action where the callback will be triggered and executed, e.g. findOneAndDelete
farmSchema.post(type, async function (data) {
    if (data.products.length) {
        await Product.deleteMany({_id: {$in: data.products}})
    }
})
```



### Joi (Schema Validation)
Schema description language  
Schema validator

```bash
npm i joi
```

`schemas.js`
```js
const Joi = require('joi');

// schema validator
const resourceJoiSchema = Joi.object({
    num: Joi.number().required().min(0).max(99),
    ...
})

const c = (req, res, next) => {
    // NOTE: schema validation
    const { error } = resourceJoiSchema.validate(resource)
    if (error) {
        const msg = error.details.map(e => e.message).join(',');
        next(throw new AppError(msg, 400));
    }
    next();
}

module.exports = {
    validateResource
}
```



### Cookies
Bits of information stored in user's browser  
Sent on every subsequent requests  
Allow stateful HTTP

```bash
npm i cookie-parser
```

`app.js`
```js
const express = require('express');
const app = express();
const cookieParser = require('cookie-parser');

app.use(cookieParser(secret));

app.get('/' (req, res) => {
    // setting signed cookies
    app.cookies(key, value, {signed: true});

    // getting cookies
    const { key = 'default' } = req.cookies;

    // getting signed cookies
    const { signedKey = 'default' } = req.signedCookies;
    ...
})
```

No need to reimplement HMAC as `cookie-parser` already uses this method.



### Session
Server side data store to allow stateful HTTP  

```bash
# uses memory store by default; not applicable for prod, only for dev
# use prod data store e.g. mongodb, redis
npm i express-session
```

`app.js`
```js
const express = require('express');
const app = express();
const session = require('express-session');

const sessionConfig = {
    secret: 'secret',
    resave: false,
    saveUninitialized: false,
    cookie: {
        httpOnly: true, // security
        expires: Date.now() + 1000 * 60 * 60 * 24 * 7, // week
        maxAge: 1000 * 60 * 60 * 24 * 7
    }
}
app.use(session(sessionConfig));

app.get('/', (req, res) => {
    // NOTE: access session data
    req.session.key = value;
    ...
})
```

#### Flash
Special area in the session  
Messages are written to flash and is cleared after being displayed

```bash
npm i connect-flash
```

`app.js`
```js
const flash = require('connect-flash');

app.use(session(...));
app.use(flash());

// middleware to save flash messages to locals so we don't have to always pass it to views
app.use((req, res, next) => {
    res.locals.messages = req.flash(key);
    next();
})

app.get('/' (req, res) => {
    // setting flash, usually followed by res.redirect()
    req.flash(key, value)

    // getting flash
    req.flash(key)
    ...
})
```



### Auth
**Authentication** - who are you  
**Authorization** - what can you access, after authentication

#### BCrypt
Password hashing function

```bash
npm i bcrypt
```

`app.js`
```js
const bcrypt = require('bcrypt');

const hashPassword = async (pw) => {
    // saltRounds slows hashhing for security; 12 is a sweet spot
    const salt = await bcrypt.genSalt(saltRounds);

    // salt is included in the generated hash
    const hash = await bcrypt.hash(pw, salt);
}

const hashPasswordMix = async (pw) => {
    // when we don't need to generate the salt manually
    const hash = await bcrypt.hash(pw, saltRounds);
}

const login = async (pw, hashedPw) => {
    const result = await bcrypt.compare(pw, hashedPw);
    ...
}
```

Authentication
```js
// middleware to check if user is logged in
const requireLogin = (req, res, next) => {
    if (!req.session._id) {
        return res.redirect('/login');
    }
    next();
}

// method to validate user
User.statics.findAndValidate = async function (username, password) {
    const user = await User.findOne({ username });
    const isValid = await bcrypt.compare(password, user.pass);
    return isValid ? user : false;
}

// middleware to hash the password before saving to database
User.pre('save', async function (next) {
    if (this.isModified('password')){
        this.password = await bcrypt.hash(this.password, 12)
    }
    next();
})

app.post('/register', async (req, res) => {
    const { username, password } = req.body;
    const user = new User({
        username,
        password: hash,
    })
    await user.save();
    req.session.user_id = user._id;
    res.redirect('/');
})

app.post('/login', async (req, res) => {
    const { username, password } = req.body;

    // const user = await User.findOne({ username });
    // const isValidPassword = bcrypt.compare(password, user.pass);
    // or
    const user = await User.findAndValidate(username, password);

    if (user) {
        req.session.user_id = user._id;
        res.redirect('/secret');
    } else {
        res.redirect('/login');
    }
})

app.get('/secret', (req, res) => {
    if (!req.session._id) {
        res.redirect('/login');
    }
    res.send('success');
})

app.get('/secret2', requireLogin, (req, res) => {
    res.send('success');
})

app.post('/logout', (req, res) => {
    req.session.destroy(); // delete all session data
    res.redirect('/login');
})
```

#### Passport
Easier Auth implementation

```bash
npm i passport passport-local passport-local-mongoose
```

`models/user.js`
```js
const mongoose = require('mongoose');
const Schema = mongoose.Schema;
const passportLocalMongoose = require('passport-local-mongoose');

const userSchema = new Schema({
    email: {
        type: String,
        required: true;
        unique: true
    }
});

// adds username, password and configurations
userSchema.plugin(passportLocalMongoose);

module.exports = mongoose.model('User', userSchema);
```

`app.js`
```js
const passport = require('passport');
const LocalStrategy = require('passport-local');
const User = require('./models/user')
const userRoutes = require('./routes/user')

app.use(passport.initialize())

// must be put after express session middleware
app.use(passport.sessions())
// authentication strategy
passport.use(new LocalStrategy(User.authenticate()));

// how data is stored in the session
passport.serializeUser(User.serializeUser());
// how to get the data out from the session
passport.deserializeUser(User.deserializeUser())

// middleware to provide values to view pages
app.use((req, res, next) => {
    req.locals.currentUser = req.user;
    ...
})

app.use('/', userRoutes);
```

`middleware.js`
```js
module.exports.isLoggedIn = (req, res, next) => {
    if (!req.isAuthenticated) {
        req.flash('error', 'not authenticated');
        return res.redirect('/login');
    }
    next();
}
```

`routes/user.js`
```js
const express = require('express');
const router = express.Router();
const User = require('../models/user');
const catchAsync = require('../utils/catchAsync');
const passport = require('passport');
const { isLoggedIn } = require('../middleware')

router.get('/register', (req, res) => {
    res.render('users/register')
})

router.post('/register', catchAsync((req, res) => {
    try {
        const (username, email, password) = req.body;
        const user = new User({username, email});
        const newUser = await user.Register(user, password);

        // log in user after registration
        req.login(newUser, err => {
            if (err) return next(err);
            req.flash('success', 'Welcome!');
            res.redirect('/');
        })
    } catch (E) {
        req.flash('error', e.message);
        res.redirect('/register')
    }
    
}))

router.get('/login', (req, res) => {
    res.render('users/login')
})

router.post('/login', passport.authenticate('local', { failureFlash: true, failureRedirect: '/login' }), (req, res) => {
    req.flash('success', 'Welcome back!');
    const returnTo = req.session.returnTo || '/';
    delete req.session.returnTo;
    res.redirect(returnTo);
})

router.get('/secret', (req, res) => {
    // consider putting this logic to `isLoggedIn` middleware
    if (!req.isAuthenticated) {
        // save the original url before redirecting a non authenticated user to login so we can return the user to it
        req.session.returnTo = req.originalUrl;
        req.flash('error', 'not authenticated');
        return res.redirect('/login');
    }
    ... 
})

router.get('/secret2', isLoggedIn, (req, res) => {
    ... 
})

router.get('/logout', (req, res) => {
    req.logout();
    req.flash('success', 'goodbye');
    res.redirect('/');
})

module.exports = router;
```

Authorization
```js
const isAuthor = (req, res, next) => {
    const { id } = req.params;
    const element = await Resource.findById(id);
    if (!element.author.equals(req.user._id)) {
        req.flash('error', 'no access');
        return res.redirect('/');
    }
    next();
}

app.post('/:id', (req, res) => {
    const { id } = req.params;
    const element = await Resource.findById(id);
    if (!element.author.equals(req.user._id)) {
        req.flash('error', 'no access');
        return res.redirect('/');
    }
    ...
})

app.post('/v2/:id', isAuthor, (req, res) => {
    ...
})
```



### .env
Environment variables

```bash
npm - dotenv
```

`.env`
```
KEY=value
```

`app.js`
```js
if (process.env.NODE_ENV !== 'production') {
    // use .env in dev environment
    require('dotenv').config();
}

const variable = process.env.KEY || 'default';
```



### File Upload
1. Set `enctype=multipart/form-data` to the form
2. Create `input` of `type="file"` element
3. Add encoder middleware in back-end
4. Consider constraints (e.g. max number of images, image size and resolution)

```bash
npm i multer
npm i cloudinary multer-storage-cloudinary
```

`.env`
```
CLOUDINARY_CLOUD_NAME=x
CLOUDINARY_KEY=x
CLOUDINARY_SECRET=x
```

`cloudinary/index.js`
```js
const cloudinary = require('cloudinary').v2;
const { CloudinaryStorage } = require('multer-storage-cloudinary');

cloudinary.config({
    cloud_name: process.env.CLOUDINARY_CLOUD_NAME,
    api_key: process.env.CLOUDINARY_KEY,
    api_secret: CLOUDINARY_SECRET
})

const storage = new CloudinaryStorage({
    cloudinary,
    params: {
        folder: 'test',
        allowedFormats: ['jpeg', 'jpg', 'png']
    }
})

module.exports = {
    cloudinary,
    storage
}
```

`route.js`
```js
const multer = require('multer');
const { storage } = require('./cloudinary');
const upload = multer({ storage });

// route to automatically upload images
router.post('/', upload.single('image'), (req, res) => {
    // added by upload middleware
    const image = req.file;

    // for upload.multiple(name)
    // const images = req.files.map(f => ({ path: f.path, filename: f.filename }))
    ...
})

// route to delete images
router.put('/', upload.multiple('images'), (req, res) => {
    ...
    // images is an array of filename (not url)
    for (let img of images) {
        await cloudinary.uploader.destroy(img);
    }
    ...
})
```



### Geocoding

```bash
npm i @mapbox/mapbox-sdk
```

`.env`
```
MAPBOX_TOKEN=x
```

`models/entity.js`
```js
// mongodb supports geocoding, provides operations
const geoSchema = new Schema({
    ...
    geometry: {
        type: {
            type: String,
            enum: ['Point'],,
            required: true
        },
        coordinates: {
            type: [Number],
            required: true
        }
    }
    ...
})
```

`controller/entity.js`
```js
const mbxGeocoding = require('@mapbox/mapbox-sdk/services/geocoding');
const mapBoxToken = process.env.MAPBOX_TOKEN;
const geocoder = mbxGeocoding({ accessToken: mapBoxToken });

const map = async (req, res) => {
    ...
    // get geometry data from a place (string) e.g. Manila, Philippines 
    const geoData = await geocoder.forwardGeocode({
        query: place,
        limit: 1
    }).send()

    // returns a geojson; contains coordinates
    geo.geometry = geoData.body.features[0].geometry;
    geo.save();
    ...
}
```



### Common Security Issues

#### SQL/NoSQL Injection
Injecting database operations via input elements

```js
db.users.find({username: req.body.username})
// injection: {'gt': ''}

// will return all users
db.users.find({username: {'gt': ''}})
```

Use a database sanitation library (e.g. Express Mongoose Sanitize)
```
npm i expess-mongo-sanitize

// app.js
const mongoSanitize = require('express-mongo-sanitize');
app.use(mongoSanitize());
```

#### Cross Site Scripting (XSS)
Injecting client side scripts on a web app (e.g. to steal data)  

`<script>...</script>` on user input  
Adding a script to url that sends cookies to another server

Use an HTML sanitation library (e.g. create custom JOI extension using `sanitize-html` library)  
Set session cookies as `httpOnly` and secure (HTTPS)  
Set custom session cookie name instead of the default

#### Hide errors and traceback

#### Add security headers  
`helmet` - Express security with headers  
Content security policy might break the app.  
It controls the locations from which certain resource types may be loaded. Configure appropriately.

```
npm i helmet

// app.js
const helmet = require('helmet');
app.use(helmet());

app.use(helmet.contentSecurityPolicy({
    directives: {
        // set sources per resource type
        ...
    }
}))
```



### MongoDB Atlas
MongoDB as a service  
Has a free plan, no credit card required

1. Create cluster
2. Create database user
3. Set network access
4. Connect to cluster

#### As session store
```bash
npm i connect-mongo
```

`app.js`
```js
const session = require('express-session');
const MongoDBStore = require('connect-mongo')(session);

const store = new MongoDBStore({
    url: ...,
    secret: 'x',
    // update session one time during a 24 hour period (except for changes in session data); in seconds
    touchAfter: 24 * 3600
})

store.on('error' function(e){
    console.log('session store error', e);
})

const sessionConfig = {
    store,
    ...
}

app.use(session(sessionConfig));
```



### Heroku
Platform as a service  
Has a free plan, no credit card required

1. Install heroku cli
2. `heroku create`
3. Prepare env variables (e.g. database url, secrets)
4. Create start script and configure env port
5. Configure env variables either on Heroku website settings or Heroku cli
6. Whitelist Heroku ip in mongodb atlas
7. Push app code to Heroku

```bash
heroku login
heroku create // make space for the app; do in app's root directory
heroku logs -tail // troubleshoot heroku app

heroku config:set SECRET=VALUE
heroku restart
```

`package.json`
```
{
    ...
    "scripts": {
        "start": "node app.js" // app entry point
    }
    ...
}
```

`app.js`
```js
// PORT is provided by heroku
const port = process.env.PORT || 3000;
app.list(port, () => {
    ...
})
```

```bash
# push code to heroku
git add .
git commit -m "deploy"
git push heroku master
```
