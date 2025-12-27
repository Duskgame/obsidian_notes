- [[#GameScreen.kt|GameScreen.kt]]
	- [[#GameScreen.kt#How the UI reads state (state → UI)|How the UI reads state (state → UI)]]
	- [[#GameScreen.kt#How the UI sends events to the ViewModel (UI → events → ViewModel)|How the UI sends events to the ViewModel (UI → events → ViewModel)]]
	- [[#GameScreen.kt#Code|Code]]
- [[#GameUiState.kt|GameUiState.kt]]
- [[#GameViewModel.kt|GameViewModel.kt]]
	- [[#GameViewModel.kt#What state the ViewModel owns|What state the ViewModel owns]]
	- [[#GameViewModel.kt#How the ViewModel handles events and updates state (events → state)|How the ViewModel handles events and updates state (events → state)]]
	- [[#GameViewModel.kt#Code|Code]]
- [[#MainActivity.kt|MainActivity.kt]]
- [[#Overall data & event flow summary|Overall data & event flow summary]]



## GameScreen.kt

### How the UI reads state (state → UI)

In `GameScreen` the [[State in Compose|State]] from the ViewModel is observed and passed down to composables

- `collectAsState()` turns the `StateFlow<GameUiState>` into a Compose state value (`gameUiState`) so the UI automatically recomposes when `_uiState` changes.
- `GameLayout` and `GameStatus` get **plain values** (strings, booleans, ints) from `gameUiState` and from `userGuess`. They do not know about the ViewModel or flows, only about data passed in.
- `FinalScoreDialog` is shown when `gameUiState.isGameOver` becomes `true`, also coming from the ViewModel state.

This is the **state flows down** part: ViewModel → `uiState`/`userGuess` → composables via parameters.

### How the UI sends events to the ViewModel (UI → events → ViewModel)

The UI never changes state directly; it calls functions on the ViewModel. Those functions are the **events**.

In `GameLayout`:
```
OutlinedTextField(
    value = userGuess,
    onValueChange = onUserGuessChanged,
    ...
)

```

In `GameScreen`:
```
GameLayout(
    onUserGuessChanged = { gameViewModel.updateUserGuess(it) },
    userGuess = gameViewModel.userGuess,
    ...
)
```

In `GameViewModel`:
```
fun updateUserGuess(guessedWord: String) {
    userGuess = guessedWord
}

```

Flow:  
User types → `OutlinedTextField` calls `onValueChange` → `GameLayout` forwards to `onUserGuessChanged` → `GameScreen` calls `gameViewModel.updateUserGuess(it)` → ViewModel updates `userGuess`. Because `userGuess` uses `mutableStateOf`, any composable reading it will recompose.


From keyboard “Done” and Submit button

Keyboard:

```
keyboardActions = KeyboardActions(
     onDone = { onKeyboardDone() }
)
```

`GameScreen` passes:

```
onKeyboardDone = { gameViewModel.checkUserGuess() }
```

Button:

```
Button(
     onClick = { gameViewModel.checkUserGuess() }
) { ... }
```

Both **keyboard Done** and **Submit button** trigger the same event: `checkUserGuess()` in the ViewModel.

From Skip button

```
OutlinedButton(
     onClick = { gameViewModel.skipWord() },
    ... 
)
```

Skip invokes `skipWord()` in the ViewModel.

From FinalScoreDialog “Play again”


```
FinalScoreDialog(
     score = gameUiState.score,
     onPlayAgain = { gameViewModel.resetGame() }
)
```

Dialog’s confirm button calls `onPlayAgain`, which maps to `resetGame()` in the ViewModel.

This is the **events flow up** part: UI → callbacks → ViewModel functions.

### Code
```
imports
	import android.app.Activity  
	import androidx.compose.foundation.background  
	import androidx.compose.foundation.layout.Arrangement  
	import androidx.compose.foundation.layout.Column  
	import androidx.compose.foundation.layout.fillMaxWidth  
	import androidx.compose.foundation.layout.padding  
	import androidx.compose.foundation.layout.safeDrawingPadding  
	import androidx.compose.foundation.layout.statusBarsPadding  
	import androidx.compose.foundation.layout.wrapContentHeight  
	import androidx.compose.foundation.rememberScrollState  
	import androidx.compose.foundation.text.KeyboardActions  
	import androidx.compose.foundation.text.KeyboardOptions  
	import androidx.compose.foundation.verticalScroll  
	import androidx.compose.material3.AlertDialog  
	import androidx.compose.material3.Button  
	import androidx.compose.material3.Card  
	import androidx.compose.material3.CardDefaults  
	import androidx.compose.material3.MaterialTheme.colorScheme  
	import androidx.compose.material3.MaterialTheme.shapes  
	import androidx.compose.material3.MaterialTheme.typography  
	import androidx.compose.material3.OutlinedButton  
	import androidx.compose.material3.OutlinedTextField  
	import androidx.compose.material3.Text  
	import androidx.compose.material3.TextButton  
	import androidx.compose.runtime.Composable  
	import androidx.compose.runtime.collectAsState  
	import androidx.compose.runtime.getValue  
	import androidx.compose.ui.Alignment  
	import androidx.compose.ui.Modifier  
	import androidx.compose.ui.draw.clip  
	import androidx.compose.ui.platform.LocalContext  
	import androidx.compose.ui.res.dimensionResource  
	import androidx.compose.ui.res.stringResource  
	import androidx.compose.ui.text.input.ImeAction  
	import androidx.compose.ui.text.style.TextAlign  
	import androidx.compose.ui.tooling.preview.Preview  
	import androidx.compose.ui.unit.dp  
	import androidx.compose.ui.unit.sp  
	import androidx.lifecycle.viewmodel.compose.viewModel  
	import com.example.unscramble.R  
	import com.example.unscramble.ui.theme.UnscrambleTheme  
  
@Composable  
fun GameScreen(  
    gameViewModel: GameViewModel = viewModel(),  
) {  
    val gameUiState by gameViewModel.uiState.collectAsState()  
    val mediumPadding = dimensionResource(R.dimen.padding_medium)  
  
    Column(  
        modifier = Modifier  
            .statusBarsPadding()  
            .verticalScroll(rememberScrollState())  
            .safeDrawingPadding()  
            .padding(mediumPadding),  
        verticalArrangement = Arrangement.Center,  
        horizontalAlignment = Alignment.CenterHorizontally  
    ) {  
  
        Text(  
            text = stringResource(R.string.app_name),  
            style = typography.titleLarge,  
        )  
        GameLayout(  
            onUserGuessChanged = { gameViewModel.updateUserGuess(it) },  
            userGuess = gameViewModel.userGuess,  
            onKeyboardDone = { gameViewModel.checkUserGuess() },  
            currentScrambledWord = gameUiState.currentScrambledWord,  
            isGuessWrong = gameUiState.isGuessedWordWrong,  
            wordCount = gameUiState.currentWordCount,  
            modifier = Modifier  
                .fillMaxWidth()  
                .wrapContentHeight()  
                .padding(mediumPadding)  
        )  
        Column(  
            modifier = Modifier  
                .fillMaxWidth()  
                .padding(mediumPadding),  
            verticalArrangement = Arrangement.spacedBy(mediumPadding),  
            horizontalAlignment = Alignment.CenterHorizontally  
        ) {  
  
            Button(  
                modifier = Modifier.fillMaxWidth(),  
                onClick = { gameViewModel.checkUserGuess() }  
            ) {  
                Text(  
                    text = stringResource(R.string.submit),  
                    fontSize = 16.sp  
                )  
            }  
  
            OutlinedButton(  
                onClick = { gameViewModel.skipWord() },  
                modifier = Modifier.fillMaxWidth()  
            ) {  
                Text(  
                    text = stringResource(R.string.skip),  
                    fontSize = 16.sp  
                )  
            }  
        }  
        GameStatus(score = gameUiState.score, modifier = Modifier.padding(20.dp))  
  
    }  
    if (gameUiState.isGameOver) {  
        FinalScoreDialog(  
            score = gameUiState.score,  
            onPlayAgain = { gameViewModel.resetGame() }  
        )  
    }  
}  
  
@Composable  
fun GameStatus(score: Int, modifier: Modifier = Modifier) {  
    Card(  
        modifier = modifier  
    ) {  
        Text(  
            text = stringResource(R.string.score, score),  
            style = typography.headlineMedium,  
            modifier = Modifier.padding(8.dp)  
        )  
    }  
}  
  
@Composable  
fun GameLayout(  
    onUserGuessChanged: (String) -> Unit,  
    isGuessWrong: Boolean,  
    userGuess: String,  
    onKeyboardDone: () -> Unit,  
    currentScrambledWord: String,  
    modifier: Modifier = Modifier,  
    wordCount: Int,  
) {  
    val mediumPadding = dimensionResource(R.dimen.padding_medium)  
  
    Card(  
        modifier = modifier,  
        elevation = CardDefaults.cardElevation(defaultElevation = 5.dp)  
    ) {  
        Column(  
            verticalArrangement = Arrangement.spacedBy(mediumPadding),  
            horizontalAlignment = Alignment.CenterHorizontally,  
            modifier = Modifier.padding(mediumPadding)  
        ) {  
            Text(  
                modifier = Modifier  
                    .clip(shapes.medium)  
                    .background(colorScheme.surfaceTint)  
                    .padding(horizontal = 10.dp, vertical = 4.dp)  
                    .align(alignment = Alignment.End),  
                text = stringResource(R.string.word_count, wordCount),  
                style = typography.titleMedium,  
                color = colorScheme.onPrimary  
            )  
            Text(  
                text = currentScrambledWord,  
                style = typography.displayMedium  
            )  
            Text(  
                text = stringResource(R.string.instructions),  
                textAlign = TextAlign.Center,  
                style = typography.titleMedium  
            )  
            OutlinedTextField(  
                value = userGuess,  
                singleLine = true,  
                modifier = Modifier.fillMaxWidth(),  
                onValueChange = onUserGuessChanged,  
                label = {  
                    if (isGuessWrong) {  
                        Text(stringResource(R.string.wrong_guess))  
                    } else {  
                        Text(stringResource(R.string.enter_your_word))  
                    }  
                },  
                isError = isGuessWrong,  
                keyboardOptions = KeyboardOptions.Default.copy(  
                    imeAction = ImeAction.Done  
                ),  
                keyboardActions = KeyboardActions(  
                    onDone = { onKeyboardDone() }  
                ),  
  
                )  
        }  
    }}  
  
/*  
 * Creates and shows an AlertDialog with final score. */@Composable  
private fun FinalScoreDialog(  
    score: Int,  
    onPlayAgain: () -> Unit,  
    modifier: Modifier = Modifier,  
) {  
    val activity = (LocalContext.current as Activity)  
  
    AlertDialog(  
        onDismissRequest = {  
            // Dismiss the dialog when the user clicks outside the dialog or on the back  
            // button. If you want to disable that functionality, simply use an empty            // onCloseRequest.        },  
        title = { Text(text = stringResource(R.string.congratulations)) },  
        text = { Text(text = stringResource(R.string.you_scored, score)) },  
        modifier = modifier,  
        dismissButton = {  
            TextButton(  
                onClick = {  
                    activity.finish()  
                }  
            ) {  
                Text(text = stringResource(R.string.exit))  
            }  
        },  
        confirmButton = {  
            TextButton(onClick = onPlayAgain) {  
                Text(text = stringResource(R.string.play_again))  
            }  
        }    )  
}  
  
@Preview(showBackground = true)  
@Composable  
fun GameScreenPreview() {  
    UnscrambleTheme {  
        GameScreen()  
    }  
}
```


## GameUiState.kt

`GameUiState` contains what the screen needs to show:

- `currentScrambledWord`
- `currentWordCount`
- `score`
- `isGuessedWordWrong`
- `isGameOver`

```
package com.example.unscramble.ui

data class GameUiState(
    val currentScrambledWord: String = "",
    val currentWordCount: Int = 1,
    val score: Int = 0,
    val isGuessedWordWrong: Boolean = false,
    val isGameOver: Boolean = false,
)

```


## GameViewModel.kt

The ViewModel here is the **single source of truth** for the game, the UI **reads state** from it, and the UI **sends events** to it, which the ViewModel handles by updating state. This is classic MVVM with unidirectional data flow.

### What state the ViewModel owns

In `GameViewModel` the game data lives in two kinds of state:

- **UI state (`GameUiState` via StateFlow)**
- **ViewModel-only state** (not exposed directly to UI)
	-  `userGuess`: what the user typed in the text field. It is observable (`mutableStateOf`) so Compose recomposes when it changes, but only the ViewModel can set it (`private set`).
	- `currentWord` and `usedWords`: internal game logic (which word is the correct answer, which words are already used). The UI never sees these directly.
	
The `init { resetGame() }` block initializes state when the ViewModel is created, picking the first scrambled word and putting it into `_uiState`.

### How the ViewModel handles events and updates state (events → state)

Each ViewModel method represents an event handler that updates internal state and/or `_uiState`.

`checkUserGuess()`
```
fun checkUserGuess() {
    if (userGuess.equals(currentWord, ignoreCase = true)) {
        val updatedScore = _uiState.value.score.plus(SCORE_INCREASE)
        updateGameState(updatedScore)
    } else {
        _uiState.update { currentState ->
            currentState.copy(isGuessedWordWrong = true)
        }
    }
    updateUserGuess("") // clear input
}
```

- If the guess matches `currentWord` (case-insensitive), it calculates a new score and calls `updateGameState(updatedScore)`.
- If wrong, it sets `isGuessedWordWrong = true` via `_uiState.update { it.copy(...) }`. This makes the text field show an error label and red error state because `GameLayout` reads `isGuessWrong`.
- Then it clears `userGuess` so the text field becomes empty.

`updateGameState(updatedScore: Int)`
```
private fun updateGameState(updatedScore: Int) {
    if (usedWords.size == MAX_NO_OF_WORDS){
        _uiState.update { currentState ->
            currentState.copy(
                isGuessedWordWrong = false,
                score = updatedScore,
                isGameOver = true
            )
        }
    } else{
        _uiState.update { currentState ->
            currentState.copy(
                isGuessedWordWrong = false,
                currentScrambledWord = pickRandomWordAndShuffle(),
                currentWordCount = currentState.currentWordCount.inc(),
                score = updatedScore
            )
        }
    }
}
```

- Checks if the game has reached the maximum number of words (`usedWords.size == MAX_NO_OF_WORDS`).
- If yes:
    - Sets `isGameOver = true` and updates `score`.
    - `GameScreen` sees `isGameOver == true` and shows `FinalScoreDialog`.
- If no:
    - Picks a new scrambled word with `pickRandomWordAndShuffle()`, increments `currentWordCount`, updates `score`, and clears `isGuessedWordWrong`.
    - `GameScreen` and `GameLayout` recompose with the new word and updated count and score.


### Code
```
class GameViewModel : ViewModel() {

    var userGuess by mutableStateOf("")
        private set
    private lateinit var currentWord: String
    private var usedWords: MutableSet<String> = mutableSetOf()
    private val _uiState = MutableStateFlow(GameUiState())
    val uiState: StateFlow<GameUiState> = _uiState.asStateFlow()


    private fun pickRandomWordAndShuffle(): String {
        // Continue picking up a new random word until you get one that hasn't been used before
        currentWord = allWords.random()
        if (usedWords.contains(currentWord)) {
            return pickRandomWordAndShuffle()
        } else {
            usedWords.add(currentWord)
            return shuffleCurrentWord(currentWord)
        }
    }

    private fun shuffleCurrentWord(word: String): String {
        val tempWord = word.toCharArray()
        // Scramble the word
        tempWord.shuffle()
        while (String(tempWord).equals(word)) {
            tempWord.shuffle()
        }
        return String(tempWord)
    }

    fun updateUserGuess(guessedWord: String) {
        userGuess = guessedWord
    }

    fun checkUserGuess() {
        if (userGuess.equals(currentWord, ignoreCase = true)) {
            // User's guess is correct, increase the score
            // and call updateGameState() to prepare the game for next round
            val updatedScore = _uiState.value.score.plus(SCORE_INCREASE)
            updateGameState(updatedScore)
        } else {
            // User's guess is wrong, show an error
            _uiState.update { currentState ->
                currentState.copy(isGuessedWordWrong = true)
            }
        }
        // Reset user guess
        updateUserGuess("")
    }

    private fun updateGameState(updatedScore: Int) {
        if (usedWords.size == MAX_NO_OF_WORDS){
            //Last round in the game, update isGameOver to true, don't pick a new word
            _uiState.update { currentState ->
                currentState.copy(
                    isGuessedWordWrong = false,
                    score = updatedScore,
                    isGameOver = true
                )
            }
        } else{
            // Normal round in the game
            _uiState.update { currentState ->
                currentState.copy(
                    isGuessedWordWrong = false,
                    currentScrambledWord = pickRandomWordAndShuffle(),
                    currentWordCount = currentState.currentWordCount.inc(),
                    score = updatedScore
                )
            }
        }
    }

    fun skipWord() {
        updateGameState(_uiState.value.score)
        // Reset user guess
        updateUserGuess("")
    }

    fun resetGame() {
        usedWords.clear()
        _uiState.value = GameUiState(currentScrambledWord = pickRandomWordAndShuffle())
    }

    init {
        resetGame()
    }

}
```


## MainActivity.kt
```
package com.example.unscramble

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.activity.enableEdgeToEdge
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.material3.Surface
import androidx.compose.ui.Modifier
import com.example.unscramble.ui.GameScreen
import com.example.unscramble.ui.theme.UnscrambleTheme

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        enableEdgeToEdge()
        super.onCreate(savedInstanceState)
        setContent {
            UnscrambleTheme {
                Surface(
                    modifier = Modifier.fillMaxSize(),
                ) {
                    GameScreen()
                }
            }
        }
    }
}

```


## Overall data & event flow summary

Putting it all together:

- **State lives in ViewModel**:
    - UI state: `uiState: StateFlow<GameUiState>`
    - Local input: `userGuess`
- **[[User Interface|UI]] observes state**:
    - `collectAsState()` for `uiState`
    - direct read of `userGuess`
- **User interacts**:
    - Types → `updateUserGuess()`
    - Presses Done / Submit → `checkUserGuess()`
    - Presses Skip → `skipWord()`
    - Presses Play again → `resetGame()`
- **ViewModel updates state**:
    - Internal logic (`currentWord`, `usedWords`, `pickRandomWordAndShuffle()`)
    - `_uiState.update { it.copy(...) }` to produce new `GameUiState`
- **[[Jetpack Compose|Compose]] recomposes** because the observed state (`uiState`, `userGuess`) changed, so the UI always reflects the latest game state.