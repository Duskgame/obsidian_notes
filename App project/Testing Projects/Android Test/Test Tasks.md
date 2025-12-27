https://developer.android.com/codelabs/basic-android-kotlin-compose-practice-classes-and-collections?continue=https%3A%2F%2Fdeveloper.android.com%2Fcourses%2Fpathways%2Fandroid-basics-compose-unit-3-pathway-1%23codelab-https%3A%2F%2Fdeveloper.android.com%2Fcodelabs%2Fbasic-android-kotlin-compose-practice-classes-and-collections#1

Task 1 Solution

```
data class Event(  
    val title: String,  
    val description: String? = null,  
    val dayPart: String,  
    val durationInMinutes: Int  
)
```


Task 2 Solution

```
data class Event(  
    val title: String,  
    val description: String? = null,  
    val dayPart: DayPart,  
    val durationInMinutes: Int  
)  
  
enum class DayPart {  
    MORNING,  
    AFTERNOON,  
    EVENING;  
}
```

Task 3 Solution

```
val events = mutableListOf<Event>(event1, event2, event3, event4, event5, event6)
```

Task 4 Solution

```
val shortEvents = events.filter { it.durationInMinutes < 60 }
println("You have ${shortEvents.size} short events.")
```

Task 5 Solution

```
val groupedEvents = events.groupBy { it.daypart }
groupedEvents.forEach { (daypart, events) ->
    println("$daypart: ${events.size} events")
}

- `forEach` goes through each entry in the map one by one.
    
- Each entry is a `Map.Entry<Daypart, List<Event>>`.
    
- `(daypart, events)` is destructuring the entry:
    
    - `daypart` is the map key.
        
    - `events` is the list (the map value).
        
- Inside the lambda, `println` prints the daypart and how many events are in its list (`events.size`).

```

Task 6 Solution

```
println("Last event of the day: ${events.last().title}")
```


Task 7 Solution

```
val Event.durationOfEvent: String  
    get() = "${if (this.durationInMinutes < 60) "short" else "long"}"
```