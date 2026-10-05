# quiz-cli

An interactive Node.js command-line quiz game covering JavaScript, Node.js, and general programming concepts.

> **Current status:** The repository currently contains a path mismatch between `index.js` and the actual file locations. As a result, `npm start` is expected to fail until the import and data paths are corrected or the files are moved. See [Known Path Issue](#known-path-issue).

## Project Overview

`quiz-cli` is an educational terminal-based quiz application built with native Node.js APIs. It allows users to:

- Select a quiz category.
- Choose how many questions to answer.
- Answer multiple-choice questions interactively.
- View progress while playing.
- Review correct answers and explanations.
- See a final score.
- Replay the quiz.

The included question bank covers:

- JavaScript Basics
- Node.js Fundamentals
- General Programming

## Features

- Interactive command-line interface.
- Category selection.
- Configurable question count:
  - All available questions.
  - 3 questions, when available.
  - 5 questions, when available.
- Multiple-choice questions.
- Input validation for selections and confirmations.
- Fisher–Yates question shuffling.
- Score tracking.
- Progress display.
- Answer history and result review.
- Explanations for questions.
- Replay support.
- ANSI-colored terminal output.
- No third-party runtime dependencies.

## Technology Stack

- **Runtime:** Node.js 18 or later
- **Language:** JavaScript
- **Module system:** ECMAScript modules
- **Package manager:** npm
- **Dependencies:** Node.js built-in modules only
- **License:** MIT

The application uses the following built-in Node.js modules:

- `fs/promises` for loading the question data.
- `url` and `path` for resolving file paths.
- `readline` for interactive terminal input.
- Node's built-in test runner through the configured `npm test` script.

## Prerequisites

Install the following software before working with the project:

- Node.js 18 or later
- npm, normally included with Node.js
- A terminal capable of displaying ANSI color codes

Check your installed versions:

```bash
node --version
npm --version
```

## Installation

Clone the repository and switch to the `main` branch:

```bash
git clone https://github.com/KondleSanjay/test-app.git
cd test-app
git checkout main
```

Install the project metadata and dependencies:

```bash
npm install
```

The project does not declare third-party runtime dependencies, so installation should not download application libraries.

## Current Status and Known Path Issue

The repository currently stores all files at the root:

```text
colors.js
index.js
input.js
package.json
questions.json
quiz.js
```

However, `index.js` currently expects the following paths:

```text
./src/input.js
./src/quiz.js
./src/colors.js
data/questions.json
```

Those `src/` and `data/` paths do not currently exist in the repository. Therefore, the intended start command is expected to fail with a module or file-not-found error until the path mismatch is resolved.

### Suggested fixes

One of the following changes is required:

1. Update the imports and question-data path in `index.js` to reference the root-level files:

   ```text
   ./input.js
   ./quiz.js
   ./colors.js
   ./questions.json
   ```

2. Or reorganize the repository to match the paths currently used by `index.js`:

   ```text
   src/
   ├── colors.js
   ├── input.js
   └── quiz.js

   data/
   └── questions.json
   ```

This README does not modify the application code. Until one of these approaches is applied, the game should be considered not runnable from the current repository layout.

## How to Run

After resolving the known path issue, start the application with:

```bash
npm start
```

The `npm start` script runs:

```bash
node index.js
```

You can also run the entry point directly:

```bash
node index.js
```

## Intended Usage

The intended game flow is:

1. Start the application.
2. Read the displayed banner and category list.
3. Select a category:
   - JavaScript Basics
   - Node.js Fundamentals
   - General Programming
4. Select the number of questions:
   - All available questions
   - 3 questions
   - 5 questions
5. Answer each multiple-choice question.
6. View progress during the quiz.
7. Review the final score and answer explanations.
8. Choose whether to play again.
9. Exit when finished.

### Example Session

The exact visual formatting depends on terminal ANSI support, but the interaction is conceptually similar to:

```text
Welcome to quiz-cli!

Choose a category:
1. JavaScript Basics
2. Node.js Fundamentals
3. General Programming

Select a category: 1

How many questions would you like?
1. All
2. 3
3. 5

Select an option: 2

Question 1 of 3
What is ...?

1. Option A
2. Option B
3. Option C
4. Option D

Your answer: 2

Progress: 1/3
...

Quiz complete!
Score: 2/3

Review:
- Correct answer: ...
- Explanation: ...

Play again? (y/n):
```

The displayed questions, options, answers, and explanations come from `questions.json`.

## Project Structure

```text
.
├── colors.js       # ANSI color and styling helpers
├── index.js        # Application entry point and game orchestration
├── input.js        # Interactive terminal input and validation helpers
├── package.json     # Project metadata and npm scripts
├── questions.json   # Quiz categories and question bank
└── quiz.js         # Quiz engine, scoring, shuffling, and result handling
```

### Module Overview

#### `index.js`

The main application entry point. It is responsible for:

- Loading the question data.
- Displaying the application banner and categories.
- Asking the user to select a category.
- Asking how many questions to play.
- Creating and running the quiz.
- Displaying the score and review.
- Supporting replay.
- Handling errors.
- Closing the readline interface when the application exits.

#### `input.js`

Provides interactive input utilities built on Node.js `readline`, including:

- `createInterface`
- `prompt`
- `select`
- `confirm`
- `pressEnter`

These helpers provide validation and reusable terminal interaction behavior.

#### `quiz.js`

Exports the `Quiz` class, which handles the core game logic:

- Shuffling questions with the Fisher–Yates algorithm.
- Tracking the current question.
- Tracking the score.
- Recording answer history.
- Rendering progress.
- Asking questions.
- Displaying results and explanations.

#### `colors.js`

Provides ANSI styling helpers used to format terminal output with colors and other visual emphasis.

#### `questions.json`

Contains the quiz categories and their questions. The current data includes five questions in each category.

## Question Data Format

The question bank is organized by category. Each category includes a display name and a list of questions.

Conceptually, the structure is:

```json
{
  "categories": {
    "javascript": {
      "name": "JavaScript Basics",
      "questions": [
        {
          "question": "Question text",
          "options": [
            "First option",
            "Second option",
            "Third option",
            "Fourth option"
          ],
          "answer": 0,
          "explanation": "Explanation of the correct answer."
        }
      ]
    }
  }
}
```

Each question contains:

| Field | Description |
|---|---|
| `question` | The question displayed to the player. |
| `options` | An array of possible answers. |
| `answer` | The zero-based index of the correct option. `0` refers to the first option. |
| `explanation` | Explanation shown during the result review. |

The current categories are:

| Key | Display name | Questions |
|---|---|---:|
| `javascript` | JavaScript Basics | 5 |
| `nodejs` | Node.js Fundamentals | 5 |
| `general` | General Programming | 5 |

When adding questions, ensure that:

- `answer` is a valid zero-based index.
- Every question has at least one option.
- The explanation accurately describes the correct answer.
- The JSON remains valid.

## Configuration

The application does not use environment variables or an environment template.

Configuration is primarily stored in:

- `package.json` for project metadata and commands.
- `questions.json` for quiz categories and content.

No passwords, API keys, tokens, databases, or external services are required.

## NPM Scripts

The project defines the following scripts:

| Command | Description |
|---|---|
| `npm start` | Runs `node index.js`. Expected to fail currently because of the known path mismatch. |
| `npm test` | Runs Node's built-in test runner with `node --test`. |

Run the start script:

```bash
npm start
```

Run the configured test command:

```bash
npm test
```

## Testing

The project is configured to use Node.js's built-in test runner:

```bash
node --test
```

However, the repository currently contains no test files. As a result:

- There are no automated application tests to execute.
- `npm test` is configured but does not represent meaningful test coverage yet.
- Manual testing should be performed through the interactive CLI after resolving the path issue.

Recommended manual checks include:

- Starting the application.
- Selecting each category.
- Selecting all available question-count options.
- Entering valid and invalid answers.
- Confirming score and explanation output.
- Testing replay behavior.
- Exiting through the available confirmation flow.

## Build

There is no separate build or compilation step. The project runs directly in Node.js as an ES-module application.

After correcting the path mismatch, use:

```bash
npm start
```

## Architecture

The application follows a small modular architecture:

```text
index.js
   │
   ├── input.js
   │      └── readline-based prompts and validation
   │
   ├── quiz.js
   │      └── question selection, shuffling, scoring, and results
   │
   ├── colors.js
   │      └── terminal styling
   │
   └── questions.json
          └── categories, questions, answers, and explanations
```

The entry point coordinates the user experience, while `input.js` and `quiz.js` encapsulate terminal interaction and quiz behavior respectively.

## Troubleshooting

### `ERR_MODULE_NOT_FOUND` for a file under `src/`

This occurs because `index.js` imports modules from `src/`, but the files currently exist at the repository root.

Fix the imports in `index.js` or move the modules into a `src/` directory.

### Question data file cannot be found

`index.js` currently expects:

```text
data/questions.json
```

The actual file is:

```text
questions.json
```

Update the data path or move the file into the expected `data/` directory.

### Colors do not display correctly

The application uses ANSI escape sequences for terminal styling. Use a modern terminal with ANSI color support. The quiz should remain functionally usable even if color rendering is limited.

### Invalid input

Use the selectable options shown by the application and enter the requested number or confirmation response. The input helpers in `input.js` provide validation for prompts, selections, and confirmations.

### `npm test` reports no tests

No test files are currently included in the repository. The test command is configured, but automated tests need to be added before it can provide meaningful coverage.

## Contributing

Contributions are welcome. To contribute:

1. Fork the repository.
2. Create a feature branch:

   ```bash
   git checkout -b feature/your-change
   ```

3. Make your changes.
4. Keep quiz data valid JSON.
5. Verify that question answer indexes are zero-based.
6. Run the available checks:

   ```bash
   npm test
   ```

7. Manually test the CLI after resolving the known path issue.
8. Commit your changes.
9. Push the branch and open a pull request.

Useful contribution areas include:

- Resolving the current path mismatch.
- Adding automated tests.
- Improving CLI accessibility and input handling.
- Adding more educational questions.
- Improving result summaries and explanations.
- Adding documentation for future question authors.

## License

This project is licensed under the MIT License.
