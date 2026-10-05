# Quiz CLI

An interactive command-line quiz game for learning JavaScript, Node.js, and general programming concepts.

The application presents multiple-choice questions, provides immediate feedback and explanations, tracks the user's score, and displays a final performance summary with an incorrect-answer review.

> **Current repository note:** The inspected `main` branch contains a path/layout mismatch. `index.js` imports modules from `src/` and loads quiz data from `data/questions.json`, but the repository currently stores those files at the project root. The application may require a small path correction before it can be started successfully. See [Known Path Issue](#known-path-issue).

## Key Features

- Interactive terminal-based quiz experience
- Three quiz categories:
  - JavaScript Basics
  - Node.js Fundamentals
  - General Programming
- Select all questions, three questions, or five questions
- Multiple-choice questions with numbered options
- Randomized question order using the Fisher–Yates shuffle algorithm
- Immediate correctness feedback
- Explanations for questions
- Visual progress bar
- Score and percentage calculation
- Performance messages based on the final score
- Review of incorrectly answered questions
- Option to play again
- Terminal colors using ANSI escape codes
- No third-party runtime dependencies

## Technology Stack

- **Language:** JavaScript
- **Runtime:** Node.js 18 or newer
- **Module system:** ECMAScript modules
- **Input handling:** Node.js built-in `readline` module
- **File access:** Node.js `fs/promises` API
- **Terminal styling:** ANSI escape codes
- **Testing:** Node.js built-in test runner
- **Package manager:** npm

## Prerequisites

Install the following software before running the project:

- Node.js version 18 or newer
- npm, typically included with Node.js
- An interactive terminal

Check your installed versions:

```bash
node --version
npm --version
```

## Project Structure

The repository currently contains all files at the root level:

```text
test-app/
├── colors.js
├── index.js
├── input.js
├── package.json
├── questions.json
└── quiz.js
```

### File descriptions

| File | Description |
|---|---|
| `index.js` | Main CLI entry point. Loads quiz data, manages the application flow, and coordinates input and quiz functionality. |
| `input.js` | Provides Promise-based terminal input helpers built on Node.js `readline`. |
| `quiz.js` | Contains the `Quiz` class, question state management, scoring, progress tracking, and result display logic. |
| `colors.js` | Provides ANSI terminal color and formatting helpers without external dependencies. |
| `questions.json` | Static quiz data containing three categories and 15 total questions. |
| `package.json` | Project metadata, npm scripts, module configuration, runtime requirement, and license information. |

## Setup Instructions

Clone or download the repository, then enter the project directory:

```bash
cd test-app
```

Install the project dependencies:

```bash
npm install
```

The project does not declare any runtime or development dependencies, so this command does not install third-party application packages. It can still be used to initialize the standard npm project setup.

Before starting the application, review the [Known Path Issue](#known-path-issue).

## Configuration

The project has no environment-specific configuration.

There are:

- No `.env` files
- No environment variables
- No database configuration
- No external service configuration
- No separate build configuration
- No TypeScript configuration
- No linting or formatting configuration

Quiz content is stored in:

```text
questions.json
```

The data contains the following top-level categories:

- `javascript`
- `nodejs`
- `general`

Each category includes a display name and a collection of questions. Each question contains:

- `question`
- `options`
- `answer`
- `explanation`

## Known Path Issue

The current source layout does not match the paths referenced by `index.js`.

### Files currently present

```text
colors.js
input.js
questions.json
quiz.js
```

### Paths referenced by `index.js`

```text
./src/input.js
./src/quiz.js
./src/colors.js
data/questions.json
```

As a result, running the application as-is may produce module or file-not-found errors.

One of the following changes is required:

### Option 1: Update `index.js`

Change the module imports to reference the root-level files:

```js
import { createInterface, select, confirm, pressEnter } from './input.js';
import { Quiz } from './quiz.js';
import * as colors from './colors.js';
```

Update the question data path to load the root-level file:

```text
questions.json
```

### Option 2: Move files to match the existing paths

Restructure the project as follows:

```text
test-app/
├── data/
│   └── questions.json
├── src/
│   ├── colors.js
│   ├── input.js
│   └── quiz.js
├── index.js
└── package.json
```

The repository does not currently include either correction.

## How to Run

The package defines the following start command:

```bash
npm start
```

This runs:

```bash
node index.js
```

Because of the path mismatch described above, the command may not work until the imports and question data path are corrected.

The entry point also includes a Node.js executable shebang:

```bash
#!/usr/bin/env node
```

## Usage

Once the path issue has been resolved, start the quiz with:

```bash
npm start
```

The application guides you through the following flow:

1. A welcome banner is displayed.
2. Select a quiz category.
3. Select the number of questions:
   - All questions
   - Three questions
   - Five questions
4. Answer each multiple-choice question by selecting an option number.
5. View immediate correctness feedback.
6. Read the explanation when one is available.
7. View the final score and percentage.
8. Review incorrectly answered questions.
9. Choose whether to play again.

The interactive input helpers accept:

- Numbered selections for menus
- `y` or responses beginning with `y` for confirmation prompts
- Enter to continue when prompted

## Quiz Categories

The quiz contains 15 questions total, distributed across three categories.

### JavaScript Basics

Topics include:

- `const` declarations
- Adding elements to arrays with `push()`
- Strict equality using `===`
- Primitive and non-primitive values
- The behavior of `typeof null`

### Node.js Fundamentals

Topics include:

- The Node.js `fs` module
- The event loop
- `npm init`
- `process.argv`
- ES module `import` syntax

### General Programming

Topics include:

- API terminology
- Recursion
- JSON terminology
- Callback functions
- Version control

## Application Modules

### `index.js`

The main application entry point.

Responsibilities include:

- Creating the readline interface
- Loading question data
- Displaying the welcome banner
- Selecting the category
- Selecting the question count
- Creating a `Quiz` instance
- Running the question loop
- Displaying results
- Handling replay
- Handling errors
- Closing the readline interface

Errors are displayed with a colored message and stack trace. The process exits with status code `1` when an unrecoverable error occurs.

### `input.js`

Provides Promise-based wrappers around Node.js's callback-based `readline` API.

Exports:

#### `createInterface()`

Creates and returns a readline interface connected to standard input and output.

#### `prompt(rl, question)`

Prompts the user and returns a trimmed response.

#### `select(rl, question, options)`

Displays numbered options and repeats until the user enters a valid option number.

Returns an object containing:

```text
{
  index,
  value
}
```

#### `confirm(rl, question)`

Prompts for a yes/no response and returns `true` when the trimmed response begins with `y`.

#### `pressEnter(rl, message)`

Waits until the user presses Enter.

### `quiz.js`

Exports the `Quiz` class, which manages quiz state and scoring.

#### Constructor state

A `Quiz` instance tracks:

- `questions`
- `categoryName`
- `currentIndex`
- `score`
- `answers`

Questions are shuffled when the quiz is created. The shuffle operation copies the supplied array and does not mutate the original question list.

#### Getters

| Getter | Description |
|---|---|
| `currentQuestion` | Returns the current question or `null`. |
| `totalQuestions` | Returns the total number of questions. |
| `isComplete` | Indicates whether all questions have been answered. |
| `progress` | Returns the completion percentage rounded to the nearest integer. |

#### Methods

| Method | Description |
|---|---|
| `askQuestion(rl)` | Displays the current question, collects an answer, updates the score, and displays feedback. |
| `renderProgressBar()` | Returns a 30-character visual progress bar. |
| `showResults()` | Displays the category, score, percentage, performance message, and incorrect-answer review. |

#### Performance messages

| Percentage | Message |
|---:|---|
| `100%` | Perfect score |
| `80–99%` | Great job |
| `60–79%` | Good effort |
| `40–59%` | Room for improvement |
| Below `40%` | Keep practicing |

### `colors.js`

Provides terminal formatting functions using ANSI escape sequences.

Available formatting helpers include:

- `red`
- `green`
- `yellow`
- `blue`
- `cyan`
- `magenta`
- `bold`
- `dim`

Semantic helpers include:

- `success`
- `error`
- `warning`
- `info`
- `highlight`

The module does not require an external color library.

## Data Flow

```text
questions.json
    ↓
index.js loads category data
    ↓
User selects category and question count
    ↓
Quiz receives the selected questions
    ↓
Quiz shuffles the questions
    ↓
input.js collects answers
    ↓
Quiz calculates the score and stores answer history
    ↓
Quiz displays results and incorrect-answer review
```

## Testing

Run the configured test command with:

```bash
npm test
```

This invokes Node.js's built-in test runner:

```bash
node --test
```

No test files were found in the inspected repository. There is no `test/` or `tests/` directory and no `*.test.js` files, so the test command may complete without executing project-specific tests.

## Build

This project does not have a separate build or packaging step.

It is a dependency-free Node.js ES module application that is intended to run directly from source:

```bash
node index.js
```

The npm start script provides the standard equivalent:

```bash
npm start
```

## Docker

Docker configuration is not included.

The repository contains no:

- `Dockerfile`
- `docker-compose.yml`
- Container configuration

The application is intended to run locally in a terminal with Node.js installed.

## CI/CD

No CI/CD configuration was found.

The repository does not contain:

- A `.github` directory
- GitHub Actions workflow files
- Deployment configuration

## Troubleshooting

### Module not found errors

If startup reports that modules under `src/` cannot be found, verify that the files are either:

- Moved into the expected `src/` directory, or
- Referenced from their actual root-level paths in `index.js`

Expected imports for the current root-level layout are:

```js
import { createInterface, select, confirm, pressEnter } from './input.js';
import { Quiz } from './quiz.js';
import * as colors from './colors.js';
```

### Question data file not found

If the application cannot load quiz data, verify that `index.js` points to the actual location of `questions.json`.

The inspected repository stores the file at:

```text
./questions.json
```

However, the current code attempts to load:

```text
./data/questions.json
```

### Terminal colors are not displayed

The application uses ANSI escape codes directly. Terminal color support may vary depending on the terminal environment. The quiz functionality does not depend on the colors.

### The test command finds no tests

The `npm test` script is configured, but no test files were found in the inspected repository. Add Node.js test files using the supported test runner if automated coverage is needed.

## Contributing

No project-specific contribution guidelines were found in the repository.

For changes, contributors should:

1. Keep the application compatible with Node.js 18 or newer.
2. Preserve the ES module configuration.
3. Update `questions.json` consistently when adding or changing quiz content.
4. Verify the interactive CLI flow manually.
5. Run the available npm commands before submitting changes:

   ```bash
   npm test
   npm start
   ```

6. Avoid committing secrets or environment-specific files.

## License

This project is licensed under the [MIT License](https://opensource.org/licenses/MIT), as specified in `package.json`.
