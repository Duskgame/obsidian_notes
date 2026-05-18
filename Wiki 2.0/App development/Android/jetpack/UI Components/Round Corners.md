[[Jetpack Compose]]

rounding corner in [[jetpack compose]]

Box(
    modifier = Modifier
        .background(
            color = Color.Blue,
            ==shape = RoundedCornerShape(16.dp)==
        )
        .padding(16.dp)
)

Image(
    painter = painterResource(R.drawable.your_image),
    contentDescription = null,
    modifier = Modifier
        .size(120.dp)
        ==.clip(RoundedCornerShape(16.dp))==
)

Box(
    modifier = Modifier
        ==.clip(==
            ==RoundedCornerShape(==
                ==topStart = 16.dp,==
                ==topEnd = 16.dp,==
                ==bottomStart = 0.dp,==
                ==bottomEnd = 0.dp==
            )
        )
        .background(Color.Green)
        .padding(16.dp)
)

Card/Surface(
    ==[[shape]] = RoundedCornerShape(12.dp),==
    modifier = Modifier.fillMaxWidth()
) {
    Text(
        "Hello",
        modifier = Modifier.padding(16.dp)
    )
}