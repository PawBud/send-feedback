# reporters
The reporter of the send-feedback element is the way the actual feedback is sent.
It is only called if the input is valid, after validating all the fields.

## Default reporters

#### `emailReporter`

This reporter will open up the default email app of the operating system, and have the email ready to be sent to address
you will specify.

Use [`emailReporter.useReporter`](methods.md#useReporter) to register a reporter.
For data property this repoter just needs the email to be specifed:

```javascript
sendFeedback.useReporter('emailReporter', {
  email: '<your-support-email-address>'
});
```

#### `browserReporter`

This will open up the url specified in the browser, it will be like a `GET` request.
You can specify the query parameter.
```javascript
sendFeedback.useReporter('browserReporter', {
  url: 'https://example.com',
  titleParam: 'title', // optional, default is title
  bodyParam: 'body' //  optional, default is body
});
```

### `githubReporter`

This will open up the browser to issue tracker. The title, body will be set and
it will be fromatted with markdown.
```javascript
sendFeedback.useReporter('githubReporter', {
  url: 'https://github.com/<username>/<repo>/issues/new'
});
```

### `postRequestReporter`

This reporter can send a post request in background to a server.
The `send-feedback` element will show a reporter a loader until the
feedback is successfully sent.
```
sendFeedback.useReporter('postRequestReporter', {
  url: 'url of your server',
  titleParam: 'string' // optional the title parameter default is title
  bodyParam: 'string' // optional the body parameter default is body
});
```

The loader shown, can be customized:
```javascript
// default is &#10004; Feedback sent.
// &#10060; -> ✔
sendFeedback.loaderSucessText = 'your desired text';

// default is &#10060; Error sending feedback! try again..
// &#10060; -> ❌
sendFeedback.loaderErrorText = 'your desired text';
```

You can take a look this [setting up backend tutorial](/tutorials/setting-up-backend.md)
for setting up a backend that accpets feedback sent from the app.

# Custom reporter
You can pass a function to be used as a reporter. The function will be called with
an object and data if you passed data when calling `useReporter`.
```javascript
{
  title, // title the user entered
  body,  // body user entered
  logs // pre read logs as filename and file content
}
```

If you custom reporter need to use the built in loader you can show the loader using
`this.showLoader()` in the custom function, and to stop the loader `this.hideLoader()`
if you want to show error you need to pass `true` as a argument indicating an error had
occured `this.hideLoader(true)`.

Note: the filename if the path passed into the `sendFeedback.logs` array.
see [documentation of logs for more info.](methods.md#logs)
