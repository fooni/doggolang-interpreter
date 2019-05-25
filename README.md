# Doggolang Interpreter
Doggolang is an esoteric programming language used in a 
[wundernut](https://github.com/wunderdogsw/wunderpahkina-vol10) coding challenge.

This project is an implementation of a doggolang interpreter 
capable of evaluating any doggolang code to an integer result.

The interpreter is built with [ANTRL](https://www.antlr.org/) and [Kotlin](https://kotlinlang.org/). The aim of the project was to
find an *elegant* and *minimal* solution to the problem.

## The solution
The challenge was to find the result of the following *very important code*:

```doggolang
samantha AWOO 1
hooch AWOO 500
einstein AWOO 10
fuji AWOO 0
GRRR fuji YIP hooch BOW
    samantha AWOO samantha WOOF 3
    RUF? samantha YAP 100 VUH
      samantha AWOO samantha BARK 1
    ROWH
      einstein AWOO einstein WOOF 1
      samantha AWOO samantha ARF einstein
    ARRUF
    fuji AWOO fuji WOOF 1
BORF
GRRR fuji YAP 0 BOW
    samantha AWOO samantha WOOF 375
    fuji AWOO fuji BARK 3
BORF
samantha
```

The result of the code is **64185** (verified by a [unit test](src/test/kotlin/lang/doggo/DoggoInterpreterTest.kt)).

The solution consists of the [Doggo grammar](src/main/antlr/Doggo.g4), 
a [DoggoVisitor](src/main/kotlin/lang/doggo/DoggoVisitor.kt) implementing the interpreter logic
and [DoggoTools](src/main/kotlin/lang/doggo/DoggoTools.kt) for bringing everything together and
evaluating Doggolang programs.

## Generating parser code
The project source does not contain the autogenerated code for lexing or parsing. This code can
be generated by running `./gradlew generateGrammarSource`. Code generation also run with Kotlin 
compilation so a viable alternative is to run the test `./gradlew test` to generate the code and 
verify that the generation was successful.

## Language ambiguities
Doggolang grammar is inferred from a few code samples and contains a couple of ambiguities. 
The following design choices were made with the grammar:

### YAP
`YAP` can be interpreted either as *greater than* or *greater than or equal*. The former
was chosen for aesthetic reasons as `YIP` can only be interpreted as *less than*.
 
### ROWH
`ROWH` interpreted as *else branch* was made optional as having an explicit *end if* token
resolves the [dangling else problem](https://en.wikipedia.org/wiki/Dangling_else).

### Result statement
All doggolang examples end in a variable evaluation. The interpreter adopts a more lenient
approach of accepting any expression as the final statement. As a result the final statement
is the only statement required to make a valid doggolang program.
