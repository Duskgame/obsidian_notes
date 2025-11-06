"Unit" just stands for "something that has only one value", it's a traditional name, comes from functional languages.

The purpose is the same as C's or Java's `void`. Only Unit is a proper [[Data Type]], so it can be passed as a generic argument etc.

1. Why we don't call it "Void": because the word "void" means "nothing", and there's another type, `Nothing`, that means just "no value at all", i.e. the computation did not complete normally (looped forever or threw an exception). We could not afford the clash of meanings.
    
2. Why Unit has a value (i.e. is not the same as Nothing): because generic code can work smoothly then. If you pass Unit for a generic [[Parameter]] T, the code written for any T will expect an object, and there must be an object, the sole value of Unit.
    
3. How to access that value of Unit: since it's a singleton object, just say `Unit`