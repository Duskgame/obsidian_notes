```
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContent {
            CoursesTheme {
                Surface(
                    modifier = Modifier
                        .fillMaxSize(),
                    color = MaterialTheme.colorScheme.background
                ) {
                    CoursesApp(DataSource().loadTopics(), modifier = Modifier)
                }
            }
        }
    }
}

@Composable
fun TopicItem(topic: Topic, modifier: Modifier = Modifier) {
    Card() {
        Row {
            Image(
                painter = painterResource(topic.picture),
                contentDescription = stringResource(topic.title),
                modifier = modifier.size(68.dp)
            )
            Column(
                modifier = modifier.padding(top = 16.dp, start = 16.dp, end = 16.dp)
            ) {
                Text(
                    text = stringResource(topic.title),
                    fontSize = 12.sp
                )
                Row {
                    Icon(
                        painter = painterResource(R.drawable.ic_grain),
                        contentDescription = "grain",
                        tint = Color.Black
                    )
                    Text(
                        modifier = modifier.padding(start = 8.dp),
                        text = topic.numberOfCourses.toString(),
                        fontSize = 12.sp
                    )
                }

            }
        }
    }
}

@Composable
fun ListOfCourses(topicList: List<Topic>, modifier: Modifier = Modifier) {

    LazyVerticalGrid(
        modifier = modifier,
        columns = GridCells.Fixed(2),
        verticalArrangement = Arrangement.spacedBy(8.dp),
        horizontalArrangement = Arrangement.spacedBy(8.dp),
        contentPadding = PaddingValues(8.dp)

    ) {
        items(topicList) { topic ->
            TopicItem(
                topic = topic,
                modifier = modifier
            )

        }
    }
}


@Composable
fun GroupByFirstCharacter(topicList: List<Topic>, modifier: Modifier) {
    val groupedTopics = topicList.groupBy { stringResource(it.title)[0] }
    groupedTopics.forEach {
        println("(${it.value.size}) ${it.key}: ${it.value}")
    }
}

@Composable
fun CharacterGroupDropdown(
    topicList: List<Topic>,
    onButtonClicked: (List<Topic>) -> Unit,
    modifier: Modifier = Modifier
) {
    var expanded by remember { mutableStateOf(false) }
    val groupedTopics = topicList.groupBy { stringResource(it.title)[0].uppercaseChar()  }
    Box {
        Button(onClick = {
            expanded = true
        }) {
            Text("A/B/C")
        }
        DropdownMenu(
            expanded = expanded,
            onDismissRequest = { expanded = false }
        ) {
            groupedTopics.forEach {
                DropdownMenuItem(
                    text = { Text("${it.key} (${it.value.size})") },
                    onClick = {
                        onButtonClicked(it.value)
                        expanded = false
                    }
                )
            }
            DropdownMenuItem(
                text = { Text("All") },
                onClick = {
                    onButtonClicked(topicList)
                    expanded = false
                }
            )
        }
    }
}


@Composable
fun CoursesApp(topicList: List<Topic>, modifier: Modifier) {
    var sortedTopicList: List<Topic> by remember { mutableStateOf(topicList) }

    Column(modifier = Modifier.padding(top = 50.dp)) {
        Row(
            modifier = modifier
                .fillMaxWidth()
                .padding(8.dp),
            horizontalArrangement = Arrangement.End,
        ) {
            var sortedByNumber by remember { mutableStateOf(true) }
            var sortedByTitle by remember { mutableStateOf(true) }
            CharacterGroupDropdown(
                topicList = topicList,
                onButtonClicked = { sortedTopicList = it },
                modifier = modifier
            )
            Button({
                sortedTopicList = if (sortedByNumber) {
                    sortedTopicList.sortedBy { it.numberOfCourses }
                } else {
                    sortedTopicList.sortedBy { it.numberOfCourses }.asReversed()
                }
                sortedByTitle = true
                sortedByNumber = !sortedByNumber
            }, modifier = modifier.padding(start = 8.dp, end = 4.dp)) {
                Text("Number")
            }
            Button({
                sortedTopicList = if (sortedByTitle) {
                    sortedTopicList.sortedBy { it.title }
                } else {
                    sortedTopicList.sortedBy { it.title }.asReversed()
                }
                sortedByNumber = true
                sortedByTitle = !sortedByTitle
            }, modifier = modifier.padding(start = 4.dp)) {
                Text("Title")
            }
        }
        ListOfCourses(sortedTopicList, modifier)
    }

}


@Preview(showBackground = true)
@Composable
fun GreetingPreview() {
    CoursesTheme {
        CoursesApp(
            topicList = DataSource().loadTopics(),
            modifier = Modifier
        )
        GroupByFirstCharacter(DataSource().loadTopics(), modifier = Modifier)
    }

```