# ObjectScript Quick Reference
A list of some common ObjectScript expressions. **Note:** In order to distinguish actual text (things like "do", "set", "##class") from placeholder text (package names, class names, method names, variable names, object references, etc.) this document uses the convention of appending the text "Name" (packageName, className, methodName, varName, orefName, etc.) where necessary.

## Object/SQL Basics

|      Action                                  |   Code                                                                            |
|----------------------------------------------|-----------------------------------------------------------------------------------|
| Call a class method                          | `Do ##class(packageName.className).methodName(arguments)`<br>`Set varName = ##class(packageName.className).methodName(arguments)`<br>Note: place a . before each pass-by-reference argument |
| Call an instance method on an object         | `Do orefName.methodName(arguments)`<br>`Set varName = orefName.methodName(arguments)`<br>Note: place a . before each pass-by-reference argument |
| Create a new object                          | `Set orefName = ##class(packageName.className).%New()`                            |
| Open an existing object                      | `Set orefName = ##class(packageName.className).%OpenId(id, concurrency, .status)` |
| Open an existing object by unique index value| `Set orefName = ##class(packageName.className).IndexNameOpen(value, concurrency, .status)`  |
| Save an object                               | `Set status = orefName.%Save()`                                                   |
| Retrieve the ID of a saved object            | `Set id = orefName.%Id()`                                                         |
| Retrieve the OID of a saved object           | `Set oid = orefName.%Oid()`                                                       |
| Retrieve property of a saved object          | `Set varName = ##class(packageName.className).propertyNameGetStored(id)`          |
| Determine if an object was modified          | `Set varName = orefName.%IsModified()`                                            |
| Validate an object without saving            | `Set status = orefName.%ValidateObject()`                                         |
| Validate a property without saving           | `Set status = ##class(packageName.className).propertyNameIsValid(orefName.propertyName)`|
| Print status after error                     | `Do $system.Status.DisplayError(status)`<br>`Write $system.Status.GetErrorText(status)` |
| Obtain status details after error            | `Do $system.Status.DecomposeStatus(status, .err)`                                 |
| Convert status into exception object         | `Set exception = ##class(%Exception.StatusException).CreateFromStatus(status)`    |
| Remove an object from process memory         | `Set orefName = ""`                                                               |
| Delete an existing object of a class         | `Set status = ##class(packageName.className).%DeleteId(id)`                       |
| Delete an existing object by unique index value| `Set status = ##class(packageName.className).IndexNameDelete(value)`            |
| Delete all saved objects of a class          | `Do ##class(packageName.className).%DeleteExtent()`<br>`Do ##class(packageName.className).%KillExtent()` |
| Reload properties of a saved object          | `Do orefName.%Reload()`                                                           |
| Clone an object                              | `Set orefClonedName = orefName.%ConstructClone()`                                 |
| Write a property                             | `Write orefName.propertyName`                                                     |
| Set a property                               | `Set orefName.propertyName = value`                                               |
| Write a class parameter                      | `Write ..#PARAMETER` <br> `Write ##class(packageName.className).#PARAMETER`       |
| Set a serial (embedded) property             | `Set orefName.serialPropertyName.embeddedPropertyName = value`                    |
| Link two objects                             | `Set oref1Name.propertyName = oref2Name`                                          |
| Populate a class                             | `Do ##class(packageName.className).Populate(count, verbose)`                      |
| Remove all objects in process memory         | `Kill`                                                                            |
| List all objects in process memory           | `Do $system.OBJ.ShowObjects()`                                                    |
| Display all properties of an object          | `Do $system.OBJ.Dump(orefName)`<br>`Zwrite orefName` (v2012.2+)                   |
| Determine If variable is an object reference | `If $isobject(varName) ; 1=yes, 0=no`                                             |
| Find classname of an object                  | `Write $classname(orefName)`                                                      |
| Start the SQL shell                          | `Do $system.SQL.Shell()`                                                          |
| Check SQL privileges                         | `Do $system.SQL.CheckPriv()`                                                      |
| Test a class query                           | `Do ##class(%ResultSet).RunQuery(className, queryName)`                           |
| Declare a variable's type for IDE code completion  | `#dim orefName as packageName.className`                                    |

## ObjectScript Commands

| Command                       | Description                                                                         |
|-------------------------------|-------------------------------------------------------------------------------------|
| `Write`                       | Display text strings, value of variable or expression                               |
| `Zwrite`                      | Display array, list string, bit string                                              |
| `Set`                         | Set value of variable                                                               |
| `Do`                          | Execute method, procedure, or routine                                               |
| `Quit` or `Return` (v2013.1)  | Terminate method, procedure, or routine. Optionally return value to calling method  |
| `Continue`                    | Stop current loop iteration, and continue looping                                   |
| `Halt`                        | Stop process and close Terminal                                               |
| `Kill`                        | Destroy variable(s)                                                                 |
| `If {} ElseIf {} Else {}`     | Evaluate conditions and branch                                                      |
| `For {}`, `While {}`, `Do {} While` | Execute block of code repeatedly                                              |
| `Try {} Catch {}`, `Throw`    | Handle errors                                                                       |

## ObjectScript Date/Time Functions and Special Variables

| Action                                           | Code                                                        |
|--------------------------------------------------|-------------------------------------------------------------|
| Date conversion (external → internal)                | `Set varName = $zdh("mm/dd/yyyy")`                      |
| Date conversion (internal → external)                | `Set varName = $zd(internalDate, format)`               |
| Time conversion (external → internal)                | `Set varName = $zth("hh:mm:ss")`                        |
| Time conversion (internal → external)                | `Set varName = $zt(internalTime, format)`               |
| Display current internal date/time string            | `Write $horolog`                                        |
| Display UTC date/time string                         | `Write $ztimestamp`                                     |
| Get the date relative to other date (any type)       | `Write $system.SQL.DATEADD("hh", -4, "yyyy-mm-dd hh:mm:ss")` |
| Get the difference between two dates (any type)      | `Write $system.SQL.DATEDIFF("hh", horolog-or-ztimestamp, horolog-or-ztimestamp)`|

## ObjectScript Branching Functions

| Action                                           | Code                                                                   |
|--------------------------------------------------|------------------------------------------------------------------------|
| Display result for value of expression           | `Write $case(expression, value1:result1, value2:result2, …, :resultN)` |
| Display result for first true condition          | `Write $select(condition1:result1, condition2:result2, …, 1:resultN)`  |

## ObjectScript String Functions

| Action                                                   | Code                                                          |
|----------------------------------------------------------|---------------------------------------------------------------|
| Display substring extracted from string                  | `Write $extract(string, start, end)`                          |
| Display right-justified string within width characters   | `Write $justify(string, width)`                               |
| Display length of string                                 | `Write $length(string)`                                       |
| Display number of delimited pieces in string             | `Write $length(string, delimiter)`                            |
| Display piece from delimited string                      | `Write $piece(string, delimiter, pieceNumber)`                |
| Set piece into delimited string                          | `Set $piece(string, delimiter, pieceNumber) = piece`          |
| Display string after replacing substring                 | `Write $replace(string, subString, replaceString)`            |
| Display reversed string                                  | `Write $reverse(string)`                                      |
| Display string after replacing characters                | `Write $translate(string, searchChars, replaceChars)`         |
| Build a list                                             | `Set listString = $listbuild(list items, separated by comma)` |
| Retrieve an item from a list                             | `Set listItem = $list(listString, position)`                  |
| Retrieves elements sequentially from a list              | `Set pointerToNextElement = 0`<br> `While = $ListNext(listString, pointerToNextElement, value) {}` |
| Put item into list string                                | `Set $list(listString, position) = substring`                 |
| Display the length of a list                             | `Write $listlength(listString)`                               |
| Search a value in a list                                 | `Write $listfind(listString, value)`                          |
| Build a list from a string                               | `Set listString = $listFromString(string, delimiter)`         |

## ObjectScript Existence Functions

| Action                                           | Code                                                        |
|--------------------------------------------------|-------------------------------------------------------------|
| Check if variable exists                              | `Write $data(varName)`                                 |
| Return value of variable, or default If undefined     | `Write $get(varName, defaultVarName)`                  |
| Return next valid subscript in array                  | `Write $order(array(subscript))`                       |

## Additional ObjectScript Functions

| Action                                           | Code                                                                |
|--------------------------------------------------|---------------------------------------------------------------------|
| Increment ^global by 1 (or by increment)         | `$increment(^globalName, increment)`<br>`$sequence(^globalName)`    |
| Match a regular expression                       | `Set matches = $match(string, regularexpression)`                   |
| Display random integer from start to start+(count-1) | `Write $random(count) + start`                                  |

## ObjectScript Special Variables

| Action                            | Code                                  |
|-----------------------------------|---------------------------------------|
| Display process ID                | `Write $job`                          |
| Display current namespace         | `Write $namespace`                    |
| Change current namespace          | `Set $namespace = newnamespace`       |
| Display username                  | `Write $username`                     |
| Display roles                     | `Write $roles`                        |

## Utilities

| Action                            | Code                                  |
|-----------------------------------|---------------------------------------|
| Change current namespace          | `Do ^%CD` <br> `zn "newnamespace"`    |
| Display a ^global                 | `Do ^%G` <br> `zwrite ^globalName     |

## Collections

| Action                                                       | Code                                                                                                       |
|--------------------------------------------------------------|------------------------------------------------------------------------------------------------------------|
| Create a new standalone list<br>Work with a list property    | `Set orefListName=##class(%ListOfDataTypes).%New()`<br>Use methods below on a list collection property       |
| Insert an element at the end of a list                       | `Do orefListName.Insert(value)`<br>`Do orefName.listPropertyName.Insert(value)`                                    |
| Insert an element into a list                                | `Do orefListName.SetAt(value, position)`<br>`Do orefName.listPropertyName.SetAt(value, position)`                  |
| Remove an element from a list                                | `Do orefListName.RemoveAt(position)`<br>`Do orefName.listPropertyName.RemoveAt(position)`                          |
| Display an element of a list                                 | `Write orefListName.GetAt(position)`<br>`Write orefName.listPropertyName.GetAt(position)`                          |
| Display the size of a list                                   | `Write orefListName.Count()`<br>`Write orefName.listPropertyName.Count()`                                          |
| Clear all the elements of a list                             | `Do orefListName.Clear()`<br>`Do orefName.listPropertyName.Clear()`                                                |
| Create a new standalone array<br>Work with an array property | `Set orefArrayName=##class(%ArrayOfDataTypes).%New()`<br>Use methods below on an array collection property   |
| Insert an element into an array                              | `Do orefArrayName.SetAt(value, key)`<br>`Do orefName.arrayPropertyName.SetAt(value, key)`                          |
| Remove an element from an array                              | `Do orefArrayName.RemoveAt(key)`<br>`Do orefName.arrayPropertyName.RemoveAt(key)`                                  |
| Display an element of an array                               | `Write orefArrayName.GetAt(key)`<br>`Do orefName.arrayPropertyName.GetAt(key)`                                     |
| Display the size of an array                                 | `Write orefArrayName.Count()`<br>`Do orefName.arrayPropertyName.Count()`                                           |
| Clear all elements of an array                               | `Do orefArrayName.Clear()`<br>`Do orefName.arrayPropertyName.Clear()`                                              |

## Relationships

| Action                                  | Code                                                                                                         |
|-----------------------------------------|--------------------------------------------------------------------------------------------------------------|
| Parent-to-children object linking       | `Do orefParentName.childPropertyName.Insert(orefChildName)`<br>`Set orefChildName.parentPropertyName = orefParentName` |
| One-to-many object linking              | `Do orefOneName.manyPropertyName.Insert(orefManyName)`<br>`Set orefManyName.onePropertyName = orefOneName`             |
| Write a property of a child object      | `Write orefParentName.childPropertyName.GetAt(position).propertyName`                                        |
| Write a property of a many object       | `Write orefOneName.manyPropertyName.GetAt(position).propertyName`                                            |
| Display the count of child/many objects | `Write orefParentName.childPropertyName.Count()`<br>`Write orefOneName.manyPropertyName.Count()`             |
| Open a many/child object directly       | `Set orefName = ##class(packageName.className).IDKEYOpen(parentID, childsub)`                                |
| Retrieve the id of a child object       | `Set status = ##class(packageName.className).IDKEYExists(parentID, childsub, .childID)`                      |
| Clear the child/many objects            | `Do orefParentName.childPropertyName.Clear()<br>Do orefOneName.manyPropertyName.Clear()`                     |

## Streams

| Action                                    | Code                                                                                  |
|-------------------------------------------|---------------------------------------------------------------------------------------|
| Create a new stream                       | `Set orefStreamName=##class(%Stream.GlobalCharacter).%New()`<br>`Set streamObject=##class(%Stream.GlobalBinary).%New()`<br>Use methods below on a stream property |
| Add text to a stream                      | `Do orefStreamName.Write(text)`<br>`Do orefName.streamPropertyName.Write(text)`               |
| Add a line of text to a stream            | `Do orefStreamName.WriteLine(text)`<br>`Do orefName.streamPropertyName.WriteLine(text)`       |
| Read len characters of text from a stream | `Write orefStreamName.Read(len)`<br>`Write orefName.streamPropertyName.Read(len)`             |
| Read a line of text from a stream         | `Write orefStreamName.ReadLine(len)`<br>`Write orefName.streamPropertyName.ReadLine(len)`     |
| Go to the beginning of a stream           | `Do orefStreamName.Rewind()`<br>`Do orefName.streamPropertyName.Rewind()`                     |
| Go to the end of a stream, for appending  | `Do orefStreamName.MoveToEnd()`<br>`Do orefName.streamPropertyName.MoveToEnd()`               |
| Clear a stream                            | `Do orefStreamName.Clear()`<br>`Do orefName.streamPropertyName.Clear()`                       |
| Display the length of a stream            | `Write orefStreamName.Size`<br>`Write orefName.streamPropertyName.Size`                       |

## Unit Testing Macros

| Action                      | Code                                             |
|-----------------------------|--------------------------------------------------|
| Assert equality             | `Do $$$AssertEquals(value1, value2, message)`    |
| Assert inequality           | `Do $$$AssertNotEquals(value1, value2, message)` |
| Assert status is OK         | `Do $$$AssertStatusOK(status, message)`          |
| Assert status isn't OK      | `Do $$$AssertStatusNotOK(status, message)`       |
| Assert condition is true    | `Do $$$AssertTrue(condition, message)`           |
| Assert condition isn't true | `Do $$$AssertNotTrue(condition, message)`        |
| Log message                 | `Do $$$LogMessage(message)`                      |

## Other Macros

| Action                           | Code                                      |
|----------------------------------|-------------------------------------------|
| Return a good status             | `Quit $$$OK`                              |
| Return an error status           | `Quit $$$ERROR($$$GeneralError, message)` |
| Throw exception on error         | `$$$ThrowOnError(status, code)` or `$$$TOE(status, code)` |
| Check if status is good          | `If $$$ISOK(status)`                      |
| Check if status is an error      | `If $$$ISERR(status)`                     |
| Return a null object reference   | `Quit $$$NULLOREF`                        |
| Place a new line within a string | `Write string1_$$$NL_string2`             |
