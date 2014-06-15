# Swan Admin

A simple administration interface for Express + Mongoose projects.

## Installing

    npm install swan-admin

## Usage

Swan Admin is ment to be used with **existing Express applications**. It assumes you have a Mongoose connection configured and working somewhere. To configure Swan Admin, you just have to give it some information about your Models.

### Basic Configuration

This should go on your Express app code.

    // Define which models will be available on the administration interface.
    // `User` and `Post` are instances of Mongoose.Model, in this example.
    var models = [
        { mongooseModel: User },
        { mongooseModel: Post }
    ];

    // Require the module and install the middleware.
    var admin = require('swan-admin');
    app.use('/admin', admin({
        models: models,
        credentials: {
            username: process.env.ADMIN_USERNAME,
            password: process.env.ADMIN_PASSWORD
        },
        sessionSecret: process.env.ADMIN_SESSION_SECRET
    }));

Swan Admin stores the authentication data on the **client**, securely. This means Swan Admin authentication is stateless, and can scale. It uses a Session Secret to encrypt the session data, before sending it to the browser. It is strongely adivised that you keep that in an environment variable, like `ADMIN_SESSION_SECRET` (as done above), whose value is a random string that only you know. The same applies to your credentials. If, for some reason, you cannot set these environment variable, you can pass the bare strings on the app configuration.

    app.use(adminMountPoint, admin({
        models: models,
        credentials: {
            username: 'boss',
            password: 'im-th3-b0ss'
        },
        sessionSecret: 'something-random-and-SECRET!'
    }));

### Advanced configuration

Each item on the `models` array can be set as follows:
    
    {
        mongooseModel: <Mongoose.Model>,  // the only MANDATORY item

        name: <String>,                   // human-readable name (defaults
                                          // to model name)

        pluralName: <String>,             // plural of `name` (defaults to
                                          // model name + 's')

        toString: <String>,               // model field to describe each
                                          // instance (defaults to 'id')
        // or
        toString: <Function>,             // takes a model instance and
                                          // returns a string

        fields: {<String>: <Object>},     // configure each model field
                                          // separately (defaults to {})

        explicitFieldsOnly: <Boolean>     // use only the fields specified in
                                          // `fields` above (defaults to false)
    }

The `fields` option is used to change a field behaviour. If you set `explicitFieldsOnly` to `true`, the `fields` also sets which model fields will be accessible on the admininstrator interface.

`fields` should be an object which model field names as keys and objects as values. The value object can be empty or have any of the following options:

    {
        editor: <String>    // sets the HTML widget for this field (defaults
                            // to 'textfield' or 'datetime')
                            // 
                            // possible values: 'textfield', 'textarea',
                            // 'markdown', 'datetime'
    }

## Known Issues

**This project is still in early stages and it is incomplete!** It hasn't been fully tested and it is not feature-complete. This project began as an administration interface for a personal project, so it is not as general as it should be yet.

I plan to enhance Swan Admin and make it more general and more efficient. If you wan't to contribute, feel free to talk to me or send a pull request. :)

### Some features not yet implemented

* Paginate instance list
* Filter/search for instances
* Enhance visual interface

## License (MIT)
Copyright © 2014 Alexandre Cisneiros - www.cisneiros.com

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.