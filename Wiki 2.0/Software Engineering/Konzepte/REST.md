**REST** (Representational State Transfer) is the standard pattern for designing HTTP APIs. A REST API exposes resources (quizzes, questions, users) at URLs, and clients interact with them using standard HTTP methods.

## HTTP Methods

Each method has a defined meaning that you should follow consistently:

| Method | Meaning | Changes data? | Example |
|---|---|---|---|
| `GET` | Read a resource | No | `GET /quizzes` |
| `POST` | Create a new resource | Yes | `POST /quizzes` |
| `PUT` | Replace/update a resource | Yes | `PUT /quizzes/{id}` |
| `DELETE` | Delete a resource | Yes | `DELETE /quizzes/{id}` |

`GET` must never modify data — it should be safe to call any number of times with no side effects.

## HTTP Status Codes

Status codes tell the client what happened. They are grouped by first digit:

| Code | Meaning | When to use |
|---|---|---|
| `200 OK` | Success | General success for GET, PUT, DELETE |
| `201 Created` | Resource created | After a successful POST |
| `400 Bad Request` | Client sent invalid data | Missing fields, wrong format |
| `404 Not Found` | Resource doesn't exist | ID not found in database |
| `500 Internal Server Error` | Server crashed | Unhandled exception |

## URL design

URLs identify resources, not actions. The action is expressed by the HTTP method:

```
✅ GET    /quizzes          → get all quizzes
✅ POST   /quizzes          → create a quiz
✅ DELETE /quizzes/q1       → delete quiz q1
✅ GET    /quizzes/q1/questions → get questions for quiz q1

❌ GET /getQuizzes          → verb in URL (wrong)
❌ POST /deleteQuiz/q1      → action in URL (wrong)
```

## POST vs PUT — when to use which

This is a common source of confusion:

| | `POST` | `PUT` |
|---|---|---|
| Purpose | Create a new resource | Update an existing resource |
| URL contains ID? | No — the server assigns the ID | Yes — you target a specific resource |
| Call it twice? | Creates two resources | Same result both times |

```
POST /quizzes           → creates a new quiz, ID assigned
PUT  /quizzes/q1        → updates quiz with ID q1
```

**Idempotency:** PUT is *idempotent* — calling it 10 times with the same data produces the same result as calling it once. POST is not — calling it 10 times creates 10 records. This is why browsers warn you "are you sure you want to resubmit?" when you refresh after a POST.

## Nested resources

When one resource belongs to another, reflect that in the URL:

```
/quizzes/{id}/questions          → questions belonging to a specific quiz
/quizzes/{id}/questions/{qid}    → a specific question within a specific quiz
```

This makes the ownership relationship clear from the URL alone. It also means you can always navigate up the hierarchy: strip `/questions/{qid}` and you have the parent quiz.

## Request and Response flow

```
Client sends:   POST /quizzes
                Content-Type: application/json
                Body: { "quizId": "q1", "name": "Geography" }

Server responds: 201 Created
                 Body: { "quizId": "q1", "name": "Geography" }
```

The `Content-Type: application/json` header tells the server the body is JSON. Without it, the server doesn't know how to parse the body.

## Related

- [[HTTP Headers]]
- [[JSON]]
- [[Ktor Server]]
