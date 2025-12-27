```
@Composable
fun HeroCard(hero: Hero, modifier: Modifier = Modifier) {
    Card(modifier = modifier
        .fillMaxWidth()) {
        Row(modifier = modifier
            .background(MaterialTheme.colorScheme.primaryContainer)
            .fillMaxWidth()
            .padding(16.dp),
            horizontalArrangement = Arrangement.SpaceBetween) {
            Column(modifier = modifier
                .padding(end = 16.dp)) {
                Text(
                    style = MaterialTheme.typography.displaySmall,
                    text = stringResource(hero.nameRes)
                )
                Text(
                    modifier = modifier.fillMaxWidth(0.75f),
                    style = MaterialTheme.typography.bodyLarge,
                    text = stringResource(hero.descriptionRes)
                )
            }
            Image(
                modifier = modifier
                    .clip(MaterialTheme.shapes.small)
                    .size(dimensionResource(R.dimen.image_size)),
                painter = painterResource(hero.imageRes),
                contentDescription = null,
            )
        }
    }
}

@Composable
fun HeroList(heroList: List<Hero>, modifier: Modifier = Modifier) {
    LazyColumn(
        modifier = modifier,
        verticalArrangement = Arrangement.spacedBy(8.dp),
        contentPadding = PaddingValues(16.dp)
    ) {
        items(heroList) { hero ->
            HeroCard(hero = hero)
        }
    }
}

@Preview(showBackground = true)
@Composable
fun HeroPreview() {
    HeroListTheme {
        HeroList(HeroesRepository.heroes)
    }
}
```

```
package com.example.herolist

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.activity.enableEdgeToEdge
import androidx.compose.foundation.layout.Row
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.foundation.layout.padding
import androidx.compose.material3.CenterAlignedTopAppBar
import androidx.compose.material3.ExperimentalMaterial3Api
import androidx.compose.material3.MaterialTheme
import androidx.compose.material3.Scaffold
import androidx.compose.material3.Text
import androidx.compose.material3.TopAppBarDefaults.topAppBarColors
import androidx.compose.runtime.Composable
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.res.stringResource
import androidx.compose.ui.tooling.preview.Preview
import com.example.herolist.data.HeroesRepository
import com.example.herolist.model.Hero
import com.example.herolist.ui.theme.HeroListTheme

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContent {
            HeroListTheme {
                Scaffold(modifier = Modifier.fillMaxSize()) { innerPadding ->
                    Greeting(
                        name = "Android",
                        modifier = Modifier.padding(innerPadding)
                    )
                }
            }
        }
    }
}

@Composable
fun Greeting(name: String, modifier: Modifier = Modifier) {
    Text(
        text = "Hello $name!",
        modifier = modifier
    )
}

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun HeroesTopAppBar(modifier: Modifier = Modifier) {
    CenterAlignedTopAppBar(
        title = {
            Row(
                verticalAlignment = Alignment.CenterVertically
            ) {
                Text(
                    text = stringResource(R.string.app_name),
                    style = MaterialTheme.typography.displayLarge
                )
            }
        },
        colors = topAppBarColors(
            containerColor = MaterialTheme.colorScheme.primaryContainer,
            titleContentColor = MaterialTheme.colorScheme.primary
        ),
        modifier = modifier
    )
}

@Composable
fun HeroesApp(heroList: List<Hero>, modifier: Modifier = Modifier) {
    Scaffold(
        topBar = {
            HeroesTopAppBar()
        }
    ) { innerPadding ->
        HeroList(heroList, modifier = modifier.padding(innerPadding))
    }
}

@Preview(showBackground = true)
@Composable
fun GreetingPreview() {
    HeroListTheme {
        HeroesApp(HeroesRepository.heroes)
    }
}
```