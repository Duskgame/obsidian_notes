```
import android.os.Build
import androidx.compose.animation.animateContentSize
import androidx.compose.animation.core.Spring
import androidx.compose.animation.core.spring
import androidx.compose.foundation.Image
import androidx.compose.foundation.background
import androidx.compose.foundation.clickable
import androidx.compose.foundation.layout.Arrangement
import androidx.compose.foundation.layout.Column
import androidx.compose.foundation.layout.Row
import androidx.compose.foundation.layout.height
import androidx.compose.foundation.layout.padding
import androidx.compose.material3.Card
import androidx.compose.material3.MaterialTheme
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.compose.runtime.getValue
import androidx.compose.runtime.mutableStateOf
import androidx.compose.runtime.remember
import androidx.compose.runtime.setValue
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.draw.clip
import androidx.compose.ui.layout.ContentScale
import androidx.compose.ui.res.painterResource
import androidx.compose.ui.res.stringResource
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp
import com.example.monthofkotlin.data.TipRepository
import com.example.monthofkotlin.model.Tip
import com.example.monthofkotlin.ui.theme.MonthOfKotlinTheme
import java.time.LocalDate
import java.time.YearMonth


@Composable
fun TipDate(tip: Tip, modifier: Modifier) {
    Text(
        text =
            if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O)
                "${
                    if (tip.dayNr < LocalDate.now().dayOfMonth) {
                        LocalDate.now()
                            .minusDays(LocalDate.now().dayOfMonth.toLong())
                            .plusDays(YearMonth.now().lengthOfMonth().toLong())
                            .plusDays(tip.dayNr)
                    } else {
                        LocalDate.now()
                            .minusDays(LocalDate.now().dayOfMonth.toLong())
                            .plusDays(tip.dayNr)
                    }
                }"
            else "Day ${tip.dayNr + 1}",
        style = MaterialTheme.typography.displaySmall,
        fontSize = 12.sp,
        lineHeight = 22.sp,
        color = MaterialTheme.colorScheme.onTertiaryContainer
    )
}

@Composable
fun TipCard(tip: Tip, modifier: Modifier = Modifier) {
    var expanded by remember { mutableStateOf(false) }
    Card {
        Column(
            modifier = modifier
                .background(MaterialTheme.colorScheme.tertiaryContainer)
                .padding(8.dp)
                .animateContentSize(
                    animationSpec = spring(
                        dampingRatio = Spring.DampingRatioMediumBouncy,
                        stiffness = Spring.StiffnessLow
                    )
                ),
            verticalArrangement = Arrangement.spacedBy(8.dp)
        ) {
            Row(
                modifier = modifier
                    .clickable(
                        interactionSource = null,
                        indication = null,
                        onClick = {
                            expanded = !expanded
                        }
                    ),
                horizontalArrangement = Arrangement.spacedBy(8.dp),
                verticalAlignment = Alignment.Top
            ) {
                Column(
                    modifier = modifier
                        .clip(MaterialTheme.shapes.small)
                        .weight(1f)
                        .padding(8.dp)
                ) {
                    TipDate(
                        tip = tip,
                        modifier = modifier
                    )
                    Text(
                        text = stringResource(tip.titleRes),
                        style = MaterialTheme.typography.displaySmall,
                        fontSize = 16.sp,
                        lineHeight = 18.sp,
                        color = MaterialTheme.colorScheme.onTertiaryContainer
                    )
                }
                Image(
                    modifier = modifier
                        .height(74.dp)
                        .clip(MaterialTheme.shapes.small),
                    painter = painterResource(tip.imageRes),
                    contentDescription = null,
                    contentScale = ContentScale.FillHeight
                )
            }
            if (expanded) {
                Text(
                    text = stringResource(tip.textRes),
                    color = MaterialTheme.colorScheme.onTertiaryContainer,
                )
            }
        }
    }
}


@Preview
@Composable
fun TipCardPreview() {
    MonthOfKotlinTheme() {
        TipCard(TipRepository.Tips[4])
    }
}

@Preview
@Composable
fun TipoListPreview() {
    MonthOfKotlinTheme() {
        TipList(TipRepository.Tips)
    }
}

```

```
import android.os.Build
import androidx.compose.foundation.layout.Arrangement
import androidx.compose.foundation.layout.PaddingValues
import androidx.compose.foundation.layout.padding
import androidx.compose.foundation.lazy.LazyColumn
import androidx.compose.foundation.lazy.items
import androidx.compose.runtime.Composable
import androidx.compose.ui.Modifier
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp
import com.example.monthofkotlin.data.TipRepository
import com.example.monthofkotlin.model.Tip
import com.example.monthofkotlin.ui.theme.MonthOfKotlinTheme
import java.time.LocalDate
import java.time.YearMonth


fun sortListToDate(tipList: List<Tip>): List<Tip> {
    val sortedTips = tipList.sortedBy { it.dayNr }
    val newList: List<Tip> =
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
            listOf(
                tipList.slice(LocalDate.now().dayOfMonth..YearMonth.now().lengthOfMonth() -1),
                tipList.slice(0..LocalDate.now().dayOfMonth - 1)
            ).flatten()
        } else {
            sortedTips
        }
    return newList
}

@Composable
fun TipList(tipList: List<Tip>, modifier: Modifier = Modifier) {
    LazyColumn(
        modifier = modifier.padding(8.dp),
        verticalArrangement = Arrangement.spacedBy(8.dp),
        contentPadding = PaddingValues(8.dp)
    ) {
        items(sortListToDate(tipList)) { tip ->
            TipCard(tip = tip)
        }
    }
}

@Preview
@Composable
fun TipListPreview() {
    MonthOfKotlinTheme() {
        TipList(TipRepository.Tips)
    }
}
```

```
import androidx.compose.foundation.Image
import androidx.compose.foundation.background
import androidx.compose.foundation.layout.Arrangement
import androidx.compose.foundation.layout.Row
import androidx.compose.foundation.layout.fillMaxWidth
import androidx.compose.foundation.layout.padding
import androidx.compose.foundation.layout.size
import androidx.compose.material3.CenterAlignedTopAppBar
import androidx.compose.material3.ExperimentalMaterial3Api
import androidx.compose.material3.MaterialTheme
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.res.dimensionResource
import androidx.compose.ui.res.painterResource
import androidx.compose.ui.res.stringResource
import androidx.compose.ui.unit.sp
import com.example.monthofkotlin.R

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun TipTopAppBar(modifier: Modifier = Modifier) {
    CenterAlignedTopAppBar(
        title = {
            Row(
                modifier = modifier
                    .fillMaxWidth()
                    .background(MaterialTheme.colorScheme.onTertiaryContainer),
                horizontalArrangement = Arrangement.Center,
                verticalAlignment = Alignment.CenterVertically
            ) {
                Image(
                    modifier = Modifier
                        .size(dimensionResource(R.dimen.image_size))
                        .padding(dimensionResource(R.dimen.padding_small)),
                    painter = painterResource(R.drawable.ic_launcher_foreground),

                    // Content Description is not needed here - image is decorative, and setting a
                    // null content description allows accessibility services to skip this element
                    // during navigation.

                    contentDescription = null
                )
                Text(
                    text = stringResource(R.string.app_name),
                    style = MaterialTheme.typography.displayLarge,
                    fontSize = 20.sp,
                    color = MaterialTheme.colorScheme.inversePrimary
                )
            }
        },
        modifier = modifier.fillMaxWidth()
    )
}
```

```
package com.example.monthofkotlin

import android.os.Build
import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.activity.enableEdgeToEdge
import androidx.annotation.RequiresApi
import androidx.compose.foundation.layout.padding
import androidx.compose.material3.Scaffold
import androidx.compose.runtime.Composable
import androidx.compose.ui.Modifier
import androidx.compose.ui.tooling.preview.Preview
import com.example.monthofkotlin.components.TipList
import com.example.monthofkotlin.components.TipTopAppBar
import com.example.monthofkotlin.data.TipRepository
import com.example.monthofkotlin.ui.theme.MonthOfKotlinTheme

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContent {
            MonthOfKotlinTheme {
                TipApp()
            }
        }
    }
}


@Composable
fun TipApp() {
    Scaffold(
        topBar = {
            TipTopAppBar()
        }
    ) { it ->
        TipList(TipRepository.Tips, modifier = Modifier.padding(it))
    }
}

@RequiresApi(Build.VERSION_CODES.O)
@Preview(showBackground = true)
@Composable
fun GreetingPreview() {
    MonthOfKotlinTheme {
        TipApp()
    }
}
```