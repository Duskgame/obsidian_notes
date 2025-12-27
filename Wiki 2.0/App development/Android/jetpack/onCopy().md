**Note on copy() method:** Use the `copy()` function to copy an object, allowing you to alter some of its properties while keeping the rest unchanged.

Example:

`val jack =` `User(name =` `"Jack", age =` `1)`

`val olderJack = jack.copy(age =` `2)`


```
fun checkUserGuess() {  
  
    if (userGuess.equals(currentWord, ignoreCase = true)) {  
  
    } else {  
    }  
    // Reset user guess  
    updateUserGuess("")  
    _uiState.update { currentState ->  
        currentState.copy(isGuessedWordWrong = true)  
}
```