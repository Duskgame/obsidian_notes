A **payload** is the data carried in the body of an HTTP request or response — the actual content, as opposed to metadata like headers or the URL.

## Request payload

When a client sends data to a server (POST, PUT), the payload is the body of the request:

```
POST /quizzes/upload
Content-Type: application/json

{
  "quiz": { "quizId": "q1", "name": "Geography" },
  "questions": [
    { "questionId": "q1", "quizId": "q1", "question": "Capital of France?", "answer": "Paris" }
  ]
}
```

The headers (Content-Type, Authorization, etc.) describe the payload. The payload is the JSON object itself.

## Response payload

When a server responds with data, the payload is the response body:

```
200 OK
Content-Type: application/json

[ { "quiz": {...}, "questions": [...] } ]
```

## When there is no payload

- `GET` requests typically have no request payload — the parameters are in the URL
- `DELETE` requests typically have no request payload
- Responses with status `204 No Content` have no payload by design

## In Ktor Client

`setBody(payload)` sets the request payload. `.body()` reads the response payload and deserializes it:

```kotlin
// sending a payload
kwizzHttpClient.post(url) {
    contentType(ContentType.Application.Json)
    setBody(myDataObject)   // this is the payload
}

// reading a payload
val result: MyType = kwizzHttpClient.get(url).body()
```

## Related

- [[REST]]
- [[HTTP Headers]]
- [[Ktor Client]]
