# Swan Admin

A simple administration interface for Express + Mongoose projects.

## Installing

    npm install swan-admin

## Usage

Swan Admin is ment to be used with **existing Express applications**. It assumes you have a Mongoose connection configured and working somewhere. To configure Swan Admin, you just have to give it some information about your Models.

### Basic Configuration

This should go on your Express app code.

    // User and Post are instances of Mongoose.Model
    var models = [
        { mongooseModel: User },
        { mongooseModel: Post }
    ];

    // This sets the root of your admin interface
    var adminMountPoint = '/admin';

    // Credentials to access the admin system
    // Try to use environment variables instad of putting them on the code
    var credentials = {
        username: process.env.ADMIN_USERNAME,
        password: process.env.ADMIN_PASSWORD
    };
    
    // Require the module and install the middleware
    // If you set the variables above, you don't have to change anything below.
    var admin = require('swan-admin');
    app.use(adminMountPoint, admin({
        models: models,
        mountPoint: adminMountPoint,
        credentials: credentials
    }));

Also, Swan Admin stores the authentication data on the **client**, securely. This means Swan Admin authentication is stateless, and can scale. It assumes you have a `SESSION_SECRET` environment variable, whose value is a random string that only you know.

If, for some reason, you cannot set this environment variable, you can pass the string on the app configuration. In that case, the last part would be something like:

    var admin = require('swan-admin');
    app.use(adminMountPoint, admin({
        models: models,
        mountPoint: adminMountPoint,
        credentials: credentials,
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

**This project is still in early stages!** It hasn't been fully tested and it is not feature-complete. This project began as an administration interface for a personal project, so it is not as general as it should be yet.

I plan to enhance Swan Admin and make it more general and more efficient. If you wan't to contribute, feel free to talk to me or send a pull request. :)

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