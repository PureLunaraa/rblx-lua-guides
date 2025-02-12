# Object-Oriented Programming

First we need to understand what exactly is Object-Oriented Programming also known as OOP. 
It is basically an organisation method which relies on different custom made objects, with properties and functions, which makes the code more readable and easier to manage.
Instead of writing long scripts with scattered variables and functions, we create custom objects that represent real-world things, like a player, enemy, or vehicle in a game.

### How Does OOP Work?
The way it works is you create something called a Class which is used to create custom made Objects inside of the game. 
The key things which we will look at this tutorial are:
- Classes
- Objects
- Methods
- Properties
- Inheritance

## Classes

A class is like a blueprint which defines the Object's default behavior. 
For example, in roblox, the ```Instance``` class provides a constructor function called 'new' (```Instance.new()```), which creates new objects like ```Part```, ```Model```, or ```Folder```.
Every class typically has a constructor function, often named 'new', which is responsible for creating an object and assigning its default behavior - functions (Methods) and values (Properties).

### How do we create a Class?

Firstly, you would need to have an understanding of Metatables.. you can check out this [tutorial](./Articles/Metatables.md).
We will need to use ```tables``` and ```metatables``` to construct it. 
Ideally, you would want every different class in a seperate script.

The first thing we will do is create a table and create the constructor function 'new'
```lua
local Class = {}

function Class.new()
   
return

return Class
```

Now that we got that, when we call the constructor function, it needs to construct something.. some type of data.. which we currently don't do. That will be another table so we can store multiple values in it, don't forget to return it!
```lua
function Class.new()
   local object = {}

   return object
return
```

We can assign functions, values to that table, we will do something simple. Let's pretend this is an enemy. I will add a health value and a Damage function
```lua
function Class.new()
   local object = {}
   object.Health = 150

   function object.Damage()
      object.Health -= 25
   function

   return object
return
```

This is really really plain ```Class```. Shall we test it?

First we will need to require the ```Class``` module script. Then we will create a new ```Object``` using it

```lua
local Class = require(path.to.class)

local NewObject = Class.new()
```
Now, that we have our new object, we can print out its health and maybe try dealing damage

```lua
print(NewObject.Health) -- This should Output, in this case, 150
NewObject.Damage() -- This will subtract 25 health
print(NewObject.Health) -- We should be left with 125 health
```
That is really really the basic of OOP.
