# Lecture #1
## Rules 

`<conclusion> :- <condition>`

`<conclusion>` - predicat

Just `<conclusion>` - fact

**Example:**

- `Parent("Victoria", "Edward")` - *определяет таблицу col0 = "Victoria", сol1 - "Edward"*
- `NumLegs("cat", 4)`

### Notes

 - **Если что объявлено, переопределить нельзя, только дополнить!**


## CodeStyle

 - **Predicates** - CamelCase (*e.g CloseRelative*)
 - **variables** - snake_case (*e.g will_smith*)

## Value Types

- **Numbers**  (*e.g 1, 5, 10, 3.14*)

- **String** (*e.g "Word"*)

- **Bool** (*e.g True, False*)

- **List** (*e.g [el1, el2, ..]*) *Elements have general type*
- **Records** (*e.g Position: {lat: 10, lon: 20}*)
### Notes
- `ShipLat(name:, lat: position.lat) :- Ship(name:, position:)`
- `ShipRecord(r: x) :- Ship(..x)` *returns table row how records*
	- *Does not work in SQLite engine (only BigQuery)*
- **Centaur(x) :- x in ["Chiron", ..] ~ Centaur("Chiron")**

## Operators
### " , " - logical AND

**Example:**
- `GrandParent(x, z) :- Parent(x, y), Parent(y, z)`

### "|" - logical OR

**Example:**
- `CloseRelative(x, y) :- Parent(x, y) | Parent(y, x) | (Parent(z, x), Parent(z, y))`

### NOT (To be c..)

### Basic operators

 - «**-**»
 - «**+**»
 - «**\***»
 - «**/ "**»
 - «**%**» - *Remainder*
 - «**^**» - *Raise to power*
 - «**++**» -  *String concatination* 
 - «**==**»  - *Equality, equating*

**Example:**
```
    CubeInfo(cube_id, volume, mass) :- 
    	MetalCube(cube_id, side, density),
    	volume == side * side * side,
    	mass == volume * densite
```
 ### Notes
 - **Fact: No assignments, only check**
 - **Хороший тон расставить скобочки в условие!!!**
 - **Пока всё коммутативно**
 - `CloseRelative(x, y) :- Parent(x, y) | Parent(y, x) | (Parent(z, x), Parent(z, y))` **equivalent to**
`CloseRelative(x, y) :- Parent(x, y)`
`CloseRelative(x, y) :- Parent(y, x)`
`CloseRelative(x, y) :- (Parent(z, x), Parent(z, y))`
 - **Predicate - only table or conventionally table**

  

## Multiset

### Definition
```
						    +--------+ 
							|  col0  |
							+--------+
    Fruit("apple")  		| apple  |
    Fruit("orange") 		| orange |
    Fruit("apple")  		| apple  |
							+--------+
```
### Intersection
```
    Q("a", "b")
    Q("a", "b") 					+-------+-------+
    Q("x", "y") 					| col1  | col2  |
								    +-------+-------+
    R("b", "t") 					|   a   |   t   |
    R("b", "t") 					|   a   |   t   |
    R("b", "t") 					|   a   |   t   |
								    |   a   |   t   |
    P(x, z) :- Q(x, y), R(y, z) 	|   a   |   t   |
								    |   a   |   t   |
									+-------+-------+
```
### **Example:**

- `C(x) :- A(x) | B(x)` - *Sum*
- `C(x) :- A(x), B(x)` -  *Intersection*
- `OurFruit(x) distinct :- MyFruit(x) | YourFruit(x)` - Only different


## More columns
 - **Set** specific columns
	```
	Human(name: "Tony", iq: 1000000)
	Human(name: "Will", iq: 0)
	```
 - **Get** specific columns

	```
	Predicate(column_name:  <expression>, …) or 
	Predicate(column_name:,) or 
	Predicate(column_name:)
	```
### Notes

 - **Set**: arguments order is important
 - **Get**: arguments order is not important

  

## Aggregation

 **Example**

 - ```
	 PhillCount(city:, count? += 1) destinct :- 
		 Human(location: city, iq:), iq > 200 
	```
	- *"destinct" - requerment*
	- *Count Human with iq > 200*
### Notes
- **Aggeregation have functions - [Sum, List etc]**

## ArgMax and -> operator

  

***Не использовать***
```
SmartestPerson(city:, person? ArgMax={arg: name, value: iq}) distinct :-
Human(name:, city:, iq:)
``` 
***Использовать***
```
SmartestPerson(city:, person? ArgMax= name -> iq )distinct :-
Human(name:, city:, iq:)
```
**Example**

 - **Array= iq -> name** - *names sorted by iq*