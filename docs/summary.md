# 29/05/17
# Astro 0.1.11
SINGLE-LINE COMMENTS
    # Hello, World!

MULTILINE COMMENTS
    #=
        I’m Astro.
        #=
            Yay, nested comments!
        =#
        And I Rock.
    =#

SUBJECT DECLARATION & DEFINITION
    var a # unitialized subject declaration.
    var b = 25

    # var is used for subjects whose values can change.
    # let is used for subjects whose values cannot change.

    let c = 5 # covariant subject declaration.
    let d # uninitialized subject declaration.

    var Ω, δ = 5, 6 # multiple subject declaration.
    let x, y, z

IDENTIFIERS
    # name of subjects must always start with a character and
    # can be followed with characters, digits or underscore.
    var my_name
    fun show99BottlesOfBeer(): pass

    # naming convention
    # camel case for variables, constants, function names and module names
    let bookTitle = "Ender's Game"
    var currentYear = 2017
    fun addNumbers(a, b): a + b

    # pascal case for concrete and abstract type names
    type InventoryManager(name, id)
    abst LightSource: SpotLight | Directional | Ambient

    # for acronyms, only the first charcater should be upper case
    type XmlParser <: Parser

    # names preceded by underscores are special to the compiler
    print _args

CONSTANTS
    let b # deferred constant initialization
    b = 56

    let d = Int() # d == 0
    d = 10 # error!

VARIABLE/CONSTANT OBJECTS
    # objects are variable by default
    # they can be made immutable with the const keyword
    var name = const Name('Steve')
    let list = const [1, 2, 3, 4]

FUNCTION DEFINITION (1)
    fun add(a, b): # parameters are immutable by default
        return a + b

    var sum = add(25, 52)

    fun echo(value):
        print(value, last:'')

    # you can annotate the argument and return types of a function
    ## Int, Int -> Int # is a type annotation
    fun mod(a, b):
        return a % b

    fun swap(var a, var b): # the bang character signifies mutability
        a, b = b, a

COMMAND NOTATION
    # if a function takes only one argument, the call parens can be ommitted if the argument is not preceded by a prefix operator, a list literal, a dict literal, a set literal or a regex literal
    print "Hello"
    print name
    print [9] # invalid
    print {9} # invalid
    print /9/ # invalid
    print 56
    print +5 # NOTE: this will evaluate as print + 5 not print(+5)

EXPRESSION-ORIENTED
    var isNyproCrazy = faveHobby == 'CountingBirds' # returns true or false to isNyproCrazy

    fun add(a, b):
        a + b # 'return' is not needed here. result of a + b will be returned to the caller.
    ..
    nypro.hieght = add(aditya.height, tripleo.height)

    # a semi-colon at the end of a block, stops it from returning its evaluated
    # result
    fun setName(p, name): ## Person, Str
        p.name = name;
    ..

    let name = setName('John Smith') # error! setName returns no result.

SPECIFYING TYPES
    var number ## Int # type specification
    var todo = Str('Create a programming language')

    var address = 506 # initialization
    let job = Str() # default construction.

    # optional type specification.
    var identifier ## Str|Int

    # inclusive type specification.
    var pegasus ## Horse&Bird

SOME BUILT-IN TYPES
    var index = UInt(2_000) # UInt represents unsigned integer; no negative values.

    var debt = Int(-100) # Int represents signed integer; you can have negative values.

    let e = F64(2.718281828459045)

    let dogBreed = Str('German Shepherd') # Str represents immutable UTF-8 string.

    var laptop = Chars('Alienware M18') # Chars represents mutable UTF-32 string.

    var listOfGroceries = ['Oranges', 'Cabbages', 'Tomatos', 'Bananas'] # this is a list.

    var game, year = 'BioShock Infinite', 2014 # this is a tuple.

VALUES & REFERENCES
    # by default primitive objects (UInt, Int, Float, Bool) are passed around by value and
    # by default complex objects (Str, user-defined types) are passed around by reference.
    var number = 502
    var account = getAccount('Dumbledore')

    # however, you can change this behavior with 'ref' and 'val'.
    var newAccount = val account # passing a complex object by value.
    var pointer = ref number # passing a primitive object by reference.

    # Astro uses compile-time reference counting to manage memory, unlike Python or
    # Javascript which use runtime tracing GC.
    # It also does reference cycle breaking at compile-time. :)
    myCompany.affiliate = ref yourCompany
    yourCompany.affiliate = ref myCompany

    let myAccount = iso Account('Nypro')
    # iso means only the subject can hold a reference to the object.

    let trooperClone = acq trooper # acq is shallow copy operation.

    let player = team.player

    fun getAmount():
        return const amount # passed by reference, but it cannot be written to.

    fun swap(var a, var b): ## ref, ref -> $
        a, b = b, a;
    ..

NUMBERS
    var index    = 5
    var axis     = -3
    let meters   = 0.25e-5
    let salary   = 10_000'f # 'f marks it as float
    var price    = 0x6FFF00p+12 # hex
    var opCode   = 0b10110001 # bin
    var interest = 0o566768 # oct
    let pi       = 3.14'bf # BigFloat

ACCESS MODIFIERS
    var money = 0 # subjects, functions, types, etc. are public by default.

    fun sub'(a, b): # becomes inaccessible outside module
        a - b

    var pi' =  3.14

    type Person'(var name', var age')

MULTIPLE DECLARATION # DEPRECATING
    var
        title  = "The Drunk Chef Visits Chicken Town"
        author = "appcypher"
        year   = "2028"

    type
        Person(var name, var age)
        Student(var courses, var class)

PROPERTIES
    # properties provide getter/setter behavior.
    # first braces pair contains the setter, the second braces pair, the getter
    var age  = {age + 5} -> {age - 5}

    # the last expression in the setter braces becomes the properties value
    # the last expression in the getter braces is the properties returned value
    var name = {name} -> {name + "Smith"}

    # empty braces means the setter/getter is inaccessible
    var date = {date} -> {}

TUPLES
    # tuple is a datatype whose element types and length are determined statically
    # you can't appended to or removed from tuples
    var name, age = 'Emeka Okorafor', 27 # this is an open tuple
    var game, year = ('Uncharted 4', 2016) # this is a closed tuple

    var a, b = x, y # a = x; b = y
    var a, b = (x, y) # a = (x, y); b = (x, y)
    var (a, b) = (x, y) # a = x; b = y

    let map = () # empty tuple

    let map = (london,) # one-element tuple
    # NOTE: the trailing comma

    # named tuple
    let http200Status = (statusCode:200, description:'Ok')

    let arguments = (5, 6)
    add(...arguments) # tuple unpacking

    # practical example
    fun check(f, ...arguments):
        let test = f(...arguments[:!1]) == arguments[!0]
        print ..('Test passed') if test else ..('Test failed')

    check(add, 5, 3, 8)

LISTS
    var unorderedList = [7, 3, 8, 5, 4, 0, 9, 1, 2, 6]

    var emptyList = []

    var names ## [Str] # specifying type of list

    # heterogenous list
    var stuffs = [200'USD, 'Steve', 1'f, Car('Eleanor')]

    # Astro uses 0-based indexing, so every list indices start at 0.
    stuffs[0] # 200'USD

    # the start index of a list view is inclusive but the end index non-inclusive
    var disqualified = contestants[1:6] # 2nd index to the 5th

    # "!" can be used to access the index backwards
    var last = contestants[!0] # last index

    # getting a view of the array from first to last index
    var all = contestants[:] # all indices

    # using negative step to reverse index access
    var reversed = contestants[::-1] # all indices backwards

    # reverse access can also be done by making the "!" index the start index
    var reversed = contestants[!0:] # all indices backwards

    let myGarage = [Car][('Mustang'), ('Eleanor'), ('Bugatti'), ('Lamborghini')] # You can specify a type

    let number = List[Str](31, 12, "")

    # list field generator
    let students = getStudentList()
    let totalGpas = students.:gpa.sum()

    fun average(students):
        students.|:gpa.sum() / size|

    # proposed operations with lists.
    var concatenate = [1, 2] ++ [3, 4] # [1, 2, 3, 4]
    var multiply = [1, 2] ** 2 # [1, 2, 1, 2]
    let subtract = [1, 2, 3, 4, 3] -- [4, 3] # [1, 2]
    let divide = [1, 2, 5, 6, 3, 4] // [5, 6] # [[1, 2], [3, 4]]

ARRAYS
    # List type is an subtype of Array that requires preallocating its indices
    # Arrays do not pre-allocate and it is adviced to use Array sparinly, with care and only when
    # high performance matters
    let calendar = 31x12[Str][] # specifying the size of an Array

    let a = 2×4 [
        [1, 2, 7, 8]
        [3, 4, 5, 6]
    ]

    # 2-dimensional list literal can be constructed using horizontal concatenation
    let a = 2×4 [
        1, 2, 7, 8;
        3, 4, 5, 6
    ]

    var matrix = Array[Int]([
        [1, 0]
        [0, 1]
    ])

    matrix[,] = [
        1, 6;
        3, 7
    ]

    let subset = mat4[1:, 1:2]

    # vectorised operations
    let z = 5 * subset.
    let y = inc(subset.)

    # Astro's lists are by default, row-major order, but you can make it
    # column major by using the transpose function
    let y = [1, 2, 3]
    # [1 2 3]

    let z = [1, 2, 3]! # transpose
    # [1]
    # [2]
    # [3]

    let a = [1, 2; 0, 4]

    # list unpacking
    let b = [
        unpack.[a!, a];
        unpack.[a, a!]
    ]
    # [1 0 1 2]
    # [2 4 0 4]
    # [1 2 1 0]
    # [0 4 2 4]

    # non-standard list literal
    let spMatrix = 2x2[Int] sp.[]

DICTIONARIES
    # dictionaries are basically key-value lists.
    let family = {
        'mum': {
            'name': 'Esther Williams'
            'age' : 42
        }
        # unquoted keys are taken as strings as well
        dad: {
            name: 'Sunday Williams'
            age : 46
        }
        # nested dict can be simplified with indentation
        sister:
            name: 'Shade Williams'
            age : 15
    }

    # accessing a value
    let sisterName = family{'sister', 'name'}
    let dadName    = family.dad.name

    # adding a new key-value pair
    family.brother = {
        name: "Daniel Williams"
        age : 23
    }

    # to use variables from the outer scope, the variable name needs to be escaped with `$`
    var professor = {
        $id1: 'Charles Xavier'
        $id2: 56
    }

    var emptyDict = {:} # empty dictionary

    var user = 2x2 Dict[Str, User]()
    var scoreList = Dict[Str, Int]()

    # when a key has the sam name as the value, the value can be ommitted
    var john = { name:, age: }

    # when a dictionary is not used dynamically, the compiler
    # treats it as an object and optimizes it
    # aliasing the type of a dict
    type Person = john.type

SET
    # is an unordered list type with no duplicate elements
    let fruits = {'orange', 'mango', 'guava', 'apple', 'orange'}

    fruits.size # 4

    # common set operations can be applied to sets
    {'guava', 'mango'}.intersect(fruits)

    {'pineapple', 'apple'}.union(fruits)

    'mango' in fruits

    let emptySet = {}

RANGE
    # range is a type of list and it's end value is non-inclusive
    let range = [0:20] # 0 to 19

REST
    var a, ...b = 1, 2, 3, 4

    fun sum(a, ...b):
        a + b.foldl(0, |x, y| -> x + y)

STRINGS
    let language = 'Astro'
    let year = 2015
    var story = "$language was started in the $year" # string interpolation.

    let calc = '5 * 50 = $(5 * 50)'
    # both single and double quotes can be used to represent a string literal.

    # non-standard string literals preceded by characters are not affected
    # by escape sequences, because they are processed verbatim.
    var verbatimStr = r."Use '\t' to represent tab"

    # multiline string. first and last new lines are always ignored.
    var verbatimStr = '''
    Hello, World!
    '''

    var string = 'Hello' # Str
    var chars = ch.'π' # Char is 32-bit UTF8 type

    # proposed operations with strings.
    var concatenate = 'ab' ++ 'c' # 'abc'
    var multiply = 'ab' ** 2 # 'abcabc'
    let subtract = 'abcac' -- 'ca' # 'abc'
    let divide = 'abcdeabc' // 'de' # ['abc', 'abc']
    let escapeSequences = '\t \n \' \" \[ \q \# \\'

    # string continuation
    var greeting = 'Hello, ' ...
    'World!'

STRING FORMATTING
    # padding
    let leftPadding        = 'left padding:        $:>10(string)'
    let rightPadding       = 'right padding:       $:<10(string)'
    let placeholderPadding = 'placeholder padding: $:_<10(string)'
    let centering          = 'centering:           $:^10(string)'
    # truncation
    let truncating         = 'truncate string:     $:.10(string)'
    # numbers
    let integer            = 'integer:             $:d(number)'
    let float              = 'float:               $:f(number)'
    let truncation         = 'figure truncation:   $:6.2f(number)'
    let positive           = 'positive:            $:+d(number)'
    let negative           = 'negative:            $:-f(number)'
    let negTruncation      = 'negative truncation: $:=-5f(number)'
    # date-time | custom objects
    let date               = 'date:                $:|Y-m-d H:M|(date)'

MULTILINE EXPRESSIONS
    # expressions that spread accross multiple lines must be enclosed in brackets
    # or used with an isolated three dots ending the previous line.
    var zero = -100 ...
    + 100

    if x < y ...
    & a == b:
        doTask

    var languageDesigner =
        "Lagbaja Jagaban"

    sum(1, 2, 3, 4, 5, 6, 7, 8
    9, 10, 11, 12)

    if x == 0, ...
    if y == 1:
        print 'cool!'

USE BLOCK:
    # a use block is like 'do' block but can only access parent scope subjects
    # declared in it's parameter section.
    let x, y, z = 1, 2, 3
    use x, y:
        print x
        print z # Error! can't access z from this scope


PASS KEYWORD
    # pass keword can be used to signify an empty block
    if name == 'Jagaban': pass
    fun div(a, b): pass


INDENTATION
    # Astro is an indentation-based language
    if x == y:
        if a == b:
            doTask
    # indentation is used here to disambigute if-else association
    else:
        doNothing

    let myName = " Nypro "
        ..trim()
        ..reverse()
        # indented dot/arrow notation links with the argument of the previous  line.
    ..print()

    playMusic(
        'Asa',
        'Jailer',
        '2009'
    ).rewind 2

    var x =
        25 + 6
        # a continued line following an assignemnt operator should be indented

    # multiple blocks can also be written on the same line using the '\\' punctuator.
    if studio.isLive: blockAccess \\ else: openAccess


IF EXPRESSION
    if isAdrianRich:
        spendAll('on parties')

    elif isOroboRich:
        spendAll('on legos')

    elif isSyconRich:
        spendAll('on suits')

    else:
        cry('we are broke')

    if phoneNumber.nil:
        useEmail()

    if stockCode <- getStockCode('APPLE'):
        print('APPLE: $stockCode')

    var player = Player()
    if $player <- getPlayer(name): # reassigning an variable with '$'
        print player.fitness

    if copy <- copy(document):
        print(copy)

    # nested if statements can be written together with their corresponding
    # else statements stacked accordingly
    if tim.age > 16 if car.state == 'good':
        tim.drive(car)
    else:
        raise Error('Driver must over the age of 16')
    else:
        raise Error('Car is in bad state. Repair immediately!')

    # if-else expression
    return 10 if x == y else 20

NIL
    # sometimes, it is important to represent an empty or missing state, this can be
    # achieved in Astro with optional typing.
    var code ## Str # cannot be nil
    var program = 'print(\"Hello World\")' ## Str|Nil
    var anotherProgram ## Str?

    # a nilable cannot be assigned to a non-nilable subject without a proper nil coalescing with a fallback value.
    var programList = Str()?
    var cartoonList ## [Str?]

    # nilable unwrap
    let name = dave?.name

    # exceptionable unwrap
    getAccount("Default")!! # if result is nil, raise NilError

    # nil coalescing
    let name = getName() ?? "John Doe"

    var b = "Hello"?
    b if b? else 5

    if isRecieving? == true:
        print('boolean value recieved')
    elif isRecieving.nil:
        print('remote server stopped recieving')

BOOLEAN EXPRESSIONS
    # Astro discourages repetition in conditional expression.
    x < y and y < z
    x < y < z

    x == y and y == z
    x && y == z # if both x and y equals z.
    x == y == z

    x == y or y < z
    x == y || < z # if x equals y and also greater than z.

    x == y and a == b # no warning

    # NOT
    # the not operator in Astro is placed adjacent the target boolean
    x != y
    x.!isbound
    x !in list
    !x?
    !(x.hasValue(0))


FOR LOOP
    for i in [1:11]:
        print i

    let interestingNumbers = {
        prime      : 2, 3, 5, 7, 11, 13
        square     : 1, 2, 4, 9, 25, 36
        fibonacci  : 1, 1, 2, 3, 5, 8
    }

    # pattern matching with for loop
    for (kind, number) in interestingNumbers: # parallel pairing through a dictionary.
        print '$kind: $number'

    for name, (kind, number) in nameList, table:
        print((name, key, value))

    # looping through a number
    for num = 50: num

    for up, down ← [1:21], [21:1]: # parallel pairing through ranges.
        print '$up <=> $down'

    for upper in [1:21], lower in [1:21]: # nested iterations through ranges.
        print '$upper <=> $lower'

    for i in [1:21]:
        i += 1 # error! immutable subject.
        j = i
    ..

    for var i in [1:21]: # mutable iterator
        i += 1
        j = i
    ..

    #  the 'end' block which executes when a loop does not break
    for line in file:
        if line.contains code:
            echo "code found!"
            break
    end:
        echo "code not found!"

    file.writeln ..(el) for el in array end ..()

    for _ in [1:fifty]:
        print('Hello')

    yield x for x in stream()

COMPREHENSION
    var listComprehension = list(yield x for i in [1:21] if i mod 2 != 0)
    var listComprehension = [x | x in [1:21] where x mod 2 != 0]
    let dictComprehension = {x : y | x in [1:11], y in [21:1] where even(y)}
    var setComprehension  = {i | i in random(0, 200)}
    var comprehension     = (i | i in random(0, 200))

    var pythagorasList = [(x, y, z) |
    z <- [1:21]
    y <- [1:z+1]
    x <- [1:y+1]
    where x² * y² == z²]

BREAK WITH
    for name in register:
        if name == 'Tony':
            break name
        ""

IN
    if student.name in defaulterList:
        print '[student.name] hasn\'t paid yet. Contact parents'

    if 'ps4' !in birthdayPresents:
        print 'Aaargh! Everyone hates me'

    if /dollar[s]?/ in sentence:
        print 'Change occurrences of "Dollar" to "Pound"'

CHAINED CONDITION CONSTRUCTS
    if person in auditionRoster, if person.mark > 40.0:
        acceptanceList.add person

    # there can ohly be two conditional constructs in a chain.
    for book in library, if book.title.contains('adventure'):
        personalLibrary.add book

WHILE LOOP & LOOP
    while file.hasNext():
        print file.next()

    # loop
    loop: # synonymous with while true
        let input = scan '>>> '
        let tokens = lex input
        let ast = parse tokens
        let bytecodes = compile ast
        let result = interpret bytecodes
        print result

    redo:
        lines = gen.readLine() # evaluated at least once.
    while gen.hasNext()

    while list.size > 0, for name in list:
        echo('$name $newline')
        list.removeTop()

CONDITION EXPRESSION
    # ternary operator is a summarized if-else clause.
    var absValue = if a > 0: a \\ else: -a

    var absValue = a if a > 0 else -a

    print ..('Test passed') if test else ..('Test failed')

    # a ternary operator statement needs to be encapsulated in brackets
    # where a semicolon is expected
    if a == (b if b > c else c): echo a

MAIN FUNCTION
    # if a module provides a main function, it will become the module's entry point
    fun main():
        print "Hello World!"

FUNCTION DEFINITION (2)
    fun multiply(a, b):
        a * b

    # if a function returns nothing, it can be annotated Nons
    fun print(point): ## -> None
        print(point.|a, b|)

    # or the return type can be left empty
    fun greet(name): ## Str
        print "Hello $name"

    # to make sure nothing is returned from a function you can use `;`
    fun effect(mood):
        if mood.isHappy:
            happySong.play();
        else:
            sadSong.play();

    # varargs.
    ## Int -> F64
    fun arithMean(...numbers): # variable number of arguments
        var total = 0
        for number = numbers:
            total += number
        total / numbers.size
    ..

    arithMean(1, 2, 3, 4) # multiple arguments can be passed to it
    arithMean(...list) # or a splatted tuple can be passed to it

    # anonymous functions are nameless functions
    fun _(): return a + b

    # function assignment
    fun sum = add

    # the states local subjects marked with '`' are always preserved
    # between function calls.
    fun callCount():
        var count' = 0
        print 'count = $(count += 1)'

    callCount() # 1
    callCount() # 2

    # functions that change values of arguments must be annotated with `!`.
    ## ref Person, ref Person
    fun swap(var a, var b):
        a, b = b, a;

    swap(john, jane)

    # default parameter values and shared value with `&` binder
    fun login(username:'demo', password:'demo'):
        access Account(username, password)

    login(password:'logic404') # using default username

    # compulsory named arguments
    fun signUp(username., password.):
        access Account(username, password)

    signUp(username:'appcypher', password:'bazinga')

    fun sendMessage(message, to.recipient):
        print((message + recipient).toUppercase())

    sendMessage('Hello', to:'Cantell')

    fun sum(first., ...rest.):
        first + rest

    # accessing a function's arguments with args
    fun printPerson(name., age.):
	    print _args

    printPerson(john) # [name: 'John', age: 25]

FIRST-CLASS FUNCTIONS
    # creating new functions from existing functions
    var details = getDetails ## User
    var plus    = add  ## Int, Int -> Int
    fun times   = mul ## F64, F64

    var binaryOp ## Number, Number -> Number

    # passing functions to functions
    fun compress(image, f): ## Image, (Image -> Image)
        f image

LAMBDAS
    let scoreListWithExtraMarks = scoreList.map(fun _(score): score + 5)
    let scoreListWithExtraMarks = scoreList.map(markFilter)

    fixtureList.filter(|game| -> game.!isCancelled)

    var pplBelow25 = census.filter |person| ->
        person.age += 1
        person.age < 25

    list.foldl(0) |x, y| -> x + y

    # recursion
    # you can recall an anonymous function using the fn keyword
    list.aggregate(0) |a| ->
        1 if a == 0 else a + fn(a - 1)

    # destructuring in lambda
    |{name:, age:}| -> print "Hello $name"

CLOSURE
    # a closure is an inner function.
    fun genDbConnector(host., username., password.):
        return fun makeDbConection(): # a closure can be returned.
            db.connect(host, username, password)
    ..
    var dbCallback = genDbConnector(host:'localhost', username:'nypro', password:'willdiearobot')

PARTIAL APPLICATION
    # partially applied functions are function calls with one or more of it's argument not applied
    let add3 = add(3, $)

    list.map add3

IMMEDIATELY INVOKED FUNCTION EXPRESSION (IIFE)
    () ->
        let sum = 45 + 23
        print sum

    # the value is passed as parameter to the lambda
    (a:5, b:10) -> a + b

    var primes = ([2:]) ->
        | [p, ...xs]: p ++ fn([i | i in xs where i mod p != 0])

    var x = (|a, b| -> a + b)(2, 3) # deprecation warning!

SELF
    # the first object passed to a function call can be reference using 'self'
    ['bread', 'pepper', 'tomato', 'lettuce', 'cucumber']
        .map('sliced ' ++ $)
        .join(', ')
        .mutate('I had a tasty lunch made with ' ++ self)

COFUNCTIONS # DEPRECATED
    fun remove(list, index): ##List[T], Int
        list -= list[index]

    remove += refreshUI # attaching a cofunction.
    emit movieList.remove(2) # emit is used to run the function along with its cofunctions.

CONTEXT LABELLING
    fun play(music): #> top
        if music != nil: music.start()
        |music| ->
            if music.artist "Bieber":
                return music.artist at top
    ..

    loop: #> outer
        while file.hasNext():
            var line = file.nextLine()
            if line != "":
                print line
            else:
                break at outer
    ..

EXT SUBJECTS
    let state = 'Idle'

    fun changeState(state):
        if state != nil:
            activateState state
        else:
            activateState ext.state # ext refers to the parent scope.
    ..

PATTERN MATCHING
    # match block is like switch-case statement, and it matches against the expression on the previous line.
    object
    | 0           -> spill # a keyword for moving to the next case
    | 1||23||40   -> .name ++ n
    | n           -> n * n
    | = getVal()? -> .value
    | [x, ...y]   -> print "list"
    | Point(x, y) -> x * y
    | /[a-z]+/    -> print "regex"
    | [x, y, z]   -> print "list"
    | {x, y, z}   -> print "set"
    | {x:, y:}    -> print "dict"
    | (x, y)      -> print "list"
    | {a:b}       -> print "dict"
    | [a:b]       -> print "range"
    | Int         -> print "integer"
    | $outer      -> outer
    | 5 if x == z -> doSth()
    | .name == 45 -> 45
    | .> 25       -> 'Greater than 25'
    ~ x > 25      -> x * 2 / 5
    ~ x < 25      -> 'x is lesser than 25'
    ! in [1..10]  -> 78_324'b
    | _           -> 'default' # otherwise
    | else        -> 'default' # same as above

    # do-match
    print $ if fruit
    | 'grape' -> 'fruit is a grape'
    | 'apple' -> 'fruit is a sweet apple'

    # you can pattern match with any language construct that has arguments
    while turn in ['y', 'Y', 'n', 'N']:
        | 'y'||'Y' -> return 1
        | 'n'||'N' -> return 0
        | _        -> print "its an invalid choice"

ASSIGNMENT DESTRUCTURING
    let [name, age, _, ...rest] = list
    let (name, age, _, ...rest) = tuple
    let {name, age, _, ...rest} = set
    let {name:, age:, _:, ...rest:} = dict
    let {key:value}  = dict
    let a, b, c = ..."abc"
    let a, b, c = x, y, z
    john.|name, age| = (newName, newAge)
    [john.name, peter.age] = ["Jean", "Pierre"]

    # pattern matching with iterators
    for (x, y) in dictionary:
        print "$x : $y"

PATTERN MATCHING OPERATOR
    let pattern = /\d+/
    let number  = "1234"
    if number ~ pattern: print number
    if number ~ Str: print number

COROUTINES & GENERATORS
    fun count(num):
        for x in [:num]: var z = yield x

    var counter = count(25) # instantiating coroutine
    count.next(25)
    count.raise(Error "Some error :)")
    delegate coroutine # 'delegate' is similar to Python's 'yield from'

ASYNC AND AWAIT
    fun showUrlText(url): #@ async
        print(str html) await fetch url
    ..

ACTORS # NEEDS REVIEW!!!
    type Task <: Actor:
        var name, time ## Str, Int

    fun Task(name):
        new(name, random(1, 1_000))

    fun sleep(actor): ## Task
        print "$name is sleeping for $(time)ms"
        sleep time
        print "$name is done"

    var
        t1 = Task("Task-1").sleep
        t2 = Task("Task-2").sleep
        t3 = Task("Task-3").sleep

TYPE DEFINITION
    type Car:
        var make, model ## Str

    fun move(car): ## Car
        "Moving"

    # multiple type declaration is allowed for types without
    # parameters or parent types
    type Duck, Person

    # initializer
    fun Car(make, model): ## Str, Str
        new(make, model)
    # new is a special function that creates a new object and maps
    # the arguments to corresponding fields

    fun Car(maker, model, year)

    # a destructor can be used for cleanups
    fun Car(!)

    type Person(var name, var age) # Person is a initializer type.
    # Initializer types have just main initializer and the parameters
    # of the initializer are mapped to fields.

    var john = Person('John Connor', 23)

    type Lamp: var color
    var yellowLamp = Lamp 'Yellow'

    # initializer overloading
    fun Lamp():
        Lamp 'White'

UNION AND INTERSECTION TYPES
    var pegasus ## Bird&Horse
    var identification ## Str|Int

TYPE CONVERSION FUNCTIONS
    # camelCased version of type names are, by convention, meant for type conversion operations
    type MyInt <: Int
    type MyFloat <: Float

    # new values are constructed out of such conversion.
    fun myInt(myFloat):
        new truncate myFloat

FIELD EXTENSION
    type Programmer(specializedLanguages) <: Person:
        let languages ## [Str]

    let orobogenius ## Programmer
    orobogenius.languages = ["Java", "PHP", "JavaScript"]

    let appcypher = Programmer(["Astro", "Java", "C++"])
    appcypher.philosophies = ["Open Source", "Transhumanism"]
    appcypher.resume = "chicken.poop.com/appcypher"
    appcypher.mentor = "Captain Jack Sparrow"

    var nypro = appcypher

TYPE EXTENSION
    type Programmer(specializedLanguages) <: Person:
        let languages ## [Str]

    Programmer.philosophies ## [Str]
    Programmer.resume
    Programmer.mentor

INHERITANCE
    type Animal
    fun sound(animal): ## Animal
        print 'Nothing'

    type Bird <: Animal
    fun sound(bird): ## Bird # overrides supertype function.
        print 'Chirp!'

    type Horse <: Animal:
    fun sound(horse): ## Horse
        print 'Neigh!'

    # Astro supports multiple inheritance.
    type Pegasus <: Horse, Bird:
    fun sound(pegasus): ## Pegasus
        sound pegasus as Horse

    # due to Astro's multiple dispatch nature, Inheritance applies equally to all
    # arguments of a function.
    type A
    type B <: A

    fun foo(a, x) ## A, X
    fun foo(b, x) ## B, X # this overrides top

    fun bar(x, a) ## X, A
    fun bar(x, b) ## X, B # this also overrides top

    # a super method can be called directly or by using super
    fun baz(x, b): ## X, B
        baz(x, b as super)

INITIALIZER TRAIN
    # initializer train is the way Astro ensures the initialization of all declared and inherited fields of a particular type.

    # it's basically about making each type responsible for the initialisation of the fields it introduced.
    type Person(name)
    type Teacher(subject) <: Person
    type Student(course) <: Person

    type TeachingStudent <: Teacher, Student:
        var schedule

    fun Teacher(name, subject): new(subject).super(name)

    fun Student(<name>, course) # single inheritance can be shortened

    fun TeachingStudent(name, subject, course, timetable):
        # a subtype cannot assign to an inherited field in the initializer train.
        new(timetable).Teacher(name, subject).Student(_, course)

    # if the compiler finds out an initializer doesn't initialize a field, it complains.

DIAMOND PROBLEM
    fun register(teacher): ## Teacher
        staffList teacher.name

    fun register(student): ## Student
        studentList student.name

    # when TeachingStudent type is declared, an ambiguity error will be issued about `register`.
    type TeachingStudent <: Teacher, Student:

    # the function needs to be overridden
    fun register(teachingStudent): ## TeachingStudent
        staffList teachingStudent.name
        studentList teachingStudent.name

    var john = TeachingStudent 'John Smith'
    john.register()

    # as a result of these MI issues, Astro advocates favoring single inheritance and composition over multiple inheritance.


ABSTRACT TYPES & FUNCTIONS # @INC
    abst Player # abstract types cannot be instantiated.

    # a function without a body (apart from initializers) is an abstract function and an error is raised if it is called directly.
    # An abstract function is expected to be implemented by a corresponding subtype function.
    fun play(pl) ## Player
    fun rewind(pl) ## Player
    fun fastForward(pl) ## Player

    type DigitalPlayer <: Player
    fun play(dpl): ## DigitalPlayer # this overrides and implements play.
        initPlaylist()
        playPlaylist()

ABSTRACT TYPES AS ENUMS
    # abstract types can be used like enums in other languages
    abst Tree:
        Leaf(value) ## $T -> Leaf
        Node(l, r) ## Leaf, Tree -> Node

    ## Tree -> $
    fun sum(t):
        | Leaf -> .value
        | Node -> sum(.l) + sum(.r)
    ..

    # abst typevalues don't need namespacing when used in the same module as they are declared
    var x = Node(Leaf 1, Node(Leaf 2, Leaf 3))

SINGLETONS
    let switch = { on: false }

    fun toggle(switch): ## switch'
        | .on == true  -> .on = false
        | .on == false -> .on = true
    ..

    window.addMouseListener({
        mouseClicked: |self, event| -> print "Mouse Clicked!"
    })

TYPE CHECKING
    if myCar :: Car:
        print 'myCar is a Car'

    if myCar <: Car:
        print 'myCar is a supertype of Car'

    if myCar !>: Car:
        print 'myCar is not a supertype of Car'

    head([1, 2, 3]) :: ([Int] -> Int)

IDENTITY/REFERENCE EQUALITY
    let a = Person 'John Smith', 25
    let b = a
    let c = val a
    a is b # true
    a is c # false
    (this is that) == (this.ref == that.ref) # true

FUNCTION == METHODS (UFCS)
    fun show(p): ## Person
        print((self.name, self.age))
    ..

    show nigel
    # In Astro, a function is a method of its first argument.
    nigel.show()

    # The only two set of functions that cannot be used with dot operator are
    # contructor functions and new functions.
    type Robot:
        var name, uniqueID
    ..

    fun Robot(name, uniqueID):
        new(name, uniqueID)

    var wall_e = Robot('Wall•E', 215)


SUBJECT REPEAT SYNTACTIC SUGARS
    # return chain.
    var result = 8.plus(6).minus(4).times(2)

    # cascading notation
    # a convenience syntax in which the intial associated object forms a long chain of access.
    profile ..open() ..deleteMsgs() ..close()

    # function application piping
    buffer ..resize(13) |> ..fill(0, $)
    add(2, 3) |> sub($, 5) |> div(2, $)

    #@ infixl(5)
    fun |>(f, x): f(a)

    # call chain
    var nameList ## [Str]
    nameList.add ..('Badmus') ..('Travis') ..('Gabriel')

    # cascading dot
    a if |a == b|.size else b
    list.|[1] + [2] + [3]|

    # chain tupling
    # NOTE: tupling call chain and tail object chain returns an open tuple
    # however tupling cascading dot returns a closed tuple
    var name, version = haskell ..name, ..version
    var first, second = getPerson ..('Seamus'), ..('Flinn')
    print(john.|name, address|.toUppercase())

COVARIANCE
    type Person(name, age)
    type Employee(<name, age>, job ) <: Person
    type Teacher(<name, age>, course) <: Person

    ## Person
    fun show(p): # a covariant parameter can take a subtype too.
        print((p.name, p.age))

    show Person('Tony Stark', 36)
    show Employee('Peter Parker', 17, 'Photographer')
    show Teacher('Diana Lane', 12, 'Biology')

    var people = [Person][Teacher(), Employee(), Employee()] # a covariant list.

TYPE CASTING
    var radianInt = int(2 * pi) # the resulting value Float of 2*pi is casted to Int.

    fun employee(person): ## Person
        Employee(self..name, ..age)

    var deitel = Person('Paul Deitel', 57)
    var editor = employee dietel

    var yearStr = str 1990
    let sorted = int yearStr.sort()

NAME ALIASES
    type Number = Integer|Float|Complex
    fun cheb    = chebyshevCoefficient
    let pi      = piConstant

FUNCTION & TYPE OBJECTS
    fun foo = bar ## Int -> Int
    type BookTitle = Str

    var
        qux = 5
        doo = bar(Str -> Int)

    fun mix(a):
        | 1 -> |a, b| -> a + b
        | 2 -> 25

    fun baz = mix 1 # error
    var bin = mix 1

MODULES AND IMPORTS # NEEDS REVIEW!!!
    import math # import all
    import math, dataframes # import all
    import math except {tan, atan} # import all except
    import nypro/math {pi, cos} # module path
    import ../math

    # namespace can help disambiguate imports
    import nmath:nypro/math {cosine:cos, sine:sin, π:pi}
    let b = nmath.tan(45)
    let d = cosine(30)

    # dynamic import
    # importing a module that is only available at runtime
    import !$(userFolder)/math {cos, sin}
    let userFolder = sys.userFolder
    # a copy of dynamic module must be in the lib folder

    # aggregate module
    # a single module can be split between different files by numbering them in certain way.
    # vector.ast, vector-1.ast, vector-2.ast, etc. all combine to a single module
    import vector

EXPORT
    export math except {cos, sin}

GENERICS
    # declaring a generic type T.
    fun add(a, b): ## $T, T -> T
        return a + b

    # where the type of the generic symbol is hinted, '$' can be ommitted from its declaration
    ## T, T, T -> T ~ T::Int
    fun add(a, b, c):
        var d ## T
        d = a + b + c

    var sum = add[Int](2, 4, 6)

    # if the generic arguments can be readily inferred, then type argument can be ommited.
    var sum = add(2, 4, 6)

    # value constraints
    # they allow the code to specify a range of values a subject should have,
    # and depending on the staticness of the subject, the an error will be thrown
    # at compile-time or runtime when the range or value is exceeded.
    var age ## Int(1:120) # age is constrained between 1 and 120
    var name ## s()::Str # s is any value constrained to Str

    # value constraints are subject declarations in their own right
    # and can cause name clashes

    # types can have generics too
    type MyList: ## $T, l() ~ l<:Int
        let list = List[T](l, 0)
        let size = l
    ..

    # nillable type
    let gender ## Str?

    var list = MyList[Int, 5]()

    # cascading type assertion
    ## ~ T.Element(<:Equatable, ::U.Element)
    fun anyCommonElements(lhs, rhs): ## |T, U|<:Sequence
        for litem in lhs, ritem in rhs:
            return true if litem == ritem
        return false

    # type reference
    ## MyList, Int -> T ~ T::MyList.T
    fun getItem(list, index):
        list[index]

    let identifier ## T<:Identfiable

    var pegasus ## U<:Horse&Bird

    # using the type of another variable
    var x = 10
    type T = x'
    var y ## T

    # unsure types
    ## !T, !U -> T # T and U could be the same types
    fun max(a, b):
        if a > b: return a
        else: return b

    ## T, $ -> $ ~ T<:Array
    fun map(array, f):
        (f(i) | i in array)

    # generics symbols
    # $X, !X, X.Y, ~, x(), Int?, T::U&V, |T,U|::X, T(<:X,::Y)

TYPE OBJECTS
    Int <: Integer # true

    type Name = Str

    # distinct types vs aliases
    type Meter = val Int
    Meter(5) == Int(5) # false

    type Meter = ref Int
    Meter(5) == Int(5) # true

    fun haveSameTypes(x, y):
        x && y :: x'
    ..

OPERATOR OVERLOADING
    # operators are special characters: +-/\&|=%$

    # you can call an operator like a function
    print(+(56, 5))

    # you can create a custom operator
    fun ++(x): #@ postfix(5)
        var t = val x
        x += 1
        t

    # certain special operators like [] and {} that can be overloaded with special names
    fun _getIndex(myList, index): ## MyList, Signed
        return myList.items[index]

    # you can make a function infix but not prefix or postfix
    fun in(x, y): #@ infixl(5, left)
        for a in y:
            return true if a == x
        false

    # special characters such as $=-/& etc., can be combined in deffernt ways to form new operators
    # but reserved characters like .,`@(){}[] can't

TRY, CATCH & ENSURE
    if numerator && denominator == 0:
        raise DomainErr() # raising an exception.

    try:
        process bigData
    catch err: ## DataCorruptionErr
        print err.msg
    ensure:
        restore bigData

    # a catch body can be ommitted if it's only printing the error's message
    try:
        process bigData
    catch err ## DataCorruptionErr

    # managed resource
    try process(bigData):
    catch err: ## DivByZeroError
        print err.msg

    # managed resource with assignment
    try data <- getData():
        use data

    # there is also defer block which can be put at the end of
    # a block for clean up
    defer: file.close()

    try, while line <- file.readNextLine():
        print line
    catch err: ## FileNotFounError
        print 'File missing!'

    # error coalescing
    let x = getValue() !! 78

SYMBOL
    # symnols are passed around as is until they are evaluated
    let symbol = (:2 + 3)
    let addition = (:name + \symbol) # interpolation with '\'
    eval addition

MACROS # NEEDS REVIEW!!!
    # Astro macros are resolved at parse time.
    # macros take asts as argument

    macro sorted(string): ## NSStr
        (:SortedStr('\(string.literal)'))
    # Note: macro functions cannot use subjects from outer scope

    macro typify(abstract): ## Abst
        let els = abstract.elements
        let types = els.map |a| -> (:type $a)
        (:\abstract; \types)

    # inline macro calls
    @typify abstract Person(var name, var age)
    # block macro calls
    @loop(times:3): print 'Happy birthday to you'

OTHER LITERALS
    let fraction = 3//2 # rational numbers.

    var regex = /\d+(.\d+)?/ # regex.

    # non-standard literals
    var ns_number = 345'NGN
    var ns_str = ns.'1, 2, 3, 4'
    var ns_list = ns.[1, 2, 3, 5]
    var ns_dict = ns.{name: 'Steve', age: 24}
    let ns_tuple = ns.(name: 'Steve')

    # builtin non-standard literals
    var raw_string = r."\t represnts a tab character"

BASIC ARITHMETIC OPERATIONS
    let t = 2 + 2'f
    let u = 5'f * 20(5 - 6) # => 5 * f + 20 * (5 - 6)
    var v = 5 / 2 # this returns an F64 even though the operands are Ints
    var w = 5 // 2 # this returns a clamped Int result of the above
    var x = 1 - 1'i # complex numbers.
    let y = 5'i16²
    let z = i8 √25

MATH EXPRESSION
    let x = 3f + 2(6 + 1)
    let y = 3 * f + 2 * (6 + 1) # same as above
    let u = (2 + 1)3

    # gotchas
    let t = f(3) + 56 # f will be seen as a function call here
    let h = 0x344f # f is not considered a variable here, but part of the hex literal
    let x = (0x344)f # allowed


INTEGER BITWISE FUNCTIONS
    var x = 2 | 5 # and
    var y = 3 & 6 # or
    var z = ~6  # not
    var a = ^7  # exclusive or
    var b = a << b # bit shift left
    var c = b >> a # bit shift right

UNICODE SUPPORT
    numbers.map|x| -> x²

    if ade.mood != happy:
        ade.mood == ☹

POINTERS
    # getting pointer to a primitive address
    let num = 55
    let pointer = ptr(num)

    # setting address value
    pointer.value = 57
    print pointer

    # set pointer address
    pointer.set 0xFFFE

    # allocating memory on the heap
    let memory = malloc[Byte](5)

    # free pointer memory
    free([memory, pointer]) # throws compile time error if dangling pointer or aliasing problem is detected


PREDEFINED TYPE HIERARCHY
    Type
    Any
        Fun
        Scalar
            Real
                Integer
                    Signed
                        Int, I8, I16, I32, I64,
                    Unsigned
                        UInt, U8, U16, U32, U64
                Float
                    F32, F64
            Complex
                Cmp64, Cmp128
            Bool
        Sequence
            Array/List
            Range
            Indexer
            Tuple
            NamedTuple
            Dict
            Generator


Non-MVP
- Macros
- custom operator overloading
- Unicode support
- Package manager
- Concurrency - coroutines, async and actors
- Exceptions
- Complete Standard Library
- Complete Generics

FEATURE SUMMARY
- Near-native speed.
- Expressive Python-like syntax.
- Automatic memory management with no runtime cost.
- Human-readable compilation errors.
- Fault-tolerant concurrency and reactive programming.
- Completely optional type annotations.
- Simpler but flexible object-oriented model.
- Extensive mathematical function library.
- Built-in package manager.
- Incremental Compilation
- Unicode support including but not limited to UTF-8.
- Compiles to webassembly with easy inter-op with Javascript.
- Free and open source.
- Powerful type sytem with multiple dispatch, variance and generics.
- Metaprogramming with hygenic macros and operator overloading.

WHERE USED
{} - dictvalueaccess, dictliteral, setliteral, setpatternmatching, dictpatternmatching, properties
[] - listliteral, range, typevar, listpatternmatching
() - tupleliteral, tuplepatternmatching
_ - patternmatching, partialapplication, anonymousname, unknowntypesig, importall, donothing, initialisertrain
. - dotnotation
~ - match
| - match, lambda, typeunion, listcomprehension
& - typeintersection
|| - chainedor
&& - chainedand
<- - ifassignment, whileassignment, tryassignment
-> - returntypearrow, lambdas, match
@ - macrosignature
` - exported
in - forloop
as - contravariantconversion
at - labeljump
$ - generictypedecl, unnamedargs, outersubjects, stringinterpolation
.. - blockend, constrainedvalue, cascadingnotation
... - rest, unpack, linecontinuation
: - blockstart, range, importalias
_ (post) - nonstandardliteral
, NL - list, dict, enums, chainedconditionalconstruct

ASTRO STRUCTURED LANGUAGE PROPOSAL
dsl(src: 'hello', link: 'bar.io'):
    list: [1, 2, 3]

POSSIBLE COMPILER DEPENDENCIES
- libffi    (foreign function interface)
- gmp       (bignum)
- libprce   (regex)

OPTIONAL COMPILE-TIME GUARANTEES
- Exception Tracking
- Index Bound Tracking
- Compile Time Type Checking Guarantee
    - for some language constructs like assignment destructuring
    var a, b = ...list

WHAT IF
- Astro had Braces
    var sum = 5 + 6

    for i in stream() if i > 25 {
        print i
    }

    fun fib(n) {
        | 2 -> 1
        | _ -> {
            fib(n - 1) + fib(n - 2)
        }
    }

    var age = { age += 5 } -> { age -= 5 }

    list.foldl(0) |a, b| { a + b }

    list.foldl(0) |a, b| {
        a + b
    }

    var students = getStudentList()
    students.:gpa.map |gpa| { gpa + 1 }

    20.each { print "Hello" }

    # no multiple declaration syntaz

    type Employee <: Person {
        var job ## Str
    }

    dsl(src: 'hello', link: 'bar.io') {
        list { [1, 2, 3] }
    }

BLOCKS
    :  block_expr
    -> block_expr
    (  block_expr)
    {  block_expr}
    [  block_expr]

INSTANCE TYPES
    add(3, 4) :: Fun:(Int, Int) -> Int
    car :: Car:{Str, Str, Int}

INTEROP PRTOPOSAL WITH JS AND PYTHON
    @pyimport math
    math.sin(45)
    math.cos(70)

    @jsimport express
    let app = express()
    let router = app.Router()
