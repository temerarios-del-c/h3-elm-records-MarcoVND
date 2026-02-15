# AGENTS.md - Elm Records Learning Project

This is an educational Elm project focused on teaching functional programming concepts through exercises on functions, records, type aliases, and HTML generation.

## Development Commands

### Essential Commands
```bash
# Validate code formatting (strict requirement)
elm-format src/ --validate

# Run static analysis with type annotation enforcement
elm-review --template jfmengels/elm-review-common/example --rules NoMissingTypeAnnotation,NoMissingTypeAnnotationInLetIn

# Build the project (compiles all source files)
elm make src/*

# Run all tests
elm-test

# Run a single test by function name
elm-test -f "add2Test"
elm-test -f "languageNamesTest"
elm-test -f "onlyStudentsTest"
elm-test -f "getVideogameGenresTest"
elm-test -f "htmlTest"

# Run tests matching a pattern
elm-test -f ".*Test"  # Run all test suites
elm-test --watch      # Watch mode for continuous testing
```

### Development Workflow Order
1. Write/modify code with type annotations
2. Validate formatting: `elm-format src/ --validate`
3. Run checks: `elm-review --template jfmengels/elm-review-common/example --rules NoMissingTypeAnnotation,NoMissingTypeAnnotationInLetIn`
4. Build: `elm make src/*`
5. Test: `elm-test`

## Code Style Guidelines

### Type Annotations (Mandatory)
- **All functions must have explicit type annotations** (enforced by elm-review)
- Top-level functions and let-in functions both require annotations
- Use descriptive parameter names that indicate purpose

```elm
-- ✅ Good
add2 : Int -> Int -> Int
add2 firstNumber secondNumber =
    firstNumber + secondNumber

-- ❌ Bad (missing type annotation)
add2 firstNumber secondNumber =
    firstNumber + secondNumber

-- ✅ Good (let-in with annotation)
calculateResult : Int -> Int -> Int
calculateResult x y =
    let
        multiply : Int -> Int -> Int
        multiply a b =
            a * b
    in
    multiply x y
```

### Formatting
- Use 4-space indentation (enforced by .editorconfig)
- Apply elm-format before committing
- Function parameters: one per line for complex signatures, same line for simple ones

### Naming Conventions
- **Functions and variables**: camelCase starting with lowercase
  - `add2`, `languageNames`, `onlyStudents`, `getVideogameGenres`
- **Types and type aliases**: PascalCase starting with uppercase
  - `Videogame`, `Computer`
- **Constants**: camelCase with descriptive names
- **Test functions**: append "Test" to function name
  - `add2Test`, `languageNamesTest`

### Import Organization
```elm
module Helper exposing (..)

import Expect exposing (Expectation)
import Fuzz exposing (Fuzzer, floatRange, int, list, string)
import Html exposing (Html, div, h1, li, text, ul)
import Test exposing (..)
import Test.Html.Query as Query
import Test.Html.Selector exposing (containing, exactText, tag, text)
```

### Record Definitions
- Use explicit field types in record definitions
- For type aliases, use PascalCase for the alias name
- Create fuzzers for records used in property tests

```elm
-- ✅ Good (record type)
type alias Language =
    { name : String
    , releaseYear : Int
    , currentVersion : String
    }

-- ✅ Good (type alias usage)
languageFuzzer : Fuzzer Language
languageFuzzer =
    Fuzz.map3 Language
        Fuzz.string
        Fuzz.int
        Fuzz.string
```

### Function Structure
- Keep functions pure and side-effect free
- Use pattern matching for complex conditional logic
- Prefer function composition over complex nested logic
- Use pipe operator `|>` for data transformation pipelines

### Error Handling
- Use Elm's built-in Maybe and Result types
- Avoid partial functions that could crash
- Use case expressions for comprehensive handling

## Testing Guidelines

### Test Structure
```elm
functionNameTest : Test
functionNameTest =
    describe "Testing functionName function"
        [ test "descriptive test case" <|
            \_ ->
                functionName input
                    |> Expect.equal expectedOutput
        , fuzz (appropriateFuzzer) "property-based test description" <|
            \fuzzInput ->
                functionName fuzzInput
                    |> Expect.equal (expectedProperty fuzzInput)
        ]
```

### Fuzzer Patterns
- Create specific fuzzers for custom record types
- Use appropriate ranges for numeric fuzzers
- Use `Fuzz.oneOf` for enum-like strings
- Use `Fuzz.map` functions to construct complex types

### HTML Testing
- Use Test.Html.Query for HTML structure validation
- Use `tag`, `containing`, `exactText` selectors appropriately
- Test both structure and content separately
- Build selectors incrementally for complex structures

## Project-Specific Patterns

### Educational Exercise Structure
This project follows a progression from simple functions to complex HTML generation:
1. Basic arithmetic functions (add2, add3, calc)
2. Record manipulation (languageNames, onlyStudents)
3. Type aliases (getVideogameGenres with Videogame alias)
4. HTML generation (main function with Computer record)

### Function Implementation Style
- Focus on readability over cleverness
- Use explicit record field access with dot notation
- Maintain consistency with exercise examples
- Avoid overly complex one-liners in educational context

### Dependencies
This project uses minimal, core dependencies:
- `elm/core` - basic language features
- `elm/html` - HTML generation
- `elm/browser` - basic browser integration
- `elm-explorations/test` - testing framework

## Common Pitfalls to Avoid
- Missing type annotations (will fail elm-review)
- Incorrect indentation (will fail elm-format)
- Using `//` operator with non-Int types
- Forgetting to expose functions in module declaration
- Testing HTML structure incorrectly (use Query.fromHtml)