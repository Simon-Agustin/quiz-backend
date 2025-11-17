# Quiz Maker API Endpoints

## Base URL
`http://localhost:4000` (default)

## Authentication
All endpoints require: `Authorization: Bearer <API_TOKEN>` header

---

## Endpoints Table

| Method | Endpoint | Description | Request Body | Response |
|-------|----------|------------|--------------|----------|
| **QUIZZES** |
| `GET` | `/quizzes` | List all quizzes | - | Array of quiz objects |
| `POST` | `/quizzes` | Create a new quiz | `{ title, description, timeLimitSeconds?, isPublished? }` | Created quiz object |
| `GET` | `/quizzes/:id` | Get quiz by ID with questions (creator view) | - | Quiz object with questions array |
| `PATCH` | `/quizzes/:id` | Update quiz metadata | `{ title?, description?, timeLimitSeconds?, isPublished? }` | Updated quiz object |
| **QUESTIONS** |
| `POST` | `/quizzes/:id/questions` | Create a question | `{ type, prompt, options?, correctAnswer?, position? }` | Created question object |
| `PATCH` | `/questions/:id` | Update question | `{ type?, prompt?, options?, correctAnswer?, position? }` | Updated question object |
| `DELETE` | `/questions/:id` | Delete a question | - | 204 No Content |
| **ATTEMPTS** |
| `GET` | `/attempts` | List all attempts | - | Array of attempt objects with quiz info |
| `GET` | `/attempts/:id` | Get attempt by ID | - | Attempt object with quiz info |
| `POST` | `/attempts` | Start a new quiz attempt | `{ quizId }` | Attempt object with sanitized quiz |
| `POST` | `/attempts/:id/answer` | Submit/update an answer | `{ questionId, value }` | `{ ok: true }` |
| `POST` | `/attempts/:id/submit` | Submit attempt for grading | - | `{ score, details: [...] }` |
| `POST` | `/attempts/:id/events` | Track anti-cheat events | `{ event: string }` | `{ ok: true }` |

---

## Question Types

| Type | Description | Required Fields |
|------|-------------|----------------|
| `mcq` | Multiple Choice Question | `prompt`, `options` (array, >= 2), `correctAnswer` (index or text) |
| `short` | Short Answer Question | `prompt`, `correctAnswer` (string) |
| `code` | Code Question | `prompt` (not auto-graded) |

---

## Response Status Codes

| Code | Meaning |
|------|---------|
| `200` | Success |
| `201` | Created |
| `204` | No Content (successful deletion) |
| `400` | Bad Request (validation error) |
| `401` | Unauthorized (missing/invalid token) |
| `404` | Not Found |
| `500` | Internal Server Error |

---

## Example Request Bodies

### Create Quiz
```json
{
  "title": "Sample Quiz",
  "description": "This is a sample quiz",
  "timeLimitSeconds": 300,
  "isPublished": false
}
```

### Create MCQ Question
```json
{
  "type": "mcq",
  "prompt": "What is 2 + 2?",
  "options": ["3", "4", "5", "6"],
  "correctAnswer": 1,
  "position": 0
}
```

### Create Short Answer Question
```json
{
  "type": "short",
  "prompt": "What is the capital of France?",
  "correctAnswer": "Paris",
  "position": 1
}
```

### Create Code Question
```json
{
  "type": "code",
  "prompt": "Write a function that returns the sum of two numbers",
  "position": 2
}
```

### Start Attempt
```json
{
  "quizId": 1
}
```

### Submit Answer
```json
{
  "questionId": 1,
  "value": "4"
}
```

### Track Event
```json
{
  "event": "tab_switch"
}
```


