# Mini project: Parser
Work in groups of 2

# Person 1: text-to-morse 
Create a parser that reads input from the console and prints the corresponding morse code.
For example, the input "hello world" should give the output:
```
.... . .-.. .-.. --- / .-- --- .-. .-.. -..
```
# Person 2: morse-to-output
Take a morse code as an input and communicate it by any means: audio, light, vibrations...
```
dit = 1 unit
dah = 3 units
intra-character space = 1 unit
inter-character space = 3 units
word space = 7 units
```

- Note: the unit time must be adjustable. 
- The time for testing will be 5, 10 and 20 WPM using the word PARIS as reference, in case of 5WPM this is equivalent to:
each PARIS word contains:
    10 dits * 1 unit = 10
    4 dahs * 3 units = 12
    9 intra-characters * 1 unit = 9
    4 inter-character * 3 units = 12 
    1 word space * 7 units =  7
= 48 units per WORD
so for 5WPM we have 240 units, meaning a unit is 0.25s


# Points
- Faster implementation: time to execute the whole workflow from text to output
- Smaller implementation: size of executable fits in less memory space
- Best architecture: good design, follows KISS and SOLID design principles.
- Obfuscated impementation: normally not good but in this case we want to prize creativity
- Features: ggoes beyond than parsing and outputting the code.
- Effective communication: a message will be given and whoever group that communicates the message first from one member to another is the winner.
- Preciseness: follows time units precisely.