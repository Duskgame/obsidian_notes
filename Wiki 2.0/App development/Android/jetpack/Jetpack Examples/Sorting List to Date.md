From [[Month of Kotlin Tips]]

```
inside TipCard element
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
```

```
inside TipList element
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
```