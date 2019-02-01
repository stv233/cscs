

CSCS (Customized Scripting in C#) is a scripting language, which is very easy to integrate into any C# code and adjust according to your needs. Basically, it's not only a language, but also a framework that you can use to create your own language. CSCS has been described in:

* [Customized Scripting in C#](https://msdn.microsoft.com/en-us/magazine/mt632273.aspx)  MSDN
* [Programming your own language in C#](http://www.codemag.com/Article/1607081)  CODE Magazine
* [Implementing a Custom Language Succinctly](https://www.syncfusion.com/resources/techportal/details/ebooks/implementing-a-custom-language)  Syncfusion E-book

The usage of CSCS in Mobile App development has been described in:

* [Developing Cross-Platform Native Apps with a Functional Language](http://www.codemag.com/article/1711081)  CODE Magazine
* [Writing Native Mobile Apps Using a Customizable Scripting Language](https://msdn.microsoft.com/en-us/magazine/mt829272)  MSDN
* [Writing Native Mobile Apps in a Functional Language Succinctly](https://www.syncfusion.com/ebooks/writing_native_mobile_apps_in_a_functional_language_succinctly)  Syncfusion E-book

<br>

Decription of CSCS
======

* The syntax is a mixture between C# and Python.
* All statements must finish with a semicolon ";".
* Identation and new lines are not used in parsing.
* All CSCS variables have at least 3 properties that can be accessed using the dot notation: properties, size, and type
  E.g. after setting n=10; n.properties will return {type, size, properties}.
* Variables and arrays are all defined implicitely, e.g. x=5, b[7]=11<br>
  An example of a list initialization: c = {"aa", "bb", "xxx"};<br>
  You can also define it explicitely: c[0]="aa"; c[1]="bb"; <br>
  Definition in index form doesn't have to start from index 0, or even from the first dimension: not defined elements will have a type NONE.
  E.g.: b[5][3][5][3]=15;<br>
  Similarly, when defining dictionaries, e.g.: x["bla"]["blu"]="wichtig";
* Control flow statements if, else, while, for, try, etc., all require statements between the curly braces (even for a single statement).

CSCS Control Flow Functions
------

| **CSCS Statement**                  | **Description**                                     |
| :------------------------------------------- |:------------------------------------------------|
| **include** (*pathToFile*)                    | Includes another scripting file, e.g. include("functions.cscs");   
| **function** *funcName* (*param1*, *param2=value2*, *param3=value3*) { *statements;* } | Declares a custom function with 0 or more parameters. Parameters can optionally have default values. When calling a function, parameters can be specified either implicitely (e.g. sine(10)), or explicitely (e.g. func(param2=value2, param1=value1)).  |
| **cfunction** *funcName* (*param1*, *param2=value2*, *param3=value3*) { *statements;* } | Declares a custom precomplied function with 0 or more parameters. Doesn't work on iOS and Android.  |
| **return** or **return** *variable*;          | Finishes execution of a function and optionally can return a value.            |
| **while** (*condition*) { *statements;* }                                    | Execute loop as long as the condition is true. <br>Curly brackets are mandatory.   |
| **for** (*init*; *condition*; *step*) { *statements;* }  | A canonic for loop, e.g. for (i = 0; i < 10; ++i).<br>Curly brackets are mandatory.          |
| **for** (*item in listOfValues*) { *statements;* }  | Executes loop for each elemеnt of listOfValues.<br>Curly brackets are mandatory. |
| **break**                                    | Breaks out of a loop.                            |
| **continue**                                 | Forces the next iteration of the loop.           |
| **if** (*condition*) { *statements;* } <br> **elif** (*condition*) { *statements;* } <br> **else** { *statements;* } |If-else control flow statements.<br>Curly brackets are mandatory.|
| **try** { *statements;* } <br> **catch**(*exceptionString*)  { *statements;* } | Try and catch control flow.<br>Curly brackets are mandatory.|
| **throw** *string*;                                    | Throws an exception, e.g. throw "value must be positive";    |
| **true**                                   | Represents a boolean value of true. Equivalent to number 1.    |
| **false**                                   | Represents a boolean value of false. Equivalent to number 0.    |

<br>

### Control Flow Example
<pre><code>
include("functions.cscs");

i = 0;
for (i = 0; i < 13; i++) {
  b += (i*4 - 1);
  if ( i == 3) {
    break;
  } else {
    continue;
  }
  print("this is never reached");
}

a = 23; b = 22;
cond = "na";
if (a < b) {
  if (b < 15) {
    cond = "cond1";
  }
  elif  (b < 50) {
    cond = "cond2";
  }
}
elif (a >= 25) {
  cond = "cond3";
}
else {
  cond = "cond4";
}

function myp(par1, par2, par3 = 100) {
  return par1 + par2 + par3;
}

z = myp(par2=20, par1=70); // z = 190

try {
  z = myp(par2=20);
  print("Error. Missing Exception: Function [myp] arguments mismatch: 3 declared, 1 supplied.");
} catch(exc) {
  print("OK. Caught: " + exc);
}
try {
  z = myp(par2=20, par3=70);
  print("Error. Missing Exception: No argument [par1] given for function [myp].");
} catch(exc) {
  print("OK. Caught: " + exc);
}
</code></pre>

<br>

CSCS Object-Oriented Functions and Named Properties
------


| **CSCS Function**                  | **Description**                                     |
| :------------------------------------------- |:------------------------------------------------|
| **class** *className : Class1, Class2, ... { }*     | A definition of a new class. it can optionaly inherit from one or more classes. Inside of a class definition you can have constructors, functions, and variable definitions. You access these variables and functions using the dot notation (all of them are public).|
| **new** *className(param1, param2, ...)*  | Creates and returns an instance (object) of class className. There can be a zero or more parameters passed to the class constructor (depending on the class constructer prameter definitions).|
| *variable*.**Properties** | Returns a list of all properties that this variable implements. For each of these properties is legal to call variable.property. Each variable implements at least the following properties: Size, Type, and Properties. |
| *variable*.**Size** | Returns either a number of elements in an array if variable is of type ARRAY or a number of characters in a string representation of this variable. |
| *variable*.**Type** | Returns this variable's type (e.g. NONE, STRING, NUMBER, ARRAY, OBJECT). |
| **GetProperty** (*objectName, propertyName*)  | Returns variable.propertyName.|
| **GetPropertyStrings** (*objectName*)  | Same as calling variable.properties.|
| **SetProperty** (*objectName, propertyName, propertyValue*)  | Same as variable.propertyName = propertyValue.|


### Object-Oriented Example

<pre><code>
class CoolStuff : Stuff1, Stuff2 {
  z = 3;
  CoolStuff(a, b, c) {
    x = a;
    y = b;
    z = c;
  } 
  function addCoolStuff() {
    return x + y + z;
  }
}

addition = 100;
obj1 = new Stuff1(10);
print(obj1.x); // prints 10
print(obj1.addStuff1(addition); // prints 110

obj2 = new Stuff2(20);
print(obj2.y); // prints 20 
print(obj2.addStuff2(addition)); // prints 120

newObj = new CoolStuff(11, 13, 17);
print(newObj.addCoolStuff()); // prints 41
print(newObj.addStuff1(addition)); // prints 111
print(newObj.addStuff2(addition)); // prints 113
</code></pre>

<br>

CSCS Math Functions
------

| **CSCS Function**                  | **Description**                                     |
| :------------------------------------------- |:------------------------------------------------|
| **Abs** (*value*)                   | Returns absolute value   
| **Acos** (*value*)                  | Returns arccosine function   
| **Asin** (*value*)                  | Returns arcsine function   
| **Ceil** (*value*)                  | Returns the smallest integral value which is greater than or equal to the specified decimal value
| **Cos** (*value*)                   | Cosine function   
| **Exp** (*value*)                   | Returns constant e (2.718281828...) to the power of the specified value   
| **Floor** (*value*)                 | Returns the largest integral value less than or equal to the specified decimal value
| **Log** (*base, power*)             | Returns the natural logarithm of a specified number.
| **Pi**                              | Returns pi constant (3.14159265358979...)
| **Pow** (*base, power*)             | Returns base to the specified power.
| **GetRandom** (*limit, numberOfRandoms=1*)        | If numberOfRandoms = 1, returns a random variable between 0 and limit. Otherwise returns a list of numberOfRandoms integers, where each element is a random number between 0 and limit. Id limit >= numberOfRandoms, each number will be present at most once|
| **Round** (*number, digits=0*)             | Rounds number according to the specified number of digits.
| **Sqrt** (*number*)             | Returns squeared root of the specified number.
| **Sin** (*value*)                   | Sine function   

<br>

CSCS Variable and Array Functions
------


| **CSCS Function**                  | **Description**                                     |
| :------------------------------------------- |:------------------------------------------------|
| **Add**(*variable, value, index = -1*)  | Appends value to the current variable array. If index is greater or equal to zero, inserts it at the index.|
| **AddVariableToHash** (*variable, value, hashKey*)                    | Appends a value to the list of values of a given hash key.|
| **AddAllToHash** (*variable, values, startFrom, hashKey, sep = "\t"*)  | Add all of the values in values list to the hash map variable. E.g. AddAllToHash("categories", lines, startWords, "all");  |
| **Contains** (*variable, value*)    | Checks if the current variable contains another variable. Makes sense only if curent variable is an array.|
| **DeepCopy** (*variable*)    | Makes a deep copy of the passed object, assigning new memory to all of it array members.|
| **DefineLocal** (*variable, value=""*)    | Defines a variable in local scope. Makes sense only if a global variable with this name already exists (without this function, a global variable will be used and modified).|
| **FindIndex** (*variable, value*)    | Looks for the value in the specified variable array and returns its index if found, or -1 otherwise.|
| **GetColumn** (*variable, column, fromRow=0*)    | Goes over all rows of the variable array starting from the specified row and return a specified column.|
| **GetKeys** (*variable*)    |If the underlying variable is a dictionary, returns all the dictionary keys.|
| **Remove** (*variable, value*)    | Removes specified value from the variable array. Returns true on success and false otherwise.|
| **RemoveAt** (*variable, index*)    | Removes a value from the variable array at specified index. Returns true on success and false otherwise.|
| **Size** (*variable*)           | Returns number of elements in a variable array or the length of the string (same as variable.Size). |
| **Type** *(variableName)*             | Returns type of the passed variable (same as variable.Type).|


### Array Example

<pre><code>
a[1]=1; a[2]=2;
c=a[1]+a[2];

a[1][2]=22;
a[5][3]=15;
a[1][2]-=100;
a[5][3]+=100;

print(a[5][2]);

a[1][2]++;
print(a[1][2]);
print(a[5][3]++);
print(++a[5][3]);
print(--a[5][3]);
print(a[5][3]--);

b[5][3][5][3]=15;
print(++b[5][3][5][3]);

x["bla"]["blu"]=113;
x["bla"]["blu"]++;
x["blabla"]["blablu"]=126;
--x["blabla"]["blablu"];
</code></pre>

<br>

CSCS Conversion Functions
------

| **CSCS Function**                  | **Description**                                     |
| :------------------------------------------- |:------------------------------------------------|
| **Bool** (*variable*)  | Converts variable to a Boolean value.|
| **Decimal** (*variable*)  | Converts variable to a decimal value.|
| **Double** (*variable*)  | Converts variable to a double value.|
| **Int** (*variable*)  | Converts variable to an integer value.|
| **String** (*variable*)  | Converts variable to a string value.|

<br>

CSCS String Functions
------


| **CSCS Function**                  | **Description**                                     |
| :------------------------------------------- |:------------------------------------------------|
| **Size** (*variableName*)           | Returns length of the string (for arrays returns number of elemnts in an array). |
| **StrBetween** (*string, from, to*) | Returns a substring with characters between substrings from and to.  
| **StrBetweenAny** (*string, from, to*) | Returns a substring with characters between any of the cgars in from and any of the chars in to.|
| **StrContains** (*string, argument, case=case*) | Returns whether a string contains a specified substring. The case parameter can be either "case" (default) or "nocase. |
| **StrEndsWith** (*string, argument, case=case*) | Returns whether a string ends with a specified substring. |
| **StrEqual** (*string, argument, case=case*) |  Returns whether a string is equal to a specified string. |
| **StrIndexOf** (*string, substring, case=case*)   | Searches for index of a specified substring in a string. Returns -1 if substring is not found. |
| **StrLower** (*string*)   | Returns string in lower case. |
| **StrReplace** (*string, src, dst*)   | Replaces all occurunces of src with dst in string, returning a new string. |
| **StrStartsWith** (*string, argument, case=case*) | Returns whether a string starts with a specified substring. |
| **StrTrim** (*string*)    | Removes all leading and trailing white characters (tabs, spaces, etc.), returning a new string. |
| **StrUpper** (*string*)   | Returns string in upper case. |
| **Substring** (*string, from=0, length=StringLength*)   | Returns a substring of specified string starting from a specified index and of specified length. |
| **Tokenize** (*string, separator="\t", option=""*)   | Converts string to a list of tokens based on the specified token separator. If option="prev", will convert all empty tokens to their previous token values.|
| **TokenizeLines** (*newVariableName, variableWithLines, fromLine=0, separator="\t"*)   | Converts a list of string in variableWithLines to the list of tokens based on the specified token separator. Adds result to a new variable newVariableName.|

<br>

CSCS Debugger
------

| **CSCS Function**                  | **Description**                                     |
| :------------------------------------------- |:------------------------------------------------|
| **StartDebugger** *(port=13337)*  | Starts running a debugger server on specified port (to accept connections from Visual Studio Code).|
| **StopDebugger** ()          | Stops running a debugger server.|


<br>

CSCS File and Command-Line Functions
------


| **CSCS Function**                  | **Description**                                     |
| :------------------------------------------- |:------------------------------------------------|
| **cd** *pathname*          | Changes current directory to pathname.|
| **cd..**              | Changes current directory to its parent (one level up).|
| **clr**           | Clear contents of the Console.|
| **copy** *source destination*          | Copies source to destination. Source can be a file, a directory or a pattern (like \*.txt).|
| **delete** *pathname*          | Deletes specified file or directory.|
| **dir** *pathname=currentDirectory*          | Lists contents of the specified directory.|
| **exists**  *pathname*         | Returns true if the specified pathname exists and false otherwise.|
| **findfiles**  *pattern1, pattern2="", ...*         | Searches for files with specified patterns.|
| **findstr** *string, pattern1, pattern2="", ...*         | Searches for a specified string in files with specified patterns.|
| **kill** *processId*         | Kills a process with specified Id.|
| **mkdir** *dirName*         | Creates a specified directory.|
| **more** *filename*         | Prints content of a file to the screen with the possibility to get to the next screen with a space.|
| **move** *source, destination*         | Moves source to destination. Source can be a file or a directory.|
| **printblack** (*arg1, arg2="", ...*)         | Prints specified arguments in black color on console.|
| **printgray** (*arg1, arg2="", ...*)         | Prints specified arguments in black color on console.|
| **printgreen** (*arg1, arg2="", ...*)         | Prints specified arguments in black color on console.|
| **printred** (*arg1, arg2="", ...*)         | Prints specified arguments in black color on console.|
| **psinfo** *pattern*         | Prints process info for all processes having name with the specified pattern.|
| **pwd**         | Prints current directory.|
| **read**          | Reads and returns a string from console.|
| **readfile** *filename*         | Reads a file and returns an array with its contents.|
| **readnum**         | Reads and returns a number from console.|
| **run** *program, arg1="", arg2=""...*         | Runs specified process with specified arguments.|
| **tail** *filename, numberOfLines=20*         | Prints last numberOfLines of a specified filename.|
| **writeline** *filename, line*         | Writes specified line to a file.|
| **writelines** *filename, variable*         | Writes all lines from a variable (which must be an array) to a file.|

<br>

CSCS Miscelaneous Functions
------


| **CSCS Function**                  | **Description**                                     |
| :------------------------------------------- |:------------------------------------------------|
| **CallNative** (*methodName, parameterName, parameterValue*)    | Calls a C# static method, implemented in Statics.cs, from CSCS code, passing a specified parameter name and value. Not available on iOS and Android. |
| **Env** (*variableName*)                   | Returns value of the specified environment variable.  |
| **Exit** (*code = 0*)               | Stops execution and exits with the specified return code.   |
| **GetNative** (*variableName*)    | Gets a value of a specified C# static variable, implemented in Statics.cs, from CSCS code. Not available on iOS and Android. |
| **Lock** { *statements;* }          | Uses a global lock object to lock the execution of code in curly braces.  |
| **Now** (*format="HH:mm:ss.fff"*)          | Returns current date and time according to the specified format. |
| **Print** (*var1="", var2="", ...*)          | Prints specified parameters, converting them all to strings. |
| **PsTime**       | Returns current process CPU time. Used for measuring the script execution time. |
| **SetEnv** (*variableName, value*)                   | Sets value of the specified environment variable.  |
| **SetNative** (*variableName, variableValue*)    | Sets a specified value to a specified C# static variable, implemented in Statics.cs, from CSCS code. Not available on iOS and Android. |
| **Show** (*funcName*)          | Prints contents of a specified CSCS function. |
| **Signal** ()         | Signals waiting threads. |
| **Sleep** (*millisecs*)          | Sleeps specified number of milliseconds.
| **StartStopWatch** ()          | Starts a stopwatch. There is just one stopwatch in the system. |
| **StopStopWatch** ()          | Stops a stopwatch. There is just one stopwatch in the system. A format is either of this form: "hh::mm:ss.fff" or "secs" or "ms".|
| **StopWatchElapsed** (*format=secs*)          | Returns elapsed time according to the specified format. A format is either of this form: "hh::mm:ss.fff" or "secs" or "ms".|
| **Thread** (*functionName*) OR { *statements;* } | Starts a new thread. The thread will either execute a specified CSCS function or all the statements between the curly brackets. |
| **ThreadId** () | Returns current thread Id. |
| **Timestamp** (*doubleValue, format="yyyy/MM/dd HH:mm:ss.fff"*)   | Converts specified number of milliseconds since 01/01/1970 to a date time string according to the passed format. |
| **Wait** ()         | Waits for a signal.  | 

