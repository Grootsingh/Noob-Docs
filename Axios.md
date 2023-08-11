### from the docs:

# why use Axios over fetch?

- Axios is capabile of Intercepting both request and response.
- The Fetch API doesn’t have an onprogress handler for showing progress bar
  but Axios does becouse it uses XMLHTTPRequest.

- Axios converts data to json format (by default)(also possible in fetch)
- Better Error handling including 404 and 500 error.(also possible in fetch)
- Axios provides built-in support for request cancellation and request timeout.(also possible in fetch)

# how to install

install code:

```
$ npm install axios
```

after installation import:

```
import axios from 'axios';
```

## Get request fetch vs Axios

```
// fetch()

fetch('https://api.github.com/orgs/axios')
  .then(response => response.json())    // one extra step
  .then(data => {
    console.log(data)
  })
  .catch(error => console.error(error));
---------------------------------------------------

// axios
axios.get('https://api.github.com/orgs/axios')
  .then(response => {
    console.log(response.data);
  }, error => {
    console.log(error);
  });
```

- the only beneft with Axios get request is that by default you get JSON
  formatted data. thats it and it is not something that can't be done in
  fetch.

- one other beneft with Axios that it can handle all type of error so we
  don't need try and catch with Axios.

## Post request fetch vs Axios

```
// fetch()

const url = "https://jsonplaceholder.typicode.com/todos";
const options = {
  method: "POST",
  headers: {
    Accept: "application/json",
    "Content-Type": "application/json;charset=UTF-8",
  },
  body: JSON.stringify({
    a: 10,
    b: 20,
  }),
};

fetch(url, options)
  .then((response) => response.json())
  .then((data) => {console.log(data)});

----------------------------------------------------------------------
// axios

const url = 'https://jsonplaceholder.typicode.com/posts'

const data = {
  a: 10,
  b: 20,
};

const config = {
    headers: {
      Accept: "application/json",
      "Content-Type": "application/json;charset=UTF-8",
    },
}

axios.post(url, data, config).then(({data}) => {console.log(data)});
```

- To send data, fetch() uses the body property for a post request to send
  data to the endpoint, while Axios uses the data property

- The data in fetch() is needed to be transformed to a string using the
  JSON.stringify method before posting request to the endpoint while Axios
  transform the data to JSON format automatically.

- Axios automatically transforms the data returned from the server, but with
  fetch() you have to call the response.json method to parse the data to a
  JavaScript object.

- With Axios, the data response provided by the server can be accessed with
  in the data object, while for the fetch() method, the final data needed to
  be name (response) then we need to format it with JSON and then we will
  recive that data in next promise.

# how to make request with Axios (the Hard way)

- Requests can be made by passing the relevant config to axios.

you can make a request by passing the config object with relevent method on that which define which type of request you are making:

```
for Sending a post request:
axios({
  method: 'post',
  url: '/user/12345',
  data: {
    firstName: 'Fred',
    lastName: 'Flintstone'
  }
---------------------------------
for get request:
axios({
  method: 'get',
  url: 'https://bit.ly/2mTM3nY',
  responseType: 'stream'
}).then((response) => {
    response.data.pipe(fs.createWriteStream('ada_lovelace.jpg'))
  });
});
```

this is how you can make request with Axios but it's not recommended
becouse Axios also provide Shorthand for all type of request you can
Imangine.

# Axios Shorthand for request

- axios.request(config)
- axios.get(url, config)
- axios.delete(url, config)
- axios.head(url, config)
- axios.options(url, config)
- axios.post(url, data, config)
- axios.put(url, data, config)
- axios.patch(url, data, config)

Note: if you don't specify any method axios will take get method
as default. so axios(url) = axios.get(url)

Note: When using the Axios Shorthand url, method, and data properties
don't need to be specified in config.

this is becouse axios.method(url,data,config) all are present in the
shorthand to begin with.

## sending multiple request with Axios

use promise.all() to make multiple request

## The Axios Instance

create a custom instance of Axios

```
instance foramt: axios.create(config)

ex: const instance = axios.create({
        baseURL: 'https://some-domain.com/api/',
        timeout: 1000,
        headers: {'X-Custom-Header': 'foobar'}
});
```

here we have created a instance which contain config for the request. it is used to
make a general instance so we don't have to repeat our config code again and agian
we can use this istance to make different different reqest.

```
ex1: const userData = instance.get("/user")
with this get request we are using url = baseURL + url and some additional config setting
are also set like  timeout: 1000 and headers: {'X-Custom-Header': 'foobar'} with this request.
```

we can use all the AxiosShorthand with Axios Instances.

### Request Config object

what keys can config object can take:

1.  url: '/user'

- is the server URL that will be used for the request.

2.  method: 'get', // default

- request method type

3.  baseURL: 'https://some-domain.com/api'

- `baseURL` will be prepended to `url` unless `url` is absolute.
  It can be convenient to set `baseURL` for an instance of axios to pass
  relative URLs to methods of that instance.

4.  transformRequest: [function (data, headers) {
    // Do whatever you want to transform the data

    return data;
    }]

- the transformRequest option allows you to define a function that will be
  applied to the request data before it is sent to the server. This function
  can be used to transform the data in a specific format or perform any
  necessary preprocessing.

Note: only applicable for request methods 'PUT', 'POST', 'PATCH' and 'DELETE'

5. transformResponse: [function (data) {
   // Do whatever you want to transform the data

   return data;
   }]

- the transformResponse option allows you to define a function that will be
  applied to the response data before it is passed to then or catch handlers.
  This function can be used to transform the response data in a specific format
  or perform any necessary post-processing.

6. headers: {'X-Requested-With': 'XMLHttpRequest'}

- Custom header you want to send to the server.

7.  params: {ID: 12345}

- params are parameter object which store key:value pair which are called
  query parameters which are added after the URL.
- "params" typically refers to the parameters that are included in the URL
  of a request. These parameters are used to provide additional information
  or data to the server.

- Parameters are specified as key-value pairs and are appended to the URL
  as query parameters. They are separated from the base URL by a question mark (?),
  and multiple parameters are separated by an ampersand (&).

ex: https://api.example.com/endpoint?param1=value1&param2=value2

NOTE: params that are null or undefined are not rendered in the URL.

```
axios.get('https://api.example.com/endpoint', {
  params: {
    param1: 'value1',
    param2: 'value2'
  }
})
  .then(response => {
    // Handle the response
  })
  .catch(error => {
    // Handle any errors
  });
```

8.  paramsSerializer: function (params) {
    return Qs.stringify(params, {arrayFormat: 'brackets'})
    }

- When you make a GET request with Axios, you can pass query parameters
  using the params option. The params option takes an object with key-value
  pairs representing the query parameters. By default, Axios serializes
  these parameters using the qs library, which uses the traditional URL
  encoding format.

- However, in certain cases, you may need to customize the serialization
  of the parameters, especially when dealing with nested objects or arrays.
  This is where the paramsSerializer option comes in.

- The paramsSerializer option allows you to define a custom function that
  will be used to serialize the request parameters into a STRING. This string
  will then be appended to the URL.

# side Knowlage URL encoding format

# before URL encoding format understand URL

- A URL is another word for a web address.

- A URL can be composed of words (e.g. w3schools.com), or an Internet Protocol
  (IP) address (e.g. 192.68.20.50).

- Most people enter the name when surfing, because names are easier to remember
  than numbers.

# URL encoding format

- URLs can only be sent over the Internet using the ASCII character-set.
  If a URL contains characters outside the ASCII set, the URL has to be converted.

- URL encoding converts non-ASCII characters into a format that can be
  transmitted over the Internet.

- URLs cannot contain spaces. URL encoding normally replaces a space with a
  plus (+) sign, or %20.

9. data: {firstName: 'Fred'}

- `data` is the data to be sent as the request body
- Only applicable for request methods 'PUT', 'POST', 'DELETE', and 'PATCH'
- depending on the data type Axios will automatically write the appropiate
  context-type;
- data type = plain text (string) ("text/plain"), JSON (object)("application/json"),
  URL-encoded form data ({param1: 'value1',param2: 'value2'})
  ("application/x-www-form-urlencoded"),FormData(new FormData())("multipart/form-data")
- all the data will be automaticlly serialized before sending to the server.

10. data: 'Country=Brasil&City=Belo Horizonte'

- alternative syntax for sending data in the body of a POST request using Axios.
- The data property is assigned a string value that represents the data to be
  sent in the request body. The string is formatted as a series of key-value pairs
  separated by ampersands (&).

- In this specific example, the data being sent is 'Country=Brasil&City=Belo Horizonte'.
  This data suggests that the request is intended to send information about a country and
  a city, with the values "Brasil" and "Belo Horizonte", respectively.

- However, it is important to note that this syntax assumes the data is already
  URL-encoded. In other words, the values should be properly encoded if they contain
  reserved characters or spaces. If the values are not URL-encoded, you should perform
  the encoding manually before passing them in this format.

- While this syntax can be used to send data in the body of a POST request, it is
  generally more common and recommended to use an object to represent the request data,
  as it allows for easier manipulation and automatic serialization. For example, you can
  use an object like this:

```
data: {
  Country: 'Brasil',
  City: 'Belo Horizonte'
}
```

- Using an object for the data allows Axios to automatically serialize
  the data, handle URL encoding, and set the appropriate content type header
  based on the request payload.

11. timeout: 1000, // default is `0` (no timeout)

- the timeout option allows you to specify the maximum amount of time
  (in milliseconds) that a request can take before it times out. If the
  request exceeds the specified timeout duration, Axios will abort the
  request and reject the Promise with an error.

```
axios.get('https://api.example.com/data', {
  timeout: 5000 // Timeout set to 5 seconds (5000 milliseconds)
})
  .then(response => {
    // Handle the response
  })
  .catch(error => {
    if (error.code === 'ECONNABORTED') {
      console.log('Request timed out');
    } else {
      console.log('An error occurred', error);
    }
  });
```

- In this example, we make a GET request to https://api.example.com/data
  and set the timeout option to 5000 milliseconds (5 seconds). If the request
  takes longer than 5 seconds to complete, Axios will abort the request and
  trigger a timeout error.

- In the error handling code, we check for the ECONNABORTED error code, which
  indicates that the request has timed out. If a timeout occurs, we can handle
  it separately from other types of errors.

- Setting a reasonable timeout for requests can be useful to prevent indefinitely
  waiting for a response and to handle scenarios where the server may not respond
  in a timely manner.

- It's worth noting that the timeout option can be applied to both GET and POST
  requests, as well as other types of requests made with Axios.

12. withCredentials: false, // default

- The withCredentials option in Axios allows you to control whether
  cross-site Access-Control requests should be made with credentials,
  such as cookies, authorization headers, or TLS client certificates.

- When making cross-origin requests, browsers enforce the Same-Origin
  Policy, which restricts the sharing of cookies and other sensitive
  information between different origins. However, there are scenarios
  where you may need to include credentials in cross-origin requests,
  such as when working with APIs that require authentication.

- By default, the withCredentials option is set to false in Axios,
  which means that cross-origin requests will not include credentials.
  If you want to include credentials, you can set the withCredentials
  option to true.

Here's an example of how to use the withCredentials option in Axios:

```
axios.get('https://api.example.com/data', {
  withCredentials: true
})
  .then(response => {
    // Handle the response
  })
  .catch(error => {
    // Handle any errors
  });
```

- In this example, we make a GET request to https://api.example.com/data
  and set the withCredentials option to true. This indicates to the browser
  that the request should include credentials.

- When withCredentials is set to true, the browser will include cookies
  and other relevant credentials in the request headers. This allows the
  server to authenticate the request and respond accordingly.

- Please note that in order for the server to accept credentials from a
  different origin, it must be configured to allow cross-origin requests
  with credentials by including the appropriate response headers, such as
  Access-Control-Allow-Origin and Access-Control-Allow-Credentials.

- Additionally, be cautious when enabling withCredentials, as it may
  introduce security considerations. Only use it when necessary and ensure
  that your server-side implementation is properly handling and validating
  incoming requests with credentials.

13. function customAdapter(config) {
    // Implement your custom request handling logic here
    // Return a Promise that resolves with the response data
    }

- customAdapter allow custom-Handle the request, you can return a fake return
  value which helps during testing the external API.

14. auth: {
    username: 'janedoe',
    password: 's00pers3cret'
    }

- The auth option in Axios allows you to provide authentication credentials
  for making requests that require authorization. It provides a convenient way
  to include authentication information, such as username and password or an
  access token, in the request headers.

- Note: auth option in Axios, it will overwrite any existing Authorization
  header that may have been set manually or by default.

There are two ways to use the auth option in Axios:

14. 1. Basic Authentication:

If you need to use Basic Authentication, where the username and
password are sent as Base64-encoded strings in the Authorization
header, you can pass an object with username and password
properties to the auth option.

```
axios.get('https://api.example.com/data', {
  auth: {
    username: 'myusername',
    password: 'mypassword'
  }
})
  .then(response => {
    // Handle the response
  })
  .catch(error => {
    // Handle any errors
  });
```

In this example, Axios will automatically encode the username and
password provided and set the Authorization header in the request.

14. 2. Bearer Token Authentication:

If you are using Bearer Token Authentication, where an access token
is sent in the Authorization header as a bearer token, you can directly
provide the token as a string to the auth option.

```
axios.get('https://api.example.com/data', {
auth: 'Bearer myaccesstoken'
})
.then(response => {
// Handle the response
})
.catch(error => {
// Handle any errors
});
```

In this example, Axios will include the provided access token as a
bearer token in the Authorization header.

- By using the auth option, Axios will automatically set the appropriate
  Authorization header based on the authentication method used.

- It's important to note that the auth option is a shorthand way of setting
  the Authorization header. If you need more fine-grained control over the
  headers, you can manually set the Authorization header using the headers
  option instead.

- Please keep in mind that you should use HTTPS when sending authentication
  credentials to ensure the security of the information transmitted over
  the network.

15. responseType: 'json', // default

- `responseType` indicates the type of data that the server will respond
  with or the format of data the clint will recive from the sever.
- options are: 'arraybuffer', 'document', 'json', 'text', 'stream'

16. responseEncoding: 'utf8', // default (only for Node.js)

17. xsrfCookieName: 'XSRF-TOKEN', // default

- `xsrfCookieName` is the name of the cookie to use as
  a value for xsrf token

- The xsrfCookieName option is used in Axios to specify the
  name of the cookie that contains the Cross-Site Request
  Forgery (CSRF) token. CSRF is a security vulnerability that
  occurs when an attacker tricks a user's browser into making
  an unintended request on a trusted website, leading to
  potential unauthorized actions.

- To mitigate CSRF attacks, web applications often generate
  and include a CSRF token in each request. This token is typically
  stored in a cookie on the client-side and sent back to the server
  as a header or parameter with each subsequent request. The server
  compares the token in the request with the token stored on the
  server to validate the request's authenticity.

- In Axios, the xsrfCookieName option allows you to specify the
  name of the cookie that contains the CSRF token. By default, Axios
  assumes that the CSRF token is stored in a cookie named 'XSRF-TOKEN'.
  However, you can customize this name by setting the xsrfCookieName
  option.

```
axios.get('https://api.example.com/data', {
  xsrfCookieName: 'csrf-token'
})
  .then(response => {
    // Handle the response
  })
  .catch(error => {
    // Handle any errors
  });
```

- In this example, we set xsrfCookieName to 'csrf-token', indicating
  that the CSRF token is stored in a cookie with the name 'csrf-token'.
  Axios will automatically include the value of this cookie as a header
  ('X-XSRF-TOKEN') in subsequent requests made by Axios. This allows the
  server to verify the authenticity of the requests by comparing the
  token in the header with the token stored on the server.

- Please note that the server-side implementation must be properly
  configured to handle CSRF protection, including generating and validating
  CSRF tokens. Additionally, the server may also require you to set the
  xsrfHeaderName option in Axios to specify the name of the header where
  the CSRF token is expected.

18. xsrfHeaderName: 'X-XSRF-TOKEN', // default

- `xsrfHeaderName` is the name of the http header that carries
  the xsrf token value

- The xsrfHeaderName option in Axios is used to specify the name
  of the header that carries the Cross-Site Request Forgery (CSRF)
  token. CSRF is a security vulnerability that can occur when an
  attacker tricks a user's browser into making unintended requests
  on a trusted website.

- To prevent CSRF attacks, web applications often generate and
  include a CSRF token in each request. The server compares the
  token in the request with the token stored on the server to validate
  the request's authenticity.

- In Axios, the xsrfHeaderName option allows you to customize the name
  of the header that carries the CSRF token. By default, Axios assumes
  that the CSRF token is sent in a header named 'X-XSRF-TOKEN'.
  However, you can customize this name by setting the xsrfHeaderName option.

- By customizing the xsrfHeaderName option in Axios, you can align it
  with the server-side expectations and ensure proper communication of
  the CSRF token for protecting against CSRF attacks.

19. onUploadProgress: function (progressEvent) {
    // Do whatever you want with the native progress event
    },

- only for the clint browser
- The onUploadProgress callback function in Axios allows you to track
  the progress of an upload operation. It is triggered periodically
  during the upload process, providing information about the current progress.

- To use the onUploadProgress callback, you can pass it as a
  configuration option when making an HTTP request that involves uploading
  data.

```
axios.post('https://api.example.com/upload', formData, {
onUploadProgress: progressEvent => {
const progress = Math.round((progressEvent.loaded \* 100) / progressEvent.total);
console.log(`Upload Progress: ${progress}%`);
}
})
.then(response => {
// Handle the response
})
.catch(error => {
// Handle any errors
});
```

- In this example, we are making a POST request to
  'https://api.example.com/upload' with the formData containing
  the data to be uploaded. We include the onUploadProgress option,
  which takes a callback function that will be invoked periodically
  during the upload process.

- Inside the callback function, we can access the progressEvent
  object, which provides information about the upload progress.
  The loaded property represents the number of bytes uploaded, and
  the total property represents the total size of the data being
  uploaded. By using these values, we can calculate and display
  the upload progress as a percentage.

- As the upload progresses, the onUploadProgress callback function
  will be triggered periodically, allowing you to track and display
  the progress in your application. This is particularly useful when
  dealing with large file uploads or when you want to provide visual
  feedback to the user.

- It's important to note that the availability and accuracy of progress
  events may vary depending on the browser, server, and network conditions.
  Additionally, some servers may not provide progress information for certain
  types of requests or operations.

20. onDownloadProgress: function (progressEvent) {
    // Do whatever you want with the native progress event
    },

- only for the clint browser

- same but for douwnload.

21. maxContentLength: 2000

- The maxContentLength option in Axios allows you to
  specify the maximum allowed size, in bytes, for the
  RESPONSE content that Axios will handle. If the response
  content exceeds this limit, Axios will throw an error.

Here's an example of how to set the maxContentLength option:

```
axios.get('https://api.example.com/data', {
  maxContentLength: 5000000 // 5MB
})
  .then(response => {
    // Handle the response
  })
  .catch(error => {
    // Handle any errors
  });
```

- In this example, we set maxContentLength to 5000000,
  indicating that the maximum allowed response content size
  is 5 megabytes (5MB). If the response content exceeds this
  limit, Axios will trigger an error that can be caught in
  the catch block.

- The maxContentLength option is useful for preventing
  excessive memory consumption or potential DoS (Denial of Service)
  attacks caused by handling extremely large response content.
  By setting a reasonable limit, you can control the size of the
  responses that Axios will handle, ensuring that they are within
  acceptable bounds for your application.

- The default value for maxContentLength in Axios is -1, which
  means no maximum limit is enforced. If you want to enforce a
  specific limit, you can provide a numeric value representing
  the maximum size in bytes.

- Keep in mind that maxContentLength applies to the response
  content size, not the entire response (including headers and
  other metadata). If the response content size exceeds the specified
  limit, Axios will terminate the request and throw an error before
  the response data is fully received.

- It's important to choose an appropriate value for maxContentLength
  based on your application's requirements and the expected response sizes.

22. maxBodyLength: 2000

- `maxBodyLength` (Node only option) defines the max size of the
  http REQUEST content in bytes allowed

23. validateStatus: function (status) {
    return status >= 200 && status < 300; // default
    }

- The validateStatus option in Axios allows you to define a
  custom function to determine whether a response's HTTP status
  code should be considered a success or a failure. By default,
  Axios considers any status code within the range of 200 to 299
  (inclusive) as a success and any other status code as a failure.

24. maxRedirects: 5, // default

- The maxRedirects option in Axios allows you to specify the maximum
  number of HTTP redirects that Axios will follow. When a server responds
  with a redirect status code (e.g., 301, 302, 307), Axios can automatically
  handle the redirect and make another request to the new location indicated
  by the redirect response.

- By default, Axios will automatically follow up to 5 redirects. However,
  you can customize this behavior by setting the maxRedirects option to a
  different value or to 0 to disable automatic redirect following.

25. beforeRedirect: (options, { headers }) => {
    if (options.hostname === "example.com") {
    options.auth = "user:password";
    }
    }

- `beforeRedirect` defines a function that will be called before redirect.
- Use this to adjust the request options upon redirecting,
- to inspect the latest response headers,
- or to cancel the request by throwing an error
- If maxRedirects is set to 0, `beforeRedirect` is not used.

- the interception of a redirect response from the server before the actual redirect
  occurs. It allows you to customize the behavior of Axios before
  it automatically follows the redirect.

26. socketPath: null, // default

- `socketPath` defines a UNIX Socket to be used in node.js.
- e.g. '/var/run/docker.sock' to send requests to the docker daemon.
- Only either `socketPath` or `proxy` can be specified.
- If both are specified, `socketPath` is used.

- The socketPath option in Axios allows you to specify a Unix socket
  path for making HTTP requests. Unix sockets provide a means of
  inter-process communication (IPC) on Unix-based systems, allowing
  processes to communicate with each other using socket connections.

- In the context of Axios, the socketPath option is used when you want
  to make an HTTP request through a Unix socket instead of a traditional
  TCP connection. This can be useful in scenarios where your server/application
  communicates with other processes or services via Unix sockets.

```
axios.get('http://unix:/var/run/myapp.sock:/data', {
  socketPath: '/var/run/myapp.sock'
})
  .then(response => {
    // Handle the response
  })
  .catch(error => {
    // Handle any errors
  });
```

- In this example, we specify the URL as http://unix:/var/run/myapp.sock:/data,
  which indicates that we want to make the request through the Unix socket located
  at /var/run/myapp.sock. We set the socketPath option to /var/run/myapp.sock to
  provide the path to the Unix socket.

- By using the socketPath option, Axios will establish a connection to the
  specified Unix socket and make the HTTP request through that socket instead
  of using a traditional host and port combination.

- It's important to note that the usage of socketPath is specific to Unix-based
  systems and may not be available or applicable in other operating systems.
  Additionally, the server or service you are connecting to must support
  communication through Unix sockets for this option to work.

26. transport: undefined, // default

- `transport` determines the transport method that will be used to make the
  request. If defined, it will be used. Otherwise, if `maxRedirects` is 0, the
  default `http` or `https` library will be used, depending on the protocol
  specified in `protocol`. Otherwise, the `httpFollow` or `httpsFollow` library
  will be used, again depending on the protocol, which can handle redirects.

- In the context of Axios, the transport option refers to the underlying HTTP
  transport mechanism used for sending HTTP requests. By default, Axios uses the
  XMLHttpRequest (XHR) object as the transport mechanism in web browsers and
  the http or https modules in Node.js.

- However, Axios allows you to configure and replace the default transport
  mechanism with a custom one that suits your needs. This can be useful in
  scenarios where you want to use a different library or implementation for
  making HTTP requests.

27. httpsAgent: new https.Agent({ keepAlive: true }), (only for node.js)

- When making HTTPS requests with Axios, the httpsAgent option allows
  you to provide a custom HTTPS agent object to handle the underlying
  network connections. The HTTPS agent manages the creation and management
  of secure sockets for HTTPS communication.

Here are the key points to understand about the httpsAgent option:

- Purpose of HTTPS Agent: An HTTPS agent is responsible for managing
  the pooling, reuse, and configuration of secure sockets used in HTTPS
  connections. It handles tasks such as establishing SSL/TLS handshakes,
  managing connection timeouts, and enforcing security settings.

- Default HTTPS Agent: By default, Axios uses the built-in Node.js https
  module's global agent as the HTTPS agent. This agent has its own default
  configuration, which includes connection pooling and other optimizations.

- Customizing the HTTPS Agent: By providing a custom HTTPS agent through
  the httpsAgent option, you can override the default behavior and customize
  the agent's settings according to your specific requirements.

- https.Agent Class: In Node.js, the https.Agent class is used to create an
  HTTPS agent. This class provides various options and methods to configure
  the behavior of the agent.

- Configurable Options: The https.Agent class has several options that can
  be set when creating an instance. Some commonly used options include:

- - keepAlive: Determines whether to use keep-alive sockets for reusing
    connections.
- - maxSockets: Specifies the maximum number of sockets the agent can
    have open at any given time.

- - keepAliveMsecs: Sets the time duration in milliseconds for how long
    a socket should remain idle in the connection pool before being closed.

- - ca, cert, key: Allows you to provide custom SSL/TLS certificates or
    keys for secure communication.
- Creating a Custom HTTPS Agent: To create a custom HTTPS agent, you
  typically use the https.Agent class and instantiate it with the desired
  options. You can then pass the agent instance to the httpsAgent option
  in Axios.

```
const https = require('https');

const agent = new https.Agent({
  keepAlive: true,
  maxSockets: 10,
  // Other options...
});

axios.get('https://api.example.com/data', {
  httpsAgent: agent
})
  .then(response => {
    // Handle the response
  })
  .catch(error => {
    // Handle any errors
  });
```

- In this example, we create a custom HTTPS agent using the
  https.Agent class from the Node.js https module. We configure
  the agent with options like keepAlive and maxSockets according
  to our needs. Then, we pass the agent instance to the httpsAgent
  option when making the Axios request.

- By customizing the HTTPS agent, you can control various aspects of
  the underlying secure sockets used in HTTPS connections. This allows
  you to optimize performance, manage connection limits, or provide
  custom SSL/TLS certificates for secure communication.

- It's important to note that the available options and their effects
  may vary depending on the underlying HTTPS agent implementation and
  the specific environment in which your code is running. Therefore,
  it's recommended to refer to the documentation of the underlying HTTPS
  agent, such as the Node.js https.Agent class, for a comprehensive
  understanding of the available options and their behavior.

27. proxy: {
    protocol: 'https',
    host: '127.0.0.1',
    port: 9000,
    auth: {
    username: 'mikeymike',
    password: 'rapunz3l'
    }
    },

- Use `false` to disable proxies, ignoring environment variables.

- the proxy option allows you to specify a proxy server that will be used
  to forward your HTTP requests. By configuring the proxy option, you can
  route your requests through the specified proxy server.

```
axios.get('https://api.example.com/data', {
  proxy: {
    host: 'proxy.example.com',
    port: 8080,
    auth: {
      username: 'proxyuser',
      password: 'proxypassword'
    }
  }
})
  .then(response => {
    // Handle the response
  })
  .catch(error => {
    // Handle any errors
  });
```

- In this example, we are making an HTTP GET request to
  https://api.example.com/data and specifying the proxy option.
  The proxy object contains the configuration for the proxy server,
  including the host (the hostname or IP address of the proxy server)
  and port (the port number on which the proxy server is running).

- You can also provide optional auth configuration if the proxy server
  requires authentication. By specifying the username and password
  properties within the auth object, you can provide the necessary
  credentials for accessing the proxy server.

- Once the proxy option is configured, Axios will automatically
  send your HTTP requests through the specified proxy server. This
  can be useful when you need to access resources that are behind a
  proxy or when you want to bypass network restrictions.

- Please note that the availability and behavior of the proxy option
  may depend on the specific environment in which you are running your
  Axios code (e.g., Node.js server-side environment). Additionally, the
  proxy server itself must be correctly set up and accessible from your
  network environment for this configuration to work.

- Make sure to consult the documentation of your specific environment
  and network setup to ensure proper configuration and usage of the
  proxy option in Axios.

# Axios proxy options

- host: The hostname or IP address of the proxy server.

- port: The port number on which the proxy server is running.

- auth: An optional object that contains the authentication
  credentials for the proxy server. It has the following properties:

- - username: The username for proxy server authentication.
- - password: The password for proxy server authentication.

- protocol: The protocol used to communicate with the proxy server.
  By default, it is determined based on the requested URL's protocol
  (http:// or https://).

- headers: An optional object that contains additional headers to
  be sent to the proxy server. Each property in the object represents
  a header name, and its corresponding value represents the header value.

```
axios.get('https://api.example.com/data', {
  proxy: {
    host: 'proxy.example.com',
    port: 8080,
    auth: {
      username: 'proxyuser',
      password: 'proxypassword'
    },
    protocol: 'http',
    headers: {
      'X-Custom-Header': 'Custom Value'
    }
  }
})
  .then(response => {
    // Handle the response
  })
  .catch(error => {
    // Handle any errors
  });

```

# what is proxy?

- A proxy server acts as an intermediary between a client
  and a server, facilitating communication between them. When
  a client makes a request to a server, it goes through the
  proxy server, which forwards the request to the server on
  behalf of the client. The server responds to the proxy, which
  then relays the response back to the client.

Here are some key points to understand about proxies:

- Privacy and Anonymity: Proxies can provide privacy and
  anonymity by hiding the client's IP address from the server.
  When the client communicates through a proxy, the server only
  sees the proxy's IP address, making it harder to track the
  client's identity.

- Caching and Performance: Proxies can cache responses from
  servers and serve them directly to clients without forwarding
  the request to the server again. This can improve performance
  and reduce bandwidth usage, especially for repeated requests or
  static content.

- Content Filtering and Access Control: Proxies can be configured
  to filter content based on specific rules or policies. This allows
  organizations to restrict access to certain websites or content
  categories, implement parental controls, or enforce security policies.

- Load Balancing: Proxies can distribute incoming client requests
  across multiple servers, helping to balance the load and ensure
  efficient utilization of server resources. This improves scalability,
  availability, and overall performance of the system.

- Bypassing Network Restrictions: Proxies can be used to bypass
  network restrictions imposed by firewalls or content filtering
  systems. By tunneling the traffic through a proxy server, clients
  can access resources that may be blocked or restricted in their network
  environment.

- Logging and Monitoring: Proxies can log and monitor client-server
  communications, providing insights into network traffic, usage
  patterns, and potential security threats. This information can be
  useful for troubleshooting, auditing, and analyzing network activity.

- Proxies can be deployed at various levels, including application-level
  proxies, reverse proxies, and network-level proxies. They can be
  implemented as software applications, hardware appliances, or services
  provided by Internet Service Providers (ISPs).

- In the context of web development and HTTP communication, proxies are
  commonly used for various purposes, such as load balancing, caching,
  security, and network optimization. They provide an additional layer of
  control and flexibility in managing client-server communications.

28. CancelToken (depricated don't use it) use signal insted.

29. signal: new AbortController().signal

- In Axios, the signal option allows you to associate a signal object
  with a request in order to cancel that request using the AbortController
  mechanism. The signal option is useful when you want to cancel an ongoing
  request due to user interaction or a specific event.

- In Axios, the signal option allows you to associate an AbortSignal object
  with a request. The AbortSignal interface is part of the AbortController API,
  which provides a way to signal the cancellation of an ongoing operation.
  When the associated AbortSignal is aborted, it triggers the cancellation
  of the associated request.

Here's a step-by-step breakdown of how to use the signal option:

1. Create an AbortController instance:

```
const abortController = new AbortController();
```

- The AbortController is used to create an AbortSignal
  object and manage the cancellation state.

2. Get the signal from the AbortController:

```
const signal = abortController.signal;
```

- The signal represents the AbortSignal associated with the
  AbortController. It can be passed as the signal option in
  the Axios request configuration.

3. Make a request with the signal option:

```
axios.get('/api/data', { signal })
  .then(response => {
    // Handle successful response
  })
  .catch(error => {
    if (axios.isCancel(error)) {
      // Request was canceled
      console.log('Request canceled:', error.message);
    } else {
      // Handle other errors
      console.log('Error:', error.message);
    }
  });
```

- The signal option is included in the Axios request configuration
  to associate the AbortSignal with the request. This allows Axios to
  listen for the cancellation of the signal.

4. Cancel the request when needed:

```
abortController.abort();
```

- Calling abort() on the AbortController triggers the cancellation
  of the associated AbortSignal. This, in turn, cancels the associated
  Axios request.

- When the request is canceled due to the associated AbortSignal being
  aborted, the request will reject with an error object. You can use
  axios.isCancel(error) to check if the error is a cancellation error.

- Canceling a request is useful in scenarios where you want to provide
  a way for users to cancel ongoing requests, such as when they navigate
  away from a page or perform a different action. It helps to optimize
  network usage and prevents unnecessary processing of responses for
  canceled requests.

30. decompress: true // default (Node only)

- The decompress option in Axios allows you to automatically
  decompress the response data (recived frim the server)
  if it is compressed using a supported encoding, such as
  gzip or deflate. When enabled, Axios will automatically
  decompress the response data and provide the uncompressed
  content in the response.

Here's how you can use the decompress option in Axios:

```
axios.get('https://api.example.com/data', {
  decompress: true
})
  .then(response => {
    // Access the uncompressed response data
    console.log(response.data);
  })
  .catch(error => {
    // Handle any errors
  });
```

- In this example, we are making an HTTP GET request to
  https://api.example.com/data and enabling the decompress
  option by setting it to true in the request configuration
  object.

- Once the response is received, Axios will automatically
  decompress the response data if it is compressed using a
  supported encoding. The uncompressed response data can then
  be accessed through the data property of the response object.

- Enabling the decompress option is particularly useful when
  you are dealing with APIs or servers that return compressed
  responses to reduce the data transfer size. By enabling
  decompression in Axios, you can work with the uncompressed
  content directly, without having to handle the decompression
  manually.

- Please note that the decompress option is available in the
  Axios adapter for Node.js environments, but it may not be
  applicable or have an effect in other environments such as
  browsers, where the decompression is typically handled by
  the browser automatically.

31. transitional: {
    // silent JSON parsing mode
    // `true` - ignore JSON parsing errors and set response.data to null if parsing failed (old behaviour)
    // `false` - throw SyntaxError if JSON parsing failed (Note: responseType must be set to 'json')
    silentJSONParsing: true, // default value for the current Axios version

    // try to parse the response string as JSON even if `responseType` is not 'json'
    forcedJSONParsing: true,

    // throw ETIMEDOUT error instead of generic ECONNABORTED on request timeouts
    clarifyTimeoutError: false,

},

Transitinal option is use to support the deprecated feature of the axios.

In Axios, the transitional option is used to enable or disable transitional
behavior when working with deprecated features or behaviors. It allows you
to control how Axios handles deprecated functionality and provides a smoother
migration path when upgrading to newer versions.

The transitional option is an object with various properties that can
be set to true or false to enable or disable specific transitional behaviors.

These properties include:

1. silentJSONParsing: When set to true, it disables the warning
   message when Axios automatically parses JSON responses for non-2xx
   status codes. This behavior is deprecated, and Axios recommends
   handling the response manually. By default, it is set to false.

2. forcedJSONParsing: When set to true, it forces Axios to parse
   JSON responses for all status codes, even if the response does not
   contain the appropriate Content-Type header. This behavior is
   deprecated, and Axios recommends handling the response manually.
   By default, it is set to false.

3. clarifyTimeoutError: When set to true, it adds additional information
   to the error message for timeout errors, providing more details about
   the timeout. By default, it is set to false.

Here's an example of how to use the transitional option:

```
axios.get('/api/data', {
  transitional: {
    silentJSONParsing: true,
    forcedJSONParsing: false,
    clarifyTimeoutError: false
  }
})
  .then(response => {
    // Handle successful response
  })
  .catch(error => {
    // Handle error
  });
```

- In the example above, the transitional option is included in the
  Axios request configuration object. The silentJSONParsing property
  is set to true to suppress the warning message for automatic JSON
  parsing, while the other transitional properties are set to their
  default values.

- By using the transitional option, you can customize the behavior of
  Axios when working with deprecated features. It gives you more control
  over how Axios handles specific functionalities during the migration
  process to newer versions or when dealing with specific use cases that
  require different transitional behaviors.

- It's worth noting that transitional behaviors are typically introduced
  to provide backward compatibility or a smoother migration path, but they
  may be removed or changed in future versions of Axios. It's recommended
  to consult the Axios documentation and keep track of any changes or
  deprecations related to transitional behaviors when upgrading your
  Axios version.

32. env: {
    // The FormData class to be used to automatically serialize the payload
    // into a FormData object
    FormData: window?.FormData || global?.FormData
    },

env refer to enviroment in which environment you are using the FromData

```
//formData before serialization
const payload = {
 name: 'John Doe',
 email: 'john@example.com',
 age: 25
};
-------------------------------------------
//formData after serialization
const formData = new FormData();
formData.append('name', payload.name);
formData.append('email', payload.email);
formData.append('age', payload.age);
```

with "FormData: window?.FormData || global?.FormData" this code Axios
will automatically serialize our form data which makes it easier to
send data over HTTP request.

33. formSerializer: {
    visitor: (value, key, path, helpers) => {}; // custom visitor function to serialize form values
    dots: boolean; // use dots instead of brackets format
    metaTokens: boolean; // keep special endings like {} in parameter key
    indexes: boolean; // array indexes format null - no brackets, false - empty brackets, true - brackets with indexes
    }

formSerializer allows you to customize how form values are serialized
when using URL-encoded form data in Axios requests.

Let's break down each property of the configuration:

1. visitor: This property specifies a custom visitor function that can be used
   to serialize form values. The visitor function is called for each key-value
   pair in the form data and allows you to modify or transform the values before
   they are serialized. The function receives four parameters: value (the current value),
   key (the current key), path (the path to the current value), and
   helpers (a set of helper functions). You can define your own logic within the
   visitor function to handle specific serialization requirements.

2. dots: This property controls whether dots (.) should be used instead of brackets ([])
   for nested form fields. When set to true, nested form fields will be serialized
   using dots instead of brackets.

3. metaTokens: This property determines whether special endings like {} should be
   preserved in parameter keys. When set to true, the special endings will be
   retained in the serialized form data.

4. indexes: This property controls the format used for array indexes in serialized form
   data. When set to null, no brackets will be used for array indexes. When set to false,
   empty brackets ([]) will be used. When set to true, brackets with indexes will be
   used (e.g., [0], [1], etc.).

You can customize these options based on your specific needs for form data
serialization. By providing a custom visitor function or adjusting the boolean
options, you can tailor the serialization process to match the desired format
or API requirements for URL-encoded form data.

Keep in mind that the formSerializer option is specific to the form-urlencoded
library used internally by Axios. It allows you to configure the serialization
behavior for URL-encoded form data, but it doesn't apply to other data formats
like JSON or multipart/form-data.

34. maxRate: [
    100 * 1024, // 100KB/s upload limit,
    100 * 1024 // 100KB/s download limit
    ]

- http adapter only (node.js)

- In the array [100 * 1024, 100 * 1024], the first element 100 _ 1024 represents
  the upload limit, and the second element 100 _ 1024 represents the download limit.
  Both values are specified in bytes per second.

- By setting these limits, you can control the rate at which data is uploaded and
  downloaded during Axios requests. In this example, the upload and download limits
  are both set to 100KB/s (kilobytes per second). This means that the request will
  be throttled to upload and download data at a maximum rate of 100 kilobytes per second.

- Throttling the upload and download rates can be useful in scenarios where you
  want to limit bandwidth usage or control the flow of data in your application.
  By specifying these limits, you can prevent excessive data transfers and ensure
  a smoother data transmission experience.

### Response Schema

the response that you get from the server contain there informations.

1.  data: {}

- `data` is the response that was provided by the server.

2.  status: 200

- `status` is the HTTP status code from the server response.

3.  statusText: 'OK'

- `statusText` is the HTTP status message from the server response.

4. headers: {}

- `headers` the HTTP headers that the server responded with
- All header names are lower cased and can be accessed using
  the bracket notation.
- Example: `response.headers['content-type']`

5.  config: {}

- `config` is the config that was provided to `axios` for the
  request (config request that you have sent)
- config is used when using an instance.

```
const instance = axios.create({
  baseURL: 'https://api.example.com',
  timeout: 5000,
  headers: {
    'Content-Type': 'application/json'
  }
});

instance.get('/users')
  .then(response => {
    // Handle the response
  })
  .catch(error => {
    // Handle any errors
  });

```

- config object all the key value that you have used to make the
  request by instance

6.  request: {}

- request object is same as config object but it is for single request.

```
axios({
  url: '/api/users',
  method: 'GET',
  params: {
    page: 1,
    limit: 10
  }
})
  .then(response => {
    // Handle the response
  })
  .catch(error => {
    // Handle any errors
  });

```

- request object all the key value that you have used to make the request
  by individual request

## Config Defaults

In Axios, you can set global defaults for all Axios requests by
using the axios.defaults object. This allows you to define default
configuration options that will be applied to every request made
through Axios.

```
1. axios.defaults.baseURL = 'https://api.example.com';
2. axios.defaults.headers.common['Authorization'] = 'Bearer your_token_here';(common return undefined)
3. axios.defaults.headers['Authorization'] = 'Bearer your_token_here';
4. axios.defaults.timeout = 5000;

2 and 3 are same
```

By setting these defaults, you don't have to specify them for each
individual request. However, you can still override these defaults
on a per-request basis by providing specific configuration options
when making a request.

It's important to note that axios.defaults is a mutable object, so
any changes made to it will affect all subsequent Axios requests.
This can be useful for setting common headers, default URLs, or
timeouts across your application.

## custom instance

you can create an Axios instance with custom defaults using the
axios.create() method. This allows you to have different sets
of defaults for different parts of your application.

```
const instance = axios.create({
  baseURL: 'https://api.example.com',
  timeout: 5000,
  headers: {
    'Authorization': 'Bearer your_token_here'
  }
});
```

## alter custom instance

you can add or overwrite custom instance properties by
instance.defaults object

```
// Set config defaults when creating the instance
const instance = axios.create({
  baseURL: 'https://api.example.com'
});

// Alter defaults after instance has been created
instance.defaults.headers.common['Authorization'] = AUTH_TOKEN;
```

### order of precedence

```
// highest precidence| Override timeout to 5 sec               ^
instance.get('/longRequest', {                                 |
  timeout: 5000                                                |
});                                                            |
                                                               ^
// Override timeout to 3 sec                                   |
instance.defaults.timeout = 3000;                              |
                                                               ^
// create an instance where timeout is 2 sec                   |
const instance = axios.create({                                |
   timeout: 2000                                               |
});                                                            |
                                                               ^
// lowest precedence| create a default time to 1 sec           |
axios.defaults.timeout = 1000;                                 |
```

1. The configuration options passed directly to the request
   method (axios.get(), axios.post(), etc.) take the
   highest precedence.

2. axios.default has the lowest precedence.

### Axios interceptors

Axios interceptors are functions that allow you to intercept
and manipulate HTTP requests and responses before they are
handled by your application's code. Interceptors are a powerful
feature of Axios that enable you to add global request/response
handling logic, such as modifying headers, logging,
error handling, and more.

Note: intercepter are async by default

Axios provides two types of interceptors:

1. Request interceptors: Request interceptors are functions
   that are executed before the request is sent to the server.
   You can use request interceptors to modify the request
   configuration, add headers, handle authentication, or perform
   any other pre-request manipulation. Request interceptors can
   be useful for implementing features like automatic token
   refresh or adding common headers to every request.

Here's an example of adding a request interceptor:

```
axios.interceptors.request.use(config => {
  // Modify the request config
  config.headers['Authorization'] = 'Bearer your_token_here';
  return config;
}, error => {
  // Handle request error
  return Promise.reject(error);
});
```

- In this example, the axios.interceptors.request.use function
  adds a request interceptor. The provided function receives the
  request config object and can modify it as needed. It then
  returns the modified config or a new config object. You can also
  handle errors that occur DURING THE INTERCEPTION PROCESS.

2. Response interceptors: Response interceptors are functions
   that are executed after a response is received from the server
   but before it's passed to the application code. Response
   interceptors allow you to globally handle and manipulate the
   response data or perform any other post-response processing.
   Response interceptors can be used, for example, to handle
   common error responses or format data consistently.

Here's an example of adding a response interceptor:

```
axios.interceptors.response.use(response => {
  // Modify the response data
  response.data = response.data.toUpperCase();
  return response;
}, error => {
  // Handle response error
  return Promise.reject(error);
});
```

- In this example, the axios.interceptors.response.use function adds
  a response interceptor. The provided function receives the response
  object and can modify it as needed. It then returns the modified
  response or a new response object. You can also handle errors that
  occur DURING THE INTERCEPTION PROCESS.

- You can add multiple interceptors by calling the
  axios.interceptors.request.use or axios.interceptors.response.use
  functions multiple times. The interceptors will be executed
  in the order they were added.

- Interceptors are powerful for handling global request and response
  behavior in a centralized manner. They allow you to abstract common
  logic, reduce code duplication, and provide consistent behavior
  across your application's HTTP requests.

# If you need to remove an interceptor later you can.

```
const myInterceptor = axios.interceptors.request.use(function () {/*...*/});
axios.interceptors.request.eject(myInterceptor);
```

# You can also clear all interceptors for requests or responses.

```
const instance = axios.create();
instance.interceptors.request.use(function () {/*...*/});
instance.interceptors.request.clear(); // Removes interceptors from requests
```

# You can add interceptors to a custom instance of axios.

```
const instance = axios.create();
instance.interceptors.request.use(function () {/*...*/});
```

# You can make interception sync

When you add request interceptors, they are presumed to be asynchronous
by default. This can cause a delay in the execution of your axios request
when the main thread is blocked (a promise is created under the hood for
the interceptor and your request gets put on the bottom of the call stack).
If your request interceptors are synchronous you can add a flag to the
options object that will tell axios to run the code synchronously and
avoid any delays in request execution.

```
axios.interceptors.request.use(function (config) {
  config.headers.test = 'I am only a header!';
  return config;
}, null, { synchronous: true });
```

# Run intercepter conditionaly

If you want to execute a particular interceptor based on a runtime check,
you can add a runWhen function to the options object. The interceptor will
not be executed if and only if the return of runWhen is false. The function
will be called with the config object (don't forget that you can bind your
own arguments to it as well.) This can be handy when you have an asynchronous
request interceptor that only needs to run at certain times.

```
function onGetCall(config) {
  return config.method === 'get';
}
axios.interceptors.request.use(function (config) {
  config.headers.test = 'special get headers';
  return config;
}, null, { runWhen: onGetCall });
```

### Handling Errors in Axios

there are mutiple methods to handle errors.

1. Using .then( response => {}) and .catch(error => {}):

catch will catch the error and block the execution.

2. Using try-catch with async/await

```
try {
  const response = await axios.get('/api/data');
  // Handle successful response
} catch (error) {
  // Handle error
}
```

3. Custom error handling with response status codes:

the default behavior is to reject every response that returns
with a status code that falls out of the range of 2xx and treat
it as an error.

```
axios.get('/api/data')
  .then(response => {
    // Handle successful response
  })
  .catch(error => {
    if (error.response) {
      // The request was made and the server responded with a status code
      // that falls outside the range of 2xx
      console.log(error.response.data);
      console.log(error.response.status);
      console.log(error.response.headers);
    } else if (error.request) {
      // The request was made but no response was received
      console.log(error.request);
    } else {
      // Something happened in setting up the request that triggered an error
      console.log('Error', error.message);
    }
  });


```

the error object provides additional information about the error.

- If error.response exists, it means the request was made and the
  server responded with a status code outside the range of 2xx.
  You can access the response data, status code, and headers from
  error.response.

- If error.request exists, it means the request was
  made but no response was received. You can access the request
  details from error.request.

- If none of these properties exist,
  it means an error occurred during the request setup, such as a
  network failure or CORS issue.

# Note

error.response object entire schema of response which contain all the property that
was sent by the server for the browser user.

# Error handleing with validateStatus

you can configer the validateStatus . if the validateStatus return true it mean request successful if false then request false.

```
axios.get('/user/12345', {
  validateStatus: function (status) {
    return status < 500; // Resolve only if the status code is less than 500
  }
})
```

# Using toJSON you get an object with more information about the HTTP error.

```
axios.get('/user/12345')
  .catch(function (error) {
    console.log(error.toJSON());
  });
--------------------------------------
{
  "message": "Request failed with status code 404",
  "name": "Error",
  "stack": "<stack trace goes here>"
}
```

Erroe.JSON() will return an object which contain standardized way to extract
essential information from the error object and convert it to a JSON format.
you can use it to display a customized error with greater details.

## Cancelling requests in Axios?

you can use AbortController to cancel request.

you can use AbortController two ways.

1. with user interection.

when user click or leave the page.

```
const controller = new AbortController();

axios.get('/foo/bar', {
   signal: controller.signal
}).then(function(response) {
   //...
});
// cancel the request
controller.abort()
```

execute controller.abort() with an event.

2. with timeout.

when the time is up the request will be canceled/Aborted

```
axios.get('/foo/bar', {
   signal: AbortSignal.timeout(5000) //Aborts request after 5 seconds
}).then(function(response) {
   //...
});
```

Note: there is no need to create new AbortController() with AbortSignal.timeout(time in mili sec)
