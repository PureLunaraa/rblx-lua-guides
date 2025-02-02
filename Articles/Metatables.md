# Metatables

I decided to make this tutorial because I was struggling to understand metatables and their capability for a long time. With this tutorial I want to help people who are getting confused and have a hard time understanding them and their power.

# Introduction to Metatables:
**Metatables** are a powerful feature in Luau, the programming language used in Roblox scripting. A **metatable** is a special Luau table that allows us to modify the behavior of other tables. Every table in Luau can have an associated **metatable**, and metatables work through **metamethods**. **Metamethods** are functions that define the behavior of tables when specific operations are performed on them. Imagine them as events, fired when a proper operation is done. With time you will start experiencing the true benefits of metatables.

# Let's see how to set-up a metatable:

In this example, we create a table **`myTable`** and a separate table **`myMetatable`** . We then associate the metatable **`myMetatable`** with the table **`myTable`** using the **`setmetatable()`** function. **`setmetatable()`** also returns the table with set, in our case **`myTable`**, but we won't look at it for now.

* **Example**

```
local myTable = {}
local myMetatable = {}
setmetatable(myTable, myMetatable)
```

**Note:** The main table in this case is the `myTable` table

# Now, Let's Start Exploring The Metamethods, what they do and what is their purpose.

Metamethods always begin with "__". It is like its own prefix that indicates this "key" is a metamethod.
As I said earlier they are functions that make the table more powerful. You can expand them, combine them as much as you want till you get the wanted result. Imagine them as events for that table.
Just like events, they fire when something triggers them.
You can find them as "hooks". There isn't really something special about them, you just have to remember what they do. Some of them you will use in almost every script, some of them you will never use, but I wanted to provide an explanation of every single metamethod that isn't disabled by Roblox.

We will begin with probably the most used metamethod: **__index**
## __index Metamethod:

The **`__index`** (index) metamethod is called when a key is accessed in a table that does not exist.


* **Purpose**
It allows you to define custom behavior when attempting to access non-existent keys. This is useful for implementing default values for missing keys or creating inheritance-like behavior. Always must return something

* **Parameters**
    * table `table` -> the main table
    * key `any` -> the provided missing key


* **Example**

```
myMetatable.__index = function(table, key)
    return "Key '" .. key .. "' not found!"
end

print(myTable.someKey)  -- Output: Key 'someKey' not found!
```

## ----------------------------

We will continue with the next **metamethod** in the list: **__newindex**

## __newindex Metamethod:

The **`__newindex`** (newindex) metamethod is called when a new key with a value is assigned to the main table.

* **Purpose**
It enables you to control the behavior when setting new keys in the main table. By using this metamethod, you can prevent unwanted additions to the table or enforce specific rules for key-value assignments.


* **Parameters**
    * table `table` -> the main table
    * key `any` -> the new assigned key
    * value `any` -> the value of the key


* **Example**
```
myMetatable.__newindex = function(table, key, value)
    print("You can't set a new value directly!")
end

myTable.newKey = "Hello, World!"  -- Output: You can't set a new value directly!
```
## ----------------------------

Now let's head to the **__tostring** metamethod
## **__tostring Metamethod:**

The **`__tostring`** (tostring) metamethod is called when the main table is converted to a string using `tostring()` or `print()`.

* **Purpose**
It allows you to customize the string representation of the table. This is helpful for displaying more meaningful information when printing or debugging tables. Always must return something

* **Parameters**
  * table `table` -> the main table

* **Example**
```
myMetatable.__tostring = function(table)
    return "The table was converted to a string"
end

print(myTable)  -- Output: The table was converted to a string
```
## ----------------------------

We will move to the **`__call`** metamethod

## __call Metamethod

The **`__call`** (call) metamethod is called when the main table is called as if it was a function.

* **Purpose**
It lets you make the main table behave like a function. This is commonly used to execute specific code when the main table is called, creating callable "function-like" tables.

* **Parameters**
  * table `table` -> the main table
  * ... `any` -> the parameters you provided

* **Example**

```
myMetatable.__call = function(table,  ...) -- in our case the arguments are going to be a number and a string
    print("The table was called with arguments:", ...)
end

myTable(1, "Hello")  -- Output: The table was called with arguments: 1 Hello
```

## ----------------------------

Alright, The next 6 metamethods in my list are really similar: **__add**, **__sub**, **__mul**, **__div**, **__mod**, **__pow**
They work the same but for different operations

## **__add**, **__sub**, **__mul**, **__div**, **__mod**, **__pow** Metamethods
These **metamethods** are called when an arithmetic operation is used on the table.
-> **__add** (add) whenever a value is added to the main table using the **`+`** operator
-> **__sub** (subtract) whenever a value is subtracted from the main table using the **`-`** operator
-> **__mul** (multiply) whenever a value is multiplied to the main table using the **`*`** operator
-> **__div** (divide) whenever a value is divided from the main table using the **`/`** operator
-> **__mod** (mod) whenever on the main table is used the modulus **`%`** operator
-> **__pow** (power) whenever on the main table is used the power **`^`** operator

* **Purpose**
It enables you to implement custom arithmetic behaviors between tables. Must return something

* **Parameters**
   * table `table` -> the main table
   * value `any` -> the value that was used for the arithmetic operation

* **Example**

```
myMetatable.__add = function(table, value)
    return value + 5
end

print(myTable += 10) -- Output: 15
```
```
myMetatable.__sub = function(table, value)
    return value - 5
end

print(myTable -= 10) -- Output: 5
```
```
myMetatable.__mul = function(table, value)
    return value * 5
end

print(myTable *= 10) -- Output: 50
```
```
myMetatable.__div = function(table, value)
    return value / 5
end

print(myTable /= 10) -- Output: 2
```
```
myMetatable.__mod = function(table, value)
    return value + 5
end

print(myTable % 3) -- Output: 8
```
```
myMetatable.__pow = function(table, value)
    return value ^ value
end

print(myTable ^ 2) -- Output: 4
```
## ----------------------------
Another operator: **__unm**

## __unm Metamethod
The **`__unm`** (unary) metamethod is called when it is used the unary **`-`** operator is used on the main table.

* **Purpose**
To provide a way to implement custom unary negation or inversion operations for tables. Always must return something

* **Parameters**
   * table `table` -> the main table

* **Example**

```
myMetatable.__unm = function(table)
    return -5
end

print(-myTable) -- Output: -5
```

## ----------------------------

Now a bit more confusing of a metamethod, at least it was for me: **`__eq`**

## __eq Metamethod
The **`__eq`** (equal) metamethod is called when two tables are compared using the **`==`** or **`~= `** operators.

* **Purpose**
It allows you to implement custom comparison behaviors between tables. Must return something
* **Parameters**
   * table `table` -> the main table
   * table2 `table` -> the other table in the comparison

    **Note:** Both tables should have the same metatable function else it would fail 

* **Example**
```
myMetatable.__eq = function(table, table2)
    return #table == #table2
end

print(myTable == {["RandomKey"] = 4}) -- Output: false. Because our table 'myTable' is empty
print(myTable == {}) -- Output: true. Because they are both empty so they are the same length
```
## ----------------------------

We are almost at the end: **`__lt`**

## __lt Metamethod
The **`__lt`** (less than) metamethod is called when two tables are compared using the **`<`** operator. 

* **Purpose**
It allows you to implement custom less-than comparison behaviors between tables. Must return something

* **Parameters**
   * table `table` -> the main table
   * table2 `table` -> the other table used in the comparison

    **Note:** Both tables should have the same metatable function else it would fail 

* **Example**
```
myMetatable.__lt = function(table, table2)
    return table.apples < table2.apples
end

local tableL = { apples = 2, oranges = 4 }
local tableM = { apples = 6, oranges = 3 }
setmetatable(tableL, myMetatable)
setmetatable(tableM, myMetatable)

print(tableL < tableM)  -- Output: true
```
## ----------------------------

A little more to go, we will check out first: **__le**

## __le Metamethod

The `__le` (less than or equal) metamethod is called when two tables are compared using the **`<=`** operator.

* **Purpose**
It allows you to implement custom less-than-or-equal comparison behaviors between tables.

* **Parameters**
   * table `table` -> the main table
   * table2 `table` -> the other table used in the comparison

    **Note:** Both tables should have the same metatable function else it would fail 

* **Example**
```
myMetatable.__le = function(table, table2)
    return table.apples <= table2.apples
end

local tableL = { apples = 6, oranges = 4 }
local tableM = { apples = 6, oranges = 3 }
setmetatable(tableL, myMetatable)
setmetatable(tableM, myMetatable)

print(tableL <= tableM)  -- Output: true
```
## ----------------------------

Now a really interesting metamethod: **__metatable**

## __metatable Metamethod

The **`__metatable`** metamethod is a special metamethod that allows you to control access to a table's metatable

* **Purpose**
It provides a level of protection and encapsulation by restricting modifications to the metatable, making it read-only for certain operations. When you define the **`__metatable`** metamethod in a metatable, you are essentially specifying what value should be returned when someone tries to access the metatable of the table using **`getmetatable(table)`**.

* **Parameters**
none...

* **Example**
```
myMetatable = {
     someValue = true
}

print(getmetatable(myTable))  -- Output: table...

myMetatable.__metatable = "This is a locked metatable!"
print(getmetatable(myTable))  -- Output: This is a locked metatable!

```
## ----------------------------

One more to the last metamethod, but now: **__len**

## __len Metamethod
The **`__len`** metamethod is called when the `#` (length) operator is applied to a table. It allows you to define custom behavior for calculating the length of a table. Must return something

* **Purpose**
To provide a way to implement a custom length calculation for tables. Must return something

* **Parameters**
   * table `table` -> the main table

* **Example**

```
myMetatable.__len = function(table)
     return #table
end

print(#myTable) -- Output: 0. Because our table `myTable` is empty
```
## -----------Extra------------

Now the most confusing one: **__mode**
If you want, skip this one because you will almost never use it and it is hard to understand

## __mode Metamethod

### To understand this metamethod, we will first need to know what a weak table is:
In Luau, weak tables are tables that do not prevent their keys or values from being garbage collected. When the only reference to a key or value in a weak table is from the weak table itself, and there are no other strong references to them, Luau will remove those key-value pairs from the weak table, allowing the garbage collector to reclaim the memory associated with them.

The **`__mode`** metamethod is a special metamethod used in weak tables in Lua. It is not directly associated with a single table but rather controls the behavior of the table used as a weak key table or a weak value table.

* **Purpose**
The purpose of the **`__mode`** metamethod is to define the mode of a weak table, indicating how the table should treat its keys and values in terms of weak references. The use of weak tables can be very helpful in specific scenarios, such as creating caches or associations where you want to avoid memory leaks and let the garbage collector manage the lifetime of objects.

* **Modes**
   * **"`k`"**: This mode indicates weak keys. When applied to a table, the table's keys will be weak references. If a key is only present in the weak table and is not strongly referenced elsewhere, it will be removed from the table when the garbage collector runs.
   * **`"v"`** : This mode indicates weak values. When applied to a table, the table's values will be weak references. If a value is only present in the weak table and is not strongly referenced elsewhere, its key-value pair will be removed from the table when the garbage collector runs.
   * **`"kv"`** : This mode indicates weak keys and weak values. When applied to a table, both keys and values in the table will be weak references. If a key or value is only present in the weak table and is not strongly referenced elsewhere, its key-value pair will be removed from the table when the garbage collector runs.
   * **`"s"`**: The **`s`** mode I am pretty sure stands for string keys. It means that when a key in the table becomes weakly referenced (there are no strong references to it), the corresponding key-value pair in the table will be automatically removed by the garbage collector.

* **Example**

```
local weakTable = {}
setmetatable(weakTable, { __mode = "k" })  -- Create a weak key table

local key = {}
local value = "Some value"

weakTable[key] = value  -- Add a key-value pair to the weak table

key = nil  -- Set the key to nil to remove the strong reference

-- At this point, the only reference to the key is from the weak table itself.
-- When the garbage collector runs, the key-value pair will be removed from the table.

collectgarbage()  -- Trigger the garbage collector

print(weakTable[key])  -- Output: nil (The key-value pair has been removed)
```

## ----------------------------

## __iter Metamethod

The **`__iter`** metamethod is used to enable a custom iteration over a table using a `for` loop

* **Purpose**
When you use a **`for`** loop on a table, Luau looks for the **`__iter`** metamethod in the table's metatable. If it exists, Luau will use it to perform the iteration, otherwise, it will use the default behavior of iterating over numerical indices starting from 1. Must return a function and an index, and a value argument

* **Parameters**
   * table `table` -> the main table

* **Example**
```
myTable = {}

myMetatable.__iter = function(table)
      return print, 5, 3  -- the `print` function with a key and a value. Then this function will run. Now when it is iterating, it will print 5 and 3 -- Output: 5 3. There are functions like `next` which will return and key, and value
end

for i, v in myTable do

end

myTable = {
    {1, 3, 5, 7, 9}, -- an array with odd numbers
    {2, 4, 6, 8} -- an array with even numbers
} -- now let's iterate over this array

myMetatable.__iter = function(table)
      return next, table 
end

for i, v in myTable do
    print(i, v) -- Output: 1 {1, 3, 5, 7, 9}; 2 {2, 4, 6, 8}
end

-- Now let's use our own function for it.

local function combine(a, b)
	return a + b, a - b -- It will return b added to a and b subtracted from a. If i have an input of like 7 and 3, the output will be: 10 4
end

-- I will have this `combine` function that will return the 2 numbers - a and b, added and subtracted from each other

--Let's change the `myTable` table, let's make it a number array

myTable = {2, 3, 431, 12, 2135, 4124, 19214, 1234}

--Now, let's add the __iter metamethod

myMetatable.__iter = function(table)
		local index = 0
		local maxIndex = #table
		
		return function() -- Adding our own function that will just keep track of the index of the table
			index = index + 1
			if index <= maxIndex then
                              return combine(index, table[index]) -- returning the key and value (the combine function returns them both (we are sending the index and its value to the combine function))
			end
		end
	end
end

for i, v in myTable do
     print(i, v) 
end --[[
   for the first index it will print: 3 -1, (1 (index) and 2 (value) added, 2 subtracted from 1)
   for the second: 5 -1 (2 (index) and 3 (value) added, 3 subtracted from 2)
   for the third: 434 -428 (3 (index) and 431 (value) added, 431 subtracted from 3)
   for the fourth: 16 -8 (4 (index) and 12 (value) added, 12 subtracted from 4)
   for the fifth: 2140 -2130 (5 (index) and 2135 (value) added, 3 subtracted from 2)
   and etc.
]] 

```

That's overall.

## ----------------------------

**Metatables are a powerful feature and should be used judiciously to maintain code readability and avoid unnecessary complexity. When used appropriately, metatables can significantly enhance the functionality and efficiency of your Luau code. I gave a basic of every metamethod but that doesn't mean you can't expand it**

**As you explore and master metatables, you'll unlock the potential to create more advanced and dynamic applications, optimize code, and design robust data structures, contributing to a more enjoyable and efficient Lua programming experience.**

**And remember, the whole power comes when you combine everything together!**

**I wish you to make the best use of metatables**

This was everything. I hope I was helpful and you learnt something new. If I missed something, you can tell. If I made a mistake somewhere, you can tell aswell. 

# Happy coding!
