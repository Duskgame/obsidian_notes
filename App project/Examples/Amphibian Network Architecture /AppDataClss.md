```
@Serializable  
data class AmphibianInfo(  
    val name: String,  
    val type: String,  
    val description: String,  
    @SerialName(value = "img_src")  
    val imageSrc: String  
)
```

`AmphibianInfo` is the **domain model / [[Data Class]]** that directly [[Maps]] to the [[JSON]] response from the Mars server [[API]]. It's the **contract between your network layer and the rest of the app**.

## Role in complete architecture flow
```
Mars Server JSON → Retrofit + kotlinx.serialization → AmphibianInfo
                            ↓
Repository returns List<AmphibianInfo> → ViewModel → UI State → Composables
```

## JSON → Kotlin mapping

The Mars server returns JSON like:
```
{
  "name": "Treefrog",
  "type": "Amphibian", 
  "description": "A small green frog...",
  "img_src": "https://android-kotlin-fun-mars-server.appspot.com/image1.jpg"
}
```

`AmphibianInfo` maps it perfectly:
```
@Serializable
data class AmphibianInfo(
    val name: String,                           // → JSON "name"
    val type: String,                           // → JSON "type"
    val description: String,                    // → JSON "description"
    @SerialName(value = "img_src")              // → JSON "img_src" (snake_case)
    val imageSrc: String                        // Kotlin camelCase
)
```

## Key serialization features

**`@Serializable`**: Tells `kotlinx.[[Serialization]]` to auto-generate JSON encoder/decoder for this class

**`@SerialName("img_src")`**: Bridges API snake_case → [[Kotlin]] camelCase convention

## Data flow through layers
```
1. Retrofit GET /amphibians → JSON array
2. Json.asConverterFactory() → List<AmphibianInfo> 
3. Repository forwards List → ViewModel
4. ViewModel wraps in Success(List) → UI state
5. HomeScreen renders each AmphibianInfo → AmphibianInfoCard
6. AsyncImage loads info.imageSrc URL
```

## Why data class?

 **`data class`**: Auto-generates `copy()`, `equals()`, `toString()`, `hashCode()`  
 **Immutable**: `val` [[Kotlin Class Properties|properties]] → safe for [[Jetpack Compose|Compose]] [[State in Compose|State]]  
 **Single source of truth**: Exact match to API response structure

**This completes the full stack**: Raw JSON → domain model → [[Repository]] → ViewModel → [[UI State]] → screen. Every layer transforms data up one level of [[OOP|abstraction]].