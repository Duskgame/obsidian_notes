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
	
	user_answer_history {
		int answerId
		int questionId
		int answeredCorrectly
		int answeredWrong
		bool lastAnsweredCorrectly
	}
	
	quiz ||--|{ question: has
	question ||--|| user_answer_history: has
```
