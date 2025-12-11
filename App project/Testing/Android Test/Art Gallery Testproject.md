```

data class ArtPiece(  
    @DrawableRes val pictureId: Int,  
    @StringRes val pictureTitleId: Int,  
    @StringRes val pictureArtistId: Int,  
    @StringRes val pictureYearId: Int,  
    @StringRes val pictureInfoId: Int  
)  
  
val artGallery: Array<ArtPiece> = arrayOf(  
    ArtPiece(  
        pictureId = R.drawable.van_gogh___starry_night___google_art_project,  
        pictureTitleId = R.string.sternennacht,  
        pictureArtistId = R.string.sternennacht_artist,  
        pictureYearId = R.string.sternennacht_year,  
        pictureInfoId = R.string.sternennacht_info  
    ),  
    ArtPiece(  
        pictureId = R.drawable._665_girl_with_a_pearl_earring,  
        pictureTitleId = R.string.perlohrring,  
        pictureArtistId = R.string.perlohrring_artist,  
        pictureYearId = R.string.perlohrring_year,  
        pictureInfoId = R.string.perlohrring_info  
    ),  
    ArtPiece(  
        pictureId = R.drawable.mona_lisa,  
        pictureTitleId = R.string.mona,  
        pictureArtistId = R.string.mona_artist,  
        pictureYearId = R.string.mona_year,  
        pictureInfoId = R.string.mona_info  
    )  
)  
  
class MainActivity : ComponentActivity() {  
    @OptIn(ExperimentalMaterial3Api::class)  
    override fun onCreate(savedInstanceState: Bundle?) {  
        super.onCreate(savedInstanceState)  
        enableEdgeToEdge()  
        setContent {  
            GalleryTheme {  
                Surface {  
                    GalleryView()  
                }  
            }        }    }  
}  
  
  
@Composable  
fun DisplayArtwork(artworkArrayIndex: Int, modifier: Modifier) {  
    Surface(  
        modifier = modifier,  
        shape = RectangleShape,  
        color = MaterialTheme.colorScheme.onSurfaceVariant,  
        shadowElevation = 5.dp  
    ) {  
        Image(  
            painter = painterResource(artGallery[artworkArrayIndex].pictureId),  
            contentDescription = stringResource(artGallery[artworkArrayIndex].pictureTitleId),  
            modifier = modifier  
                .padding(20.dp)  
        )  
    }  
}  
  
@OptIn(ExperimentalMaterial3Api::class)  
@Composable  
fun DisplayInfoTooltip(  
    currentIndex: Int,  
    modifier: Modifier = Modifier,  
    richTooltipSubheadText: String = stringResource(artGallery[currentIndex].pictureTitleId),  
    richTooltipText: String = stringResource(artGallery[currentIndex].pictureInfoId)  
) {  
    TooltipBox(  
        modifier = modifier,  
        positionProvider = TooltipDefaults.rememberRichTooltipPositionProvider(),  
        tooltip = {  
            RichTooltip(  
                title = { Text(richTooltipSubheadText) }  
            ) {  
                Text(richTooltipText)  
            }  
        },  
        state = rememberTooltipState()  
    ) {  
        DisplayInfo(  
            currentIndex,  
            modifier  
        )  
    }  
}  
  
@Composable  
fun DisplayInfo(artworkArrayIndex: Int, modifier: Modifier) {  
  
    Surface(  
        modifier = modifier,  
        shape = RectangleShape,  
        color = MaterialTheme.colorScheme.surfaceContainerLow,  
        tonalElevation = 20.dp,  
        shadowElevation = 5.dp,  
    ) {  
        Column(  
            modifier = modifier.padding(10.dp),  
            horizontalAlignment = Alignment.CenterHorizontally  
        ) {  
            Text(  
                fontWeight = FontWeight.Bold,  
                fontSize = 20.sp,  
                text = stringResource(artGallery[artworkArrayIndex].pictureTitleId),  
                modifier = modifier  
            )  
            Text(  
                stringResource(artGallery[artworkArrayIndex].pictureArtistId) + " " +  
                        stringResource(artGallery[artworkArrayIndex].pictureYearId)  
            )  
        }  
    }}  
  
  
@Composable  
@ExperimentalMaterial3Api  
fun GalleryView(  
    modifier: Modifier = Modifier  
) {  
    var currentIndex by remember { mutableIntStateOf(0) }  
  
    Column(  
        modifier  
            .padding(20.dp)  
            .fillMaxSize()  
            .verticalScroll(rememberScrollState()),  
        verticalArrangement = Arrangement.Center,  
        horizontalAlignment = Alignment.CenterHorizontally  
    ) {  
        DisplayArtwork(  
            currentIndex,  
            modifier  
        )  
  
        Spacer(modifier.height(30.dp))  
  
        DisplayInfoTooltip(  
            currentIndex,  
            modifier  
        )  
  
        Spacer(modifier.height(30.dp))  
        Row(  
            modifier  
                .fillMaxWidth(),  
            horizontalArrangement = Arrangement.SpaceBetween  
        ) {  
            var previousButtonState by remember {  
                mutableStateOf(  
                    currentIndex > 0  
                )  
            }  
            var nextButtonState by remember {  
                mutableStateOf(  
                    currentIndex < artGallery.size - 1  
                )  
            }  
            Button(  
                {  
                    currentIndex--  
                    previousButtonState = currentIndex > 0  
                    nextButtonState = currentIndex < artGallery.size - 1  
                },  
                modifier.width(120.dp),  
                enabled = previousButtonState,  
                colors = ButtonDefaults.buttonColors(  
                    containerColor = MaterialTheme.colorScheme.primary,  
                    contentColor = MaterialTheme.colorScheme.onPrimary,  
                )  
            ) {  
                Text("Previous")  
            }  
            Button(  
                {  
                    currentIndex++  
                    previousButtonState = currentIndex > 0  
                    nextButtonState = currentIndex < artGallery.size - 1  
                },  
                modifier.width(120.dp),  
                enabled = nextButtonState,  
                colors = ButtonDefaults.buttonColors(  
                    containerColor = MaterialTheme.colorScheme.primary,  
                    contentColor = MaterialTheme.colorScheme.onPrimary,  
                )  
            ) {  
                Text("Next")  
            }  
        }    }}  
  
@OptIn(ExperimentalMaterial3Api::class)  
@Preview(showBackground = true)  
@Composable  
fun GreetingPreview() {  
    GalleryTheme {  
        GalleryView()  
    }  
}
```