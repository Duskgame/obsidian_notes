https://www.json.org/json-de.html

JSON is built on two structures:

- A collection of name/value pairs. In various languages, this is realized as an _object_, record, struct, dictionary, hash table, keyed list, or associative [[Arrays|array]].
- An ordered list of values. In most languages, this is realized as an _array_, vector, list, or sequence.

These are universal data structures. Virtually all modern programming languages support them in one form or another. It makes sense that a data format that is interchangeable with programming languages also be based on these structures.

In JSON, they take on these forms:

An _object_ is an unordered set of name/value pairs. An [[Kotlin Object]] begins with {left brace and ends with }right brace. Each name is followed by :colon and the name/value pairs are separated by ,comma.

![[image-19.png]]

The structure of a JSON response has the following features:

- JSON response is an array, indicated by the square brackets. The array contains JSON objects.
- JSON objects are surrounded by curly brackets.
- Each JSON [[Kotlin Object]] contains a set of key-value pairs separated by a comma.
- A colon separates the key and value in a pair.
- Names are surrounded by quotes.
- Values can be numbers, strings, a boolean, an array, an object (JSON object), or [[null]].