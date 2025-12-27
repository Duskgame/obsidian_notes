```
@Composable
fun TipTimeLayout() {
   var amountInput by remember { mutableStateOf("") }
   val amount = amountInput.toDoubleOrNull() ?: 0.0
   val tip = calculateTip(amount)

   Column(
       modifier = Modifier
            .statusBarsPadding()
            .padding(horizontal = 40.dp)
            .verticalScroll(rememberScrollState())
            .safeDrawingPadding(),
       horizontalAlignment = Alignment.CenterHorizontally,
       verticalArrangement = Arrangement.Center
   ) {
       Text(
           text = stringResource(R.string.calculate_tip),
           modifier = Modifier
               .padding(bottom = 16.dp, top = 40.dp)
               .align(alignment = Alignment.Start)
       )
       EditNumberField(
           value = amountInput,
           onValueChange = { amountInput = it },
           modifier = Modifier
               .padding(bottom = 32.dp)
               .fillMaxWidth()
       )
       Text(
           text = stringResource(R.string.tip_amount, tip),
           style = MaterialTheme.typography.displaySmall
       )
       Spacer(modifier = Modifier.height(150.dp))
   }
}

@Composable
fun EditNumberField(
   value: String,
   onValueChange: (String) -> Unit,
   modifier: Modifier = Modifier
) {
   TextField(
       value = value,
       onValueChange = onValueChange,
       singleLine = true,
       label = { Text(stringResource(R.string.bill_amount)) },
       keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Number),
       modifier = modifier
   )
}

```


```
// Main activity that hosts the Compose UI
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        // Enables drawing edge-to-edge (content can extend behind system bars)
        enableEdgeToEdge()
        // Calls the superclass implementation of onCreate
        super.onCreate(savedInstanceState)
        // Sets the content of this activity using Jetpack Compose
        setContent {
            // Applies the custom theme defined in TipTimeTheme
            TipTimeTheme {
                // Surface provides a background for the app using the theme’s background color
                Surface(
                    modifier = Modifier.fillMaxSize(),
                ) {
                    // Calls the main layout composable for the Tip Time UI
                    TipTimeLayout()
                }
            }
        }
    }
}

// Composable that displays a row with text and a switch for rounding tips
@Composable
fun RoundTheTipRow(
    roundUp: Boolean,                     // current round-up state
    onRoundUpChanged: (Boolean) -> Unit,  // callback when switch is toggled
    modifier: Modifier = Modifier         // optional external styling
) {
    // Creates a horizontal container for the row contents
    Row(
        modifier = modifier
            .fillMaxWidth()               // row takes full width
            .size(48.dp),                 // sets fixed height of 48dp
        verticalAlignment = Alignment.CenterVertically, // vertically center items
        horizontalArrangement = Arrangement.SpaceBetween // space between text and switch
    ) {
        // Label text for the switch
        Text(text = stringResource(R.string.round_up_tip))
        // Toggle switch for rounding the tip
        Switch(
            checked = roundUp,            // current switch state
            onCheckedChange = onRoundUpChanged, // triggers callback when state changes
        )
    }
}

// Composable for an input field with a label and leading icon
@Composable
fun EditNumberField(
    @StringRes label: Int,                       // resource ID for field label
    @DrawableRes leadingIcon: Int,               // resource ID for leading icon
    value: String,                               // current text value
    onValueChange: (String) -> Unit,             // callback when value changes
    keyboardOptions: KeyboardOptions,            // keyboard behavior
    modifier: Modifier = Modifier                // optional external styling
) {
    // Material TextField allows numeric input for amount or tip percentage
    TextField(
        value = value,                           // current input value
        leadingIcon = {                          // composable for the icon on the left
            Icon(painter = painterResource(id = leadingIcon), contentDescription = null)
        },
        onValueChange = onValueChange,           // updates state when user types
        modifier = modifier,                     // styling such as padding/width
        label = { Text(stringResource(label)) }, // field label
        singleLine = true,                       // restricts to a single line
        keyboardOptions = keyboardOptions        // sets keyboard type and IME action
    )
}

// Main layout composable for the app's content
@Composable
fun TipTimeLayout() {
    // Keeps track of the user's bill amount input as state
    var amountInput by remember { mutableStateOf("") }
    // Converts the string input to a Double (or 0.0 if invalid)
    val amount = amountInput.toDoubleOrNull() ?: 0.0

    // Keeps track of the user's tip percentage input
    var tipInput by remember { mutableStateOf("") }
    // Converts tip input to a Double
    val tipPercent = tipInput.toDoubleOrNull() ?: 0.0

    // Keeps track of whether the tip should be rounded up
    var roundUp by remember { mutableStateOf(false) }

    // Calculates the tip based on user input and round-up setting
    val tip = calculateTip(amount, tipPercent, roundUp)

    // Vertical list-style layout for arranging all UI items
    Column(
        modifier = Modifier
            .statusBarsPadding()          // adds padding for status bar area
            .padding(horizontal = 40.dp)  // horizontal spacing for content edges
            .safeDrawingPadding()         // avoids overlapping system areas
            .verticalScroll(rememberScrollState()), // allows scrolling if needed
        horizontalAlignment = Alignment.CenterHorizontally, // centers children horizontally
        verticalArrangement = Arrangement.Center             // centers content vertically
    ) {
        // Header title
        Text(
            text = stringResource(R.string.calculate_tip), // “Calculate Tip” text
            modifier = Modifier
                .padding(bottom = 16.dp, top = 40.dp)      // spacing above and below
                .align(alignment = Alignment.Start)         // align to start of column
        )

        // Input for bill amount
        EditNumberField(
            label = R.string.bill_amount,       // label text resource
            leadingIcon = R.drawable.money,     // money icon
            value = amountInput,                // current input text
            onValueChange = { amountInput = it }, // update state when changed
            keyboardOptions = KeyboardOptions.Default.copy(
                keyboardType = KeyboardType.Number, // number-only keyboard
                imeAction = ImeAction.Next          // jumps to next input on Enter
            ),
            modifier = Modifier
                .padding(bottom = 32.dp)
                .fillMaxWidth()
        )

        // Input for tip percentage
        EditNumberField(
            label = R.string.how_was_the_service, // label text resource
            leadingIcon = R.drawable.percent,      // percent icon
            value = tipInput,                      // current input text
            onValueChange = { tipInput = it },     // updates state
            keyboardOptions = KeyboardOptions.Default.copy(
                keyboardType = KeyboardType.Number, // number keyboard
                imeAction = ImeAction.Done          // closes keyboard on Enter
            ),
            modifier = Modifier
                .padding(bottom = 32.dp)
                .fillMaxWidth()
        )

        // Row for switch toggle
        RoundTheTipRow(
            roundUp = roundUp,                    // current state
            onRoundUpChanged = { roundUp = it },  // updates toggle state
            modifier = Modifier.padding(bottom = 32.dp)
        )

        // Displays the calculated tip result (formatted as currency)
        Text(
            text = stringResource(R.string.tip_amount, tip),
            style = MaterialTheme.typography.displaySmall // large display font
        )

        // Spacer to push content from bottom
        Spacer(modifier = Modifier.height(150.dp))
    }
}

// Helper function that performs the actual tip calculation
private fun calculateTip(
    amount: Double,
    tipPercent: Double = 15.0,
    roundUp: Boolean
): String {
    // Calculates tip as percent of the amount
    var tip = tipPercent / 100 * amount
    // Optionally rounds up to next whole number
    if (roundUp) {
        tip = kotlin.math.ceil(tip)
    }
    // Returns the tip formatted as local currency (ex: "$10.00")
    return NumberFormat.getCurrencyInstance().format(tip)
}

// Preview composable for Android Studio’s design preview window
@Preview(showBackground = true)
@Composable
fun TipTimeLayoutPreview() {
    // Applies theme for preview consistency
    TipTimeTheme {
        TipTimeLayout()
    }
}

```