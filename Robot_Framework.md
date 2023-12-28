# Robot Framework
Robot Framework is an open source test automation framework for acceptance testing and acceptance test-driven development.

It follows different test case styles − keyword-driven, behaviour-driven and data-driven for writing test cases.

Robot Framework provides support for external libraries, tools which are open source and can be used for automation.

Test cases are written .robot or .txt files using keyword style in a tabular format.

Robot framework works fine on all the Operating Systems available.

The framework is built on Python and runs on Jython (JVM) and IronPython (.NET).

## Robot Framework Sections
The four sections of basic Robot test scripts are setting, variables, keywords, and test cases.

### 1. Settings

The settings section contains the import statements for the external libraries, resources, and the setup and teardown commands.

```
*** Settings ***
Library           SeleniumLibrary
Suite Setup       Open Browser    ${URL}    ${BROWSER}
Suite Teardown    Close All Browsers
Resource		  CommonKeywords.robot
```

The Settings section includes the SeleniumLibrary and sets up the Suite Setup and Teardown, as well as importing an external resource file.

**Libraries:**<br>
Robot framework has support for a lot of external libraries like SeleniumLibrary, Database Library, FTP Library and http library.<br>
Robot framework also has its own built-in libraries for strings, date, numbers etc.

**Resources:**<br>
Robot framework also allows the import of robot files with keywords externally to be used with test cases.

### 2. Variables

The variables section contains keyword arguments, global, and local values. Variables make it easy to change the test data in one location.

Robot framework supports variables – scalar, list and dict.

```
*** Variables ***
${URL}        http://example.com
${BROWSER}    Chrome
```

1. **Scalar** (Identifier: **`$`**) – The most common way to use variables in Robot Framework test data is using the scalar variable syntax like **`${var}`**. When this syntax is used, the variable name is replaced with its value as-is.

2. **List** (Identifier: **`@`**) – If a variable value is a list or list-like, a list variable like **`@{EXAMPLE}`** is used. In this case, the list is expanded and individual items are passed in as separate arguments.

3. **Dictionary** (Identifier: **`&`**) – A variable containing a Python dictionary or a dictionary-like object can be used as a dictionary variable like **`&{EXAMPLE}`**. In practice, this means that the dictionary is expanded and individual items are passed as named arguments to the keyword.

4. **Environment** (Identifier: **`%`**) – Robot Framework allows using environment variables in the test data using the syntax **`%{ENV_VAR_NAME}`**. For example: **`%{PATH}`**. They are limited to string values.

More Topics:<br>
[Assigning variables](https://robocorp.com/docs/languages-and-frameworks/robot-framework/variables#assigning-variables)<br>
[Built-in variables](https://robocorp.com/docs/languages-and-frameworks/robot-framework/variables#built-in-variables)<br>
[Runtime variables](https://robocorp.com/docs/languages-and-frameworks/robot-framework/variables#runtime-variables)<br>
[Environment variables available during a Control Room run](https://robocorp.com/docs/languages-and-frameworks/robot-framework/variables#environment-variables-available-during-a-control-room-run)

### 3. Keywords

The keywords section contains operations used to execute the tests.

The different types of keywords are built-in keywords, library keywords, and user-defined keywords.

Built-in and library keywords are lower-level keywords defined by the built-in Robot Framework library or an external library such as Selenium.

User-defined keywords are keywords created by combining library keywords.

We can also pass arguments to those keywords, which make the user-defined keywords like functions that can be reused.

Keywords are organized in libraries. When you add a library, you get access to all the keywords it contains.

```
*** Keywords ***
# Keyword 1: Log in to the website
Login
    [Arguments]     ${username}    ${password}
    Input Text      id=username    ${username}
    Input Text      id=password    ${password}
    Click Button    id=loginButton
```

The Keywords section includes the `Login` keyword used in the test cases.

### 4. Test Cases

The test cases section contains the test cases. Two of the more common approaches to writing test cases are keyword-driven and Gherkin.

**Keyword-driven Approach:**

The keyword-driven approach uses the Robot Framework built-in keywords and keywords from an external library like Selenium.

```
***Test Cases***
Logo is Visible
    Open Browser                 https://www.example.com chrome
	Element Should Be Visible    <WebElement>
```

**Gherkin Approach:**

The Gherkin approach uses Gherkin syntax. Gherkin is a business readable language used in behavior-driven development (BDD).

Robot Framework is case-insensitive. The keywords Close Browser and close browser are the same command

Robot framework supports keyword driven style test cases and data driven style.

**Data Driven Test Cases:**

Data driven works with high-level keyword used as a template to the test suite and the test cases are used to share data with the high-level keyword defined in the template. It makes the work very easy for testing UI with different inputs.

```
*** Test Cases ***
Secenario: Logo visible on home page
    Given   I open the browser
    And     I enter the home page URL
    When    I press enter
    Then    The home page loads
    And     The logo is visible
```

### 5. Test Case Tagging

Robot framework allows tagging to categorize and organize test cases. 

Tags are helpful for selectively running or excluding specific test cases based on criteria such as functionality, priority, or environment

Test cases are tagged with **[Tags]** in square brackets, specifying one or more tags for each test case. Tags are space-separated.

```
*** Settings ***
Library    SeleniumLibrary

*** Test Cases ***
Login with Valid Credentials
    [Tags]    Smoke    Regression
    Input Text    id=username    testuser
    Input Text    id=password    testpassword
    Click Button    id=loginButton
    Page Should Contain    Welcome, testuser

Search Functionality
    [Tags]    Smoke
    Input Text    id=searchBox    Robot Framework
    Click Button    id=searchButton
    Page Should Contain    Results for "Robot Framework"

Logout
    [Tags]    Regression
    Click Link    Logout
    Page Should Contain    Log in

*** Test Suites ***
Smoke Tests
    [Tags]    Smoke
    Login with Valid Credentials
    Search Functionality

Regression Tests
    [Tags]    Regression
    Login with Valid Credentials
    Logout
```

In this example:
- The **Smoke Tests** and **Regression Tests** test suites include specific test cases based on their tags.
- You can run all tests with a specific tag using the `--include` command-line option, e.g., `robot --include Smoke`.
- Tags can be used to group test cases based on different criteria, such as functionality (`Smoke`, `Regression`), feature (`Login`, `Search`), or priority (`High`, `Low`).
- Tags can be assigned at the test case or suite level.
- You can also use the --exclude option to exclude specific tags from execution.

Tags are helpful for filtering and organizing test cases, making it easier to run specific groups of tests during different phases of testing.

### Tags Usage Cases:

**Only execute Test cases tagged with 'Smoke' and 'Sanity' Tags:**

	robot -d Results -i Smoke -i Sanity test_suite.robot

**Run all test cases except the ones with the 'Smoke' Tag:**

	robot -d Results -e Smoke test_suite.robot

**Another way to apply tags to the test cases is by using Force Tags in the Settings section:**

Syntax: `Force Tags  Smoke`

```
*** Settings ***
Documentation  Tags in Robot Framework
Force Tags  Smoke

*** Variables ***

*** Test Cases ***
Test Case 1
    [tags]  Smoke
    Log To Console  This is test case 1
```

### 6. Reports and Logs

Robot framework provides all the details of test suite, test case execution in the form of report and logs. All the execution details of the test case are available in the log file. The details like whether the test case has failed or passed, time taken for execution, steps followed to run the test case are provided.

### 7. Documentation

You can add details about your test case so that it becomes easy for future reference.
```
*** Settings ***
Library          SeleniumLibrary
Documentation    My First Test
```

Multi-line comments:
```
*** Settings ***
Library    SeleniumLibrary
Documentation
...    My First Test
...    This test will Verify Google
```

### Sample Robot Script
```
*** Settings ***

Library    SeleniumLibrary
Documentation
...    My First Test
...    This test will Verify Google

*** Keywords ***

Navigate To Google
    Open Browser    https://www.google.com    browser=chrome

Verify Page Contains Google
    ${Get_title}=                 Get Title
    Should Be Equal As Strings    ${Get_title}    Google
    Close Browser

*** Test Cases ***

Open Google & Verify Google
    Navigate To Google           # Keyword 1
    Verify Page Contains Google  # Keyword 2
```

## Project Structure
A typical Robot Framework project has the following file structure:
```
Project_Root
    ├── Library    # Contains custom keyword libraries
    ├── Resource   # Contains the reusable Robot code files
    ├── Results    # Contains the executed test results
    └── Tests      # Contains the Robot test scripts
```
### RIDE
This editor available with Robot framework helps in writing and running test cases.

### Robot Framework Advantages
Robot framework is open source, so anyone who wants to try out can easily do so.

### Robot Framework Limitations
Robot lacks support for if-else, nested loops, which are required when the code gets complex.

# Running Tests
Test cases are executed with the robot command:
```
robot test_suite.robot
robot --name Robot --loglevel DEBUG test_suite.robot
robot -d Results test_suite.robot # Save the reports in the 'Results' folder
```

# Robot Framework cheat sheet
**[Robot Framework 5 syntax recipes cheat sheet robot:](https://robocorp.com/docs/languages-and-frameworks/robot-framework/cheat-sheet#robot-framework-5-syntax-recipes-cheat-sheet-robot)**
```
*** Settings ***
Documentation       Robot Framework 5 syntax recipes cheat sheet robot.
...                 Demonstrates Robot Framework syntax in a concise format.

Library             MyLibrary
Library             MyLibrary    WITH NAME    HelloLibrary
Library             MyLibrary    greeting=Howdy!    WITH NAME    HowdyLibrary
Resource            keywords.robot
Variables           variables.py

Suite Setup         Log    Suite Setup!
Suite Teardown      Log    Suite Teardown!
Task Setup          Log    Task Setup!
Task Teardown       Log    Task Teardown!
Task Timeout        2 minutes


*** Variables ***
${STRING}=                  cat
${NUMBER}=                  ${1}
@{LIST}=                    one    two    three
&{DICTIONARY}=              string=${STRING}    number=${NUMBER}    list=@{LIST}
${ENVIRONMENT_VARIABLE}=    %{PATH=Default value}


*** Tasks ***
Call keywords with a varying number of arguments
    A keyword without arguments
    A keyword with a required argument    Argument
    A keyword with a required argument    argument=Argument
    A keyword with an optional argument
    A keyword with an optional argument    Argument
    A keyword with an optional argument    argument=Argument
    A keyword with any number of arguments
    A keyword with any number of arguments    arg1    arg2    arg3    arg4    arg5
    A keyword with one or more arguments    arg1
    A keyword with one or more arguments    arg1    arg2    arg3

Call a keyword that returns a value
    ${value}=    A keyword that returns a value
    Log    ${value}    # Return value

Do conditional IF - ELSE IF - ELSE execution
    IF    ${NUMBER} > 1
        Log    Greater than one.
    ELSE IF    "${STRING}" == "dog"
        Log    It's a dog!
    ELSE
        Log    Probably a cat.
    END

Loop a list
    Log    ${LIST}    # ['one', 'two', 'three']
    FOR    ${item}    IN    @{LIST}
        Log    ${item}    # one, two, three
    END
    FOR    ${item}    IN    one    two    three
        Log    ${item}    # one, two, three
    END

Loop a dictionary
    Log    ${DICTIONARY}
    # {'string': 'cat', 'number': 1, 'list': ['one', 'two', 'three']}
    FOR    ${key_value_tuple}    IN    &{DICTIONARY}
        Log    ${key_value_tuple}
        # ('string', 'cat'), ('number', 1), ('list', ['one', 'two', 'three'])
    END
    FOR    ${key}    IN    @{DICTIONARY}
        Log    ${key}=${DICTIONARY}[${key}]
        # string=cat, number=1, list=['one', 'two', 'three']
    END

Loop a range from 0 to end index
    FOR    ${index}    IN RANGE    10
        Log    ${index}    # 0-9
    END

Loop a range from start to end index
    FOR    ${index}    IN RANGE    1    10
        Log    ${index}    # 1-9
    END

Loop a range from start to end index with steps
    FOR    ${index}    IN RANGE    0    10    2
        Log    ${index}    # 0, 2, 4, 6, 8
    END

Nest loops
    @{alphabets}=    Create List    a    b    c
    Log    ${alphabets}    # ['a', 'b', 'c']
    @{numbers}=    Create List    ${1}    ${2}    ${3}
    Log    ${numbers}    # [1, 2, 3]
    FOR    ${alphabet}    IN    @{alphabets}
        FOR    ${number}    IN    @{numbers}
            Log    ${alphabet}${number}
            # a1, a2, a3, b1, b2, b3, c1, c2, c3
        END
    END

Exit a loop on condition
    FOR    ${i}    IN RANGE    5
        IF    ${i} == 2    BREAK
        Log    ${i}    # 0, 1
    END

Continue a loop from the next iteration on condition
    FOR    ${i}    IN RANGE    3
        IF    ${i} == 1    CONTINUE
        Log    ${i}    # 0, 2
    END

Create a scalar variable
    ${animal}=    Set Variable    dog
    Log    ${animal}    # dog
    Log    ${animal}[0]    # d
    Log    ${animal}[-1]    # g

Create a number variable
    ${π}=    Set Variable    ${3.14}
    Log    ${π}    # 3.14

Create a list variable
    @{animals}=    Create List    dog    cat    bear
    Log    ${animals}    # ['dog', 'cat', 'bear']
    Log    ${animals}[0]    # dog
    Log    ${animals}[-1]    # bear

Create a dictionary variable
    &{dictionary}=    Create Dictionary    key1=value1    key2=value2
    Log    ${dictionary}    # {'key1': 'value1', 'key2': 'value2'}
    Log    ${dictionary}[key1]    # value1
    Log    ${dictionary.key2}    # value2

Access the items in a sequence (list, string)
    ${string}=    Set Variable    Hello world!
    Log    ${string}[0]    # H
    Log    ${string}[:5]    # Hello
    Log    ${string}[6:]    # world!
    Log    ${string}[-1]    # !
    @{list}=    Create List    one    two    three    four    five
    Log    ${list}    # ['one', 'two', 'three', 'four', 'five']
    Log    ${list}[0:6:2]    # ['one', 'three', 'five']

Call a custom Python library
    ${greeting}=    MyLibrary.Get Greeting
    Log    ${greeting}    # Hello!
    ${greeting}=    HelloLibrary.Get Greeting
    Log    ${greeting}    # Hello!
    ${greeting}=    HowdyLibrary.Get Greeting
    Log    ${greeting}    # Howdy!

Call a keyword from a separate resource file
    My keyword in a separate resource file

Access a variable in a separate variable file
    Log    ${MY_VARIABLE_FROM_A_SEPARATE_VARIABLE_FILE}

Split arguments to multiple lines
    A keyword with any number of arguments
    ...    arg1
    ...    arg2
    ...    arg3

Log available variables
    Log Variables
    # ${/} = /
    # &{DICTIONARY} = { string=cat | number=1 | list=['one', 'two', 'three'] }
    # ${OUTPUT_DIR} = /Users/<username>/...
    # ...

Evaluate Python expressions
    ${path}=    Evaluate    os.environ.get("PATH")
    ${path}=    Set Variable    ${{os.environ.get("PATH")}}

Use special variables
    Log    ${EMPTY}    # Like the ${SPACE}, but without the space.
    Log    ${False}    # Boolean False.
    Log    ${None}    # Python None
    Log    ${null}    # Java null.
    Log    ${SPACE}    # ASCII space (\x20).
    Log    ${SPACE * 4}    # Four spaces.
    Log    "${SPACE}"    # Quoted space (" ").
    Log    ${True}    # Boolean True.

TRY / EXCEPT: Catch any exception
    TRY
        Fail
    EXCEPT
        Log    EXCEPT with no arguments catches any exception.
    END

TRY / EXCEPT: Catch an exception by exact message
    TRY
        Fail    Error message
    EXCEPT    Error message
        Log    Catches only "Error message" exceptions.
        Log    Enables error-specific exception handling.
    END

TRY / EXCEPT: Multiple EXCEPT statements
    TRY
        Fail    Error message
    EXCEPT    Another error message
        Log    Catches only "Another error message" exceptions.
    EXCEPT    Error message
        Log    Catches the "Error message" exception.
    END

TRY / EXCEPT: Multiple messages in EXCEPT statement
    TRY
        Fail    CCC
    EXCEPT    AAA    BBB    CCC
        Log    Catches any "AAA", "BBB", or "CCC" exception.
    END

TRY / EXCEPT: Catch a specific exception, or an unexpected exception
    TRY
        Fail    Error message
    EXCEPT    Another message
        Log    Catches only "Another message" exceptions.
    EXCEPT
        Log    Catches any exception.
        Log    Useful for handling unexpected exceptions.
    END

TRY / EXCEPT: Catch exceptions where the message starts with
    TRY
        Fail    A long error message with lots of details
    EXCEPT    A long error message    type=start
        Log    Matches the start of an error message.
    END

TRY / EXCEPT: Capture the error message
    TRY
        Fail    Goodbye, world!
    EXCEPT    AS    ${error_message}
        Log    ${error_message}    # Goodbye, world!
    END

TRY / EXCEPT: Using ELSE when no exceptions occured
    TRY
        Log    All good!
    EXCEPT    Error message
        Log    An error occured.
    ELSE
        Log    No error occured.
    END

TRY / EXCEPT / FINALLY: Always execute code no matter if exceptions or not
    TRY
        Log    All good!
    FINALLY
        Log    FINALLY is always executed.
    END
    TRY
        Fail    Catastrophic failure!
    EXCEPT
        Log    Catches any exception.
    FINALLY
        Log    FINALLY is always executed.
    END

TRY / EXCEPT / ELSE / FINALLY: All together!
    TRY
        Fail    Error message
    EXCEPT
        Log    Executed if any exception occurs.
    ELSE
        Log    Executed if no exceptions occur.
    FINALLY
        Log    FINALLY is always executed.
    END

TRY / EXCEPT: Glob pattern matching
    TRY
        Fail    My error: 99 occured
    EXCEPT    My error: *    type=glob
        Log    Catches by glob pattern matching.
    END

TRY / EXCEPT: Regular expression matching
    TRY
        Fail    error 99 occured
    EXCEPT    [Ee]rror \\d+ occured    type=regexp
        Log    Catches by regular expression pattern matching.
    END

WHILE: Loop while the default limit (10000) is hit
    TRY
        WHILE    True
            Log    Executed until the default loop limit (10000) is hit.
        END
    EXCEPT    WHILE loop was aborted    type=start
        Log    The loop did not finish within the limit.
    END

WHILE: Loop while the given limit is hit
    TRY
        WHILE    True    limit=10
            Log    Executed until the given loop limit (10) is hit.
        END
    EXCEPT    WHILE loop was aborted    type=start
        Log    The loop did not finish within the limit.
    END

WHILE: Loop while condition evaluates to True
    ${x}=    Set Variable    ${0}
    WHILE    ${x} < 3
        Log    Executed as long as the condition is True.
        ${x}=    Evaluate    ${x} + 1
    END

WHILE: Skip a loop iteration with CONTINUE
    ${x}=    Set Variable    ${0}
    WHILE    ${x} < 3
        ${x}=    Evaluate    ${x} + 1
        #    Skip this iteration.
        IF    ${x} == 2    CONTINUE
        Log    x = ${x}    # x = 1, x = 3
    END

WHILE: Exit loop with BREAK
    WHILE    True
        BREAK
        Log    This will not be logged.
    END


*** Keywords ***
A keyword without arguments
    Log    No arguments.

A keyword with a required argument
    [Arguments]    ${argument}
    Log    Required argument: ${argument}

A keyword with an optional argument
    [Arguments]    ${argument}=Default value
    Log    Optional argument: ${argument}

A keyword with any number of arguments
    [Arguments]    @{varargs}
    Log    Any number of arguments: @{varargs}

A keyword with one or more arguments
    [Arguments]    ${argument}    @{varargs}
    Log    One or more arguments: ${argument} @{varargs}

A keyword that returns a value
    RETURN    Return value

RETURN: Return a value from a keyword
    IF    True    RETURN    It is true!    ELSE    RETURN    It is not true!

RETURN: Return without a value
    IF    True    RETURN
    Log    This will not be logged.

A keyword with documentation
    [Documentation]    This is keyword documentation.
    No Operation
```