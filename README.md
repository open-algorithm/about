# About open-algorithm
The goal is a software which translates algorithms into code. For doing this, the software shall use syntax-definitions of programming languages. So the main function would be (Algorithm-Name x Programming-Lanugage-Name -> Code)
By doing this, the software can generate any kind of code which can be described using the syntax-definition-format, which should preferrably describe context-senstive-grammars.

An algorithm shall be an interface which has name like 'datastructure.string.substring'. If one then generates code from this algorithm, the software should at first look, if there is a predefined function (eg. in Java String::substring()) in the requested programming language for this interface. If there is none predefined function for this, the software shall look, if someone wrote an algorithm for this interface. This algorithm can use another algorithms which will be translated into code using the same behaviour. The algorithm consists of really basic language-constructs which most languages implement. If one algorithm uses a construct which the requested language does not implement (eg. not write-protected variables), the person who wants to generate code for a programming language must use another algorithm. 

# Goal:
The same algorithm only has to be written once and can be used anywhere.

# Example
User creates algorithm-definition in file 'jonasrudolph.example.echoFirstTenCharacters':
```
Type: Main-Procedure
Description: Prints the first ten characters of stdinput
```

and implementation in file 'jonasrudolph.example.echoFirstTenCharacters.algorithm': 
```
Write-Protected-Variables: 
- Name: input
  Type: openAlgorithm.type.string
- Name: output
  Type: openAlgorithm.type.string
Constants:
- Name: outputStart
  Type: openAlgorithm.type.integer
  Value: 0
- Name: outputLength
  Type: openAlgorithm.type.integer
  Value: 10
----------
input = openAlgorithm.io.read.stringFromCommandLine()
output = openAlgorithm.type.string.subPart(input, outputStart, outputLength)
openAlgorithm.io.output.toStandardOut(output)
```

where file 'openAlgorithm.io.read.stringFromCommandLine' contains:
```
Type: Generator
Returns: openAlgorithm.type.string
Description: Reads a string from standardInput
```

where file 'openAlgorithm.type.string.subPart' contains:
```
Type: Function
Returns: openAlgorithm.type.string
Description: Returns a part of a string
Input-Parameters:
- Name: str
  Type: openAlgorithm.type.string
  Description: The string you want a part from
- Name: start
  Type: openAlgorithm.type.integer
  Description: The index of the first character you want
- Name: length
  Type: openAlgorithm.type.integer
  Description: The length of the part you want
```

and where file 'openAlgorithm.io.output.toStandardOut' contains:
```
Type: Procedure
Description: Writes a string to standard-output
Input-Parameters:
- Name: str
  Type: openAlgorithm.type.string
  Description: The string you want to write to standard-output
```

The following files are also existing:
'openAlgorithm.io.read.stringFromCommandLine.haskell'
```
getLine
```

'openAlgorithm.io.read.stringFromCommandLine.java'
```
Import:
- Name: java.util.Scanner
----------
(new Scanner(System.in)).next()
```

'openAlgorithm.type.string.subPart.haskell'
```
Import:
- Name: Data.Vector
----------
slice start length str
```

'openAlgorithm.type.string.subPart.java':
```
str.substring(start, start + length + 1)
```

'openAlgorithm.io.output.toStandardOut.haskell'
```
putStrLn str
```

'openAlgorithm.io.output.toStandardOut.java'
```
System.out.println(str)
```


When the user now generates code for haskell using the algorithm 'jonasrudolph.example.echoFirstTenCharacters' he gets:
'output/jonasrudolph.example.echoFirstTenCharacters.hs'
```
module Jonasrudolph.Example.EchoFirstTenCharacters
where
  import Data.Vector
  
  -- Prints the first ten characters of stdinput
  main = do
    input <- getLine
    putStrLn $ slice 0 10 input
```

And when the user now generates code for java using the same algorithm he gets:
'output/jonasrudolph/example/EchoFirstTenCharacters.java'
```
package jonasrudolph.example;

import java.util.Scanner;

class EchoFirstTenCharacters{
 
  /**
   * Prints the first ten characters of stdinput
   */
  public static void main(String[] args){
    final String input = (new Scanner(System.in)).next();
    final String output = input.substring(0, 0 + 10 + 1)
    System.out.println(output);
  }
}
```
