# React basic REST Client

Simplify the RESTful calls of your React app.

## Instalation

```
npm install --save git+https://github.com/parrotmac/react-basic-rest-client
```

## Simple usage

Create your own api client by extending the RestClient class

```javascript
import RestClient from 'react-basic-rest-client';

export default class YourRestApi extends RestClient {
  constructor () {
    // Initialize with your base URL
    super('https://api.example.com');
  }
  // Now you can write your own methods easily
  login (username, password) {
    // Returns a Promise with the response.
    return this.POST('/auth', { username, password });
  }
  getCurrentUser () {
    // If the request is successful, you can return the expected object
    // instead of the whole response.
    return this.GET('/auth')
      .then(response => response.user);
  }
};
```

Then you can use your custom client like this

```javascript
const api = new YourRestApi();
api.login('johndoe', 'p4$$w0rd')
  .then(response => response.token)   // Successfully logged in
  .then(token => saveToken(token))    // Remember your credentials
  .catch(err => alert(err.message));  // Catch any error
```

## Advanced usage

```javascript
import RestClient from 'react-basic-rest-client';

export default class YourRestApi extends RestClient {
  constructor (authToken) {
    super('https://api.example.com', {
      headers: {
        // Include as many custom headers as you need
        Authorization: `JWT ${authToken}`
        // Content-Type: application/json
        // and
        // Accept: application/json
        // are added by default
      },
      // Simulate a slow connection on development by adding
      // a 2 second delay before each request.
      devMode: __DEV_,
      simulatedDelay: 2000
    });
  }
  getWeather (date) {
    // Send the url query as an object
    return this.GET('/weather', { date })
      .then(response => response.data);
  }
  checkIn (lat, lon) {
    return this.POST('/checkin', { lat, lon });
  }
};
```

## Reference

### super(_baseUrl_ [, _options_])

You _must_ call the parent constructor as shown in the example above.

| Parameter   |  Type  | Required |  Default  |
|:------------|:------:|:--------:|:---------:|
| **baseUrl** | String |    Yes   | undefined |
| **options** | Object |    No    |     {}    |

#### options object

Supports the following values

|       Key          |   Type  | Required | Default |                                                                                                                                           Comments                                                                                                                                          |
|:-------------------|:-------:|:--------:|:-------:|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **headers**        | String  |    No    |    {}   | Headers to be appended to the request. RestApi will always include `Content-Type: application/json` and `Accept: application/json`.                                                                                                                                                         |
| **devMode**        | Boolean |    No    |  false  | When true, it enables the simulatedDelay.
| **simulatedDelay** | Number  |    No    |    0    | Useful for simulating a slow connection. Number of milliseconds to wait before making the request. NOTE: It will only take effect if devMode is true.

### this.GET(route [, query])
### this.POST(route [, body])
### this.PUT(route [, body])
### this.DELETE(route [, query])

Each one of these methods returns a Promise with the response as the parameter.

|  Parameter |  Type  | Required | Default |                            Comments                            |
|:-----------|:------:|:--------:|:-------:|:---------------------------------------------------------------|
| **route**  | String |    Yes   |    ''   | Partial route to be appended to the baseUrl                    |
| **query**  | Object |    No    |    {}   | Object to be encoded and appended as the query part to the URL |
| **body**   | Object |    No    |    {}   | Data to be sent as the JSON body of the message                |

## Limitations

* This library only supports JSON request and response bodies. If the response is not
a JSON object, it will throw a JSON parse error.

* This is just a modified version of [https://github.com/javorosas/react-native-rest-client](https://github.com/javorosas/react-native-rest-client)

## License

MIT
