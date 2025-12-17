```
package com.example.lemonadeclicker

	import android.os.Bundle
	import androidx.activity.ComponentActivity
	import androidx.activity.compose.setContent
	import androidx.activity.enableEdgeToEdge
	import androidx.compose.foundation.BorderStroke
	import androidx.compose.foundation.Image
	import androidx.compose.foundation.background
	import androidx.compose.foundation.border
	import androidx.compose.foundation.layout.Arrangement
	import androidx.compose.foundation.layout.Box
	import androidx.compose.foundation.layout.Column
	import androidx.compose.foundation.layout.PaddingValues
	import androidx.compose.foundation.layout.Row
	import androidx.compose.foundation.layout.Spacer
	import androidx.compose.foundation.layout.fillMaxSize
	import androidx.compose.foundation.layout.fillMaxWidth
	import androidx.compose.foundation.layout.height
	import androidx.compose.foundation.layout.padding
	import androidx.compose.foundation.layout.size
	import androidx.compose.foundation.layout.wrapContentSize
	import androidx.compose.foundation.shape.CircleShape
	import androidx.compose.foundation.shape.RoundedCornerShape
	import androidx.compose.material3.Button
	import androidx.compose.material3.ButtonDefaults
	import androidx.compose.material3.Scaffold
	import androidx.compose.material3.Text
	import androidx.compose.runtime.Composable
	import androidx.compose.runtime.getValue
	import androidx.compose.runtime.mutableStateOf
	import androidx.compose.runtime.remember
	import androidx.compose.runtime.setValue
	import androidx.compose.ui.Alignment
	import androidx.compose.ui.Modifier
	import androidx.compose.ui.graphics.Color
	import androidx.compose.ui.graphics.Shape
	import androidx.compose.ui.graphics.painter.Painter
	import androidx.compose.ui.res.painterResource
	import androidx.compose.ui.res.stringResource
	import androidx.compose.ui.text.font.FontWeight
	import androidx.compose.ui.text.style.LineHeightStyle
	import androidx.compose.ui.text.style.TextAlign
	import androidx.compose.ui.tooling.preview.Preview
	import androidx.compose.ui.unit.dp
	import androidx.compose.ui.unit.sp
	import com.example.lemonadeclicker.ui.theme.LemonadeClickerTheme

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContent {
            LemonadeClickerTheme {
                LemonApp()
            }
        }
    }
}

@Preview(showBackground = true)
@Composable
fun LemonApp(modifier: Modifier = Modifier){


    Box(modifier
        .fillMaxSize()
    ) {
        Banner(modifier.align(Alignment.TopCenter))
        PictureAndText(
            modifier.align(Alignment.Center)
        )
    }
}

@Composable
fun Banner(modifier: Modifier = Modifier){
    Row(modifier
        .height(50.dp)
        .background(Color.Yellow),
        verticalAlignment = Alignment.CenterVertically
    ) {
        Text(
            stringResource(R.string.banner),
            modifier.fillMaxWidth(),
            textAlign = TextAlign.Center,
            fontSize = 20.sp,
            fontWeight = FontWeight.Bold
        )
    }
}

@Composable
fun PictureAndText(modifier: Modifier){

    var current: UInt by remember { mutableStateOf(1u) }

    var imageResource = when (current) {
        1u -> R.drawable.lemon_tree
        2u -> R.drawable.lemon_squeeze
        3u -> R.drawable.lemon_drink
        else -> R.drawable.lemon_restart
    }

    var stringDescriptionResource = when (current) {
        1u -> R.string.lemontree
        2u -> R.string.lemon
        3u -> R.string.glass
        else -> R.string.emptyglass
    }

    var stringResource = when (current) {
        1u -> R.string.taptree
        2u -> R.string.tapsqueeze
        3u -> R.string.tapdrink
        else -> R.string.tapempty
    }


    Column(
        modifier,
        horizontalAlignment = Alignment.CenterHorizontally
    ) {
        var count: UInt = (2u..5u).random()
        var currentCount: UInt = 0u
        Button(
            onClick = {
                when (current) {
                    1u -> current++
                    2u -> {
                        currentCount++
                        if(currentCount == count){
                            current++
                        }

                    }
                    3u -> current++
                    else -> current = 1u
                }
                      },
            [[shape]] = RoundedCornerShape(16.dp),
            contentPadding = PaddingValues(0.dp)
        ) {
        Image(
            painter = painterResource(imageResource),
            contentDescription = stringResource(stringDescriptionResource),
            modifier
                .background(
                    Color.Gray,
                    shape = RoundedCornerShape(16.dp))
                .size(200.dp)
        )}
        Spacer(modifier = Modifier.height(20.dp))
        Text(
            stringResource(stringResource),
            fontSize = 18.sp
        )
    }
}


```