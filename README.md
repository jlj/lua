# Lua

This repo is a clone of the official Lua repository https://github.com/lua/lua, where I publish my own changes and improvements.

This branch implements…

## Direct enumeration of collections in Lua `for in` loops.

This branch adds support for direct `for..in` enumeration in Lua, for any type of collection: Lua tables and native collections 
implemented as userdata, without having to call `pair()` or `ipair()`, in the for-in statement.

For example, you can now write:

````Lua
for var1, var2, var3 in collection do
    -- do some stuff
end
````

Direct `for..in` enumeration can be customized by defining a `"__forgen"` metamethod that shall return the 3 values needed 
by the generic for loop: an iterator function, a state, and the initial value of the iterator variable.  

If no `"__forgen"` metamethod is defined and the collection is a Lua table, direct for..in enumeration is equivalent to 
calling the standard Lua library function `pair()`.

Internally, a new Lua op-code 'OP_TFORPREP' has been added, that replaces the initial JMP instruction when initializing 
a generic for loop. When executed, this opcode checks for the presence of a "__forgen" metamethod in the loop base object 
and calls it if present.
