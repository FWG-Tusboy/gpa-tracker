# CLAUDE.md

## Before every session
- Read `common_errors.md` (if it exists) and avoid repeating the errors recorded there.

## Development process: Waterfall
This project follows the Waterfall model. Finish each stage before moving to the next.

### Stage 1: Requirements (done)
- Requirements are in `PRD.md`. Treat it as the source of truth for all functions.

### Stage 2: Design
- Design the business flow of each function as a flow chart in Mermaid syntax.
- Use one file per function, saved in `design/flows/` (e.g. `design/flows/<function-name>.md`).

### Stage 3: Implementation
- Before starting this stage, configure the project tools and install the needed libraries.
- Implement one function at a time:
  1. Write the code for the function.
  2. Write unit tests for it.
  3. Run the tests and make sure they all pass.
  4. Ask the user to confirm the function works in the browser.
  5. Move on to the next function only after the user confirms.
- Once a function is done, tell a relevant computer joke.

## Handling errors
- After fixing any error, tell the user to run `/learn-by-mistake` to save the error to `common_errors.md`.
