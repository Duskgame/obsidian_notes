https://ktor.io/docs/server-requests.html#path_parameters

### Path parameter﻿[](https://ktor.io/docs/server-routing.html#path_parameter)

A [[Ktor]] path [[Parameter]] (`{param}`) matches a path segment and captures it as a parameter named `param`. This path segment is mandatory, but you can make it optional by adding a question mark: `{param?}`. For example:

- `/user/{login}` matches `/user/john`, but doesn't match `/user`.
    
- `/user/{login?}` matches `/user/john` as well as `/user`.
    
    

- > Note that optional path parameters `{param?}` can only be used at the end of the path.
    

To access a parameter value inside the route handler, use the `call.parameters` property. For example, `call.parameters["login"]` in the code snippet below will return admin for the `/user/admin` path:

```
get("/user/{login}") {
    if (call.parameters["login"] == "admin") {
        // ...
    }
}
```

### tip

If a request contains a query string, `call.parameters` also includes parameters of this query string. To learn how to access a query string and its parameters inside the handler, see [Query parameters](https://ktor.io/docs/server-requests.html#query_parameters).