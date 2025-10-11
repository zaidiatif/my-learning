# Chapter 36: Testing — Mutation Testing in JavaScript
## 1. What is Mutation Testing?

`Mutation Testing` is an advanced software testing technique that evaluates the `quality and effectiveness of your test suite` by introducing small, controlled changes (called `mutations`) into your source code and checking whether your existing tests can detect them.

### Concept

- A mutation simulates a potential bug — for example, changing > to <, or true to false.
- The testing framework then reruns your existing test suite.
- If your tests fail after a mutation, it means the test suite successfully killed the mutant (i.e., it caught the bug).
- If the tests still pass, it means the mutant survived, revealing a potential weakness in your test coverage.

### Analogy

Think of mutation testing as a “crash test” for your test suite — it tests how well your tests detect real-world defects.

### Example

#### Original Code:
```js
function isAdult(age) {
  return age >= 18;
}
```

#### Test Code:
```js
test('should return true for 18 or older', () => {
  expect(isAdult(18)).toBe(true);
});

test('should return false for under 18', () => {
  expect(isAdult(17)).toBe(false);
});
```

#### Mutations Introduced:

- Replace `>=` with `>`
- Replace `>=` with `<=`

Now, if mutation 1 is applied and your tests `still pass`, your test suite has a `hole` — it didn’t detect a subtle logic error.

Mutation testing exposes these weak spots.

## 2. Why Mutation Testing Matters

| Benefit	| Description |
|:--- |:--- |
| Test Quality Indicator	| Goes beyond coverage — it measures how effective tests are at catching bugs. |
| Finds Weak Tests	| Identifies where your tests are superficial or missing assertions. |
| Encourages Robust Assertions	| Promotes stronger validation logic in tests. |
| Ensures Maintainability	| Prevents overconfidence in high but shallow coverage numbers. |

### `Code coverage ≠ test effectiveness.`
Mutation testing ensures your tests truly validate your code’s behavior, not just execute it.

## 3. Mutation Testing Tools: Stryker

### Overview

**Stryker** is the leading `mutation testing framework for JavaScript, TypeScript, and Node.js`.
It integrates seamlessly with test runners like `Jest, Mocha`, and `Vitest`.

### How It Works

- Parses your code and creates mutants (slightly altered versions).
- Runs your test suite against each mutant.
- Reports which mutants were “killed” (tests failed) and which “survived” (tests passed).
- Generates a mutation score — a percentage of mutants killed.

### Installation
```bash
npm install --save-dev @stryker-mutator/core
```

If you’re using Jest:
```bash
npm install --save-dev @stryker-mutator/jest-runner
```

### Configuration

Create a file named `stryker.conf.json`:
```json
{
  "$schema": "./node_modules/@stryker-mutator/core/schema/stryker-schema.json",
  "mutator": "javascript",
  "testRunner": "jest",
  "reporters": ["html", "clear-text", "progress"],
  "coverageAnalysis": "off",
  "timeoutMS": 5000,
  "ignorePatterns": ["node_modules", "dist"]
}
```

Then run:
```bash
npx stryker run
```

### Output Example

-------------------|---------|----------|-----------|------------|----------|---------|
File               | % score | # killed | # survived | # timeout | # no cov | # error |
-------------------|---------|----------|-----------|------------|----------|---------|
isAdult.js         |   75%   |    3     |     1      |     0      |    0     |    0   |
-------------------|---------|----------|-----------|------------|----------|---------|
All files          |   75%   |          |            |            |          |         |


### Interpretation:

- 75% mutation score means 75% of introduced mutations were caught by your tests.
- The remaining 25% survived — your tests missed them.

## 4. Improving Test Quality with Mutation Testing

Mutation testing helps you pinpoint exactly where and why your tests are weak.

### a. Strengthen Assertions

Make assertions more specific:
```js
// Weak
expect(result).toBeTruthy();

// Strong
expect(result).toBe(true);
expect(user.age).toBeGreaterThanOrEqual(18);
```

### b. Cover Edge Cases

- Include tests for boundary conditions and negative scenarios.
- Example: if a function accepts ranges, test both ends of the range.

### c. Focus on Logic-heavy Code

- Apply mutation testing to critical modules (authentication, financial logic, validation).

### d. Continuous Integration Integration

- Add mutation testing to your CI pipeline for ongoing quality tracking.

- Configure thresholds:
```json
"thresholds": {
  "high": 90,
  "low": 70,
  "break": 60
}
```

### e. Combine with Code Coverage

- Use both coverage and mutation score together:
-   - Coverage = How much code is executed.
-   - Mutation = How well it’s verified.

## 5. Mutation Operators (Examples)

| Mutation Type	| Example Change	| Purpose |
|:--- |:--- |:--- |
| Arithmetic Operator Replacement	| `+` → `-`	| Detect weak numeric tests |
| Logical Operator Replacement	| `&&` → "`" | |	
| Relational Operator Replacement |	`>` → `<`	| Catch missed boundary tests |
| Boolean Literal Replacement	| `true` → `false`	| Expose weak logical validation |
| Conditional Boundary Change	| `>=` → `>`	| Test boundary assertions |
| Return Statement Removal	| Remove return	| Check if output is verified |

## 6. Example Project Setup (React + Jest + Stryker)

### 6.1 Initialize project
```bash
npx create-react-app mutation-demo
cd mutation-demo
npm install --save-dev @stryker-mutator/core @stryker-mutator/jest-runner
```

### 6.2 Add config
```json
{
  "mutator": "javascript",
  "testRunner": "jest",
  "reporters": ["html", "progress"],
  "thresholds": { "high": 85, "low": 70, "break": 60 }
}
```

### 6.3 Run
```bash
npx stryker run
```

### Open report

- Check `reports/mutation/html/index.html`
- View exactly which mutants survived.

## 7. Best Practices

- Use mutation testing after achieving basic unit test coverage.
- Limit scope initially — mutation testing is compute-intensive.
- Automate using CI/CD (e.g., GitHub Actions, Jenkins).
- Analyze mutants to find logical gaps, not just missing lines.
- Combine with static analysis and code coverage for a full picture.

## 8. Summary

| Concept	| Description |
|:--- |:--- |
| Mutation Testing	| Introduces artificial bugs to measure test effectiveness |
| Mutants	| Modified versions of your code (potential bugs) |
| Killed Mutants	| Detected by existing tests (good) |
| Survived Mutants	| Not detected — indicates weak tests |
| Tool	| Stryker — best for JS/TS mutation testing |
| Goal	| Improve test suite robustness beyond coverage numbers |

## Conclusion

Mutation testing provides a deeper insight into the quality of your tests, not just their quantity.
By simulating real-world bugs and ensuring your tests catch them, you build stronger, more reliable software.

**In short:** Mutation testing doesn’t test your code — it tests your tests.