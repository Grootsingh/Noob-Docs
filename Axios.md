> from the docs:

Axios 1.4.0

## why use Axios over fetch?

- Axios is capabile of Intercepting both request and response.
- The Fetch API doesn’t have an onprogress handler for showing progress bar
  but Axios does becouse it uses XMLHTTPRequest.

- Axios converts data to json format (by default)(also possible in fetch)
- Better Error handling including 404 and 500 error.(also possible in fetch)
- Axios provides built-in support for request cancellation and request timeout.(also possible in fetch)

## how to install

install code:

```
$ npm install axios
```

after installation import:

```
import axios from 'axios';
```

## Get request fetch vs Axios

```js
// fetch()

fetch("https://api.github.com/orgs/axios")
  .then((response) => response.json()) // one extra step
  .then((data) => {
    console.log(data);
  })
  .catch((error) => console.error(error));
```

```js
// axios
axios.get("https://api.github.com/orgs/axios").then(
  (response) => {
    console.log(response.data);
  },
  (error) => {
    console.log(error);
  }
);
```

- the only beneft with Axios get request is that by default you get JSON formatted data. thats it and it is not something that can't be done in fetch.

- one other beneft with Axios that it can handle all type of error so we don't need try and catch with Axios.

## Post request fetch vs Axios

```js
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
  .then((data) => {
    console.log(data);
  });
```

```js
// axios

const url = "https://jsonplaceholder.typicode.com/posts";

const data = {
  a: 10,
  b: 20,
};

const config = {
  headers: {
    Accept: "application/json",
    "Content-Type": "application/json;charset=UTF-8",
  },
};

axios.post(url, data, config).then(({ data }) => {
  console.log(data);
});
```

- To send data, fetch() uses the body property for a post request to send data to the endpoint, while Axios uses the data property

- The data in fetch() is needed to be transformed to a string using the JSON.stringify method before posting request to the endpoint while Axios transform the data to JSON format automatically.

- Axios automatically transforms the data returned from the server, but with fetch() you have to call the response.json method to parse the data to a JavaScript object.

- With Axios, the data response provided by the server can be accessed with in the data object, while for the fetch() method, the final data needed to be name (response) then we need to format it with JSON and then we will recive that data in next promise.

## how to make request with Axios (the Hard way)

Requests can be made by passing the relevant config to axios.

you can make a request by passing the config object with relevent method on that which define which type of request you are making:

for post request:

```js
axios({
  method: 'post',
  url: '/user/12345',
  data: {
    firstName: 'Fred',
    lastName: 'Flintstone'
  }
```

for get request:

```js
axios({
  method: 'get',
  url: 'https://bit.ly/2mTM3nY',
  responseType: 'stream'
}).then((response) => {
    response.data.pipe(fs.createWriteStream('ada_lovelace.jpg'))
  });
});
```

this is how you can make request with Axios but it's not recommended becouse Axios also provide Shorthand for all type of request you can Imangine.

## Axios Shorthand for request

- axios.request(config)
- axios.get(url, config)
- axios.delete(url, config)
- axios.head(url, config)
- axios.options(url, config)
- axios.post(url, data, config)
- axios.put(url, data, config)
- axios.patch(url, data, config)

> [!NOTE]
> if you don't specify any method axios will take get method
> as default. so, axios(url) = axios.get(url)

When using the Axios Shorthand url, method, and data properties you don't need to be specified in config.

this is becouse axios.method(url,data,config) all are present in the shorthand to begin with.

## Sending multiple request with Axios

use promise.all() to make multiple request

## The Axios Instance

create a custom instance of Axios

```
instance foramt: axios.create(config)
```

```js
const instance = axios.create({
  baseURL: "https://some-domain.com/api/",
  timeout: 1000,
  headers: { "X-Custom-Header": "foobar" },
});
```

here we have created a instance which contain config for the request. it is used to
make a general instance so we don't have to repeat our config code again and agian
we can use this istance to make different different reqest.

Ex: `const userData = instance.get("/user")`
with this get request we are using url = baseURL + url and some additional config setting
are also set like `timeout: 1000` and `headers: {'X-Custom-Header': 'foobar'}` with this request.

we can use all the AxiosShorthand with Axios Instances.

## Request Config object

### what keys can config object can take:

1. `url: '/user'` - is the server URL that will be used for the request.

2. `method: 'get', // default` - request method type

3. `baseURL: 'https://some-domain.com/api'` - `baseURL` will be prepended to `url` unless `url` is absolute.
   It can be convenient to set `baseURL` for an instance of axios to pass
   relative URLs to methods of that instance.

4. `transformRequest: [function (data, headers) {`<br>
   `    // Do whatever you want to transform the data`<br>
   `    return data;`<br>
   `    }]` - the transformRequest option allows you to define a function that will be applied to the request data before it is sent to the server. This function can be used to transform the data in a specific format or perform any necessary preprocessing.

> [!NOTE]
> only applicable for request methods 'PUT', 'POST', 'PATCH' and 'DELETE'

5. `transformResponse: [function (data) {`<br>
   `// Do whatever you want to transform the data`<br>
   `return data;`<br>
   `}]`

- the transformResponse option allows you to define a function that will be

  applied to the response data before it is passed to then or catch handlers.
  This function can be used to transform the response data in a specific format
  or perform any necessary post-processing.
