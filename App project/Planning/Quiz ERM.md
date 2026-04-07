
```mermaid
---
title: Quiz
---
erDiagram
	quiz {
		int quizId
		string name
	}
	
	question {
		int questionId
		int quizId
		string question
		string answer
	}
	
	answer_history {
		int answerId
		int questionId
		bool answeredCorrect
		string timestamp
	}
		
	quiz ||--|{ question: has
	question ||--o| answer_history: has
```
