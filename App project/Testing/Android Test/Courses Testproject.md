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


Reworked after next lesson

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

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun CoursesTopAppBar(modifier: Modifier = Modifier) {
    CenterAlignedTopAppBar(
        title = {
            Row(
                verticalAlignment = Alignment.CenterVertically
            ) {
                Image(
                    modifier = Modifier
                        .size(dimensionResource(id = R.dimen.image_size))
                        .padding(dimensionResource(id = R.dimen.padding_small)),
                    painter = painterResource(R.drawable.ic_launcher_foreground),
                    contentDescription = null
                )
                Text(
                    text = stringResource(R.string.app_name),
                    style = MaterialTheme.typography.displayLarge
                )
            }
        },
        colors = topAppBarColors(
            containerColor = MaterialTheme.colorScheme.primaryContainer,
            titleContentColor =  MaterialTheme.colorScheme.primary
        ),
        modifier = modifier
    )
}

@Composable
fun TopicItem(topic: Topic, modifier: Modifier = Modifier) {
    Card {
        Row(
            verticalAlignment = Alignment.Top,
            horizontalArrangement = Arrangement.SpaceBetween
        ) {
            Image(
                painter = painterResource(topic.picture),
                contentDescription = stringResource(topic.title),
                modifier = modifier
                    .padding(dimensionResource(R.dimen.padding_small))
                    .clip(MaterialTheme.shapes.small)
                    .size(dimensionResource(R.dimen.image_size))
            )
            Column(
                modifier = modifier
                    .fillMaxSize()
                    .padding(top = 16.dp, start = 8.dp, end = 8.dp),
                verticalArrangement = Arrangement.SpaceBetween,
                horizontalAlignment = Alignment.Start
            ) {
                Text(
                    text = stringResource(topic.title),
                    fontSize = 12.sp,
                    style = MaterialTheme.typography.displayMedium
                )
                Row(
                    modifier = modifier.padding(top = 8.dp),
                    verticalAlignment = Alignment.CenterVertically
                ) {
                    Icon(
                        painter = painterResource(R.drawable.ic_grain),
                        contentDescription = "grain",
                        tint = Color.Black
                    )
                    Text(
                        modifier = modifier.padding(start = 8.dp),
                        text = topic.numberOfCourses.toString(),
                        fontSize = 12.sp,
                        style = MaterialTheme.typography.bodyLarge,
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
    val groupedTopics = topicList.groupBy { stringResource(it.title)[0].uppercaseChar() }
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
    Scaffold(
        topBar = {
            CoursesTopAppBar()
        }
    ) { innerPadding ->
        Column(
            modifier = Modifier
                .padding(innerPadding)
        ) {
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
}


@Preview(showBackground = true)
@Composable
fun GreetingPreview() {
    CoursesTheme {
        CoursesApp(
            topicList = DataSource().loadTopics(),
            modifier = Modifier
        )
    }
}
```