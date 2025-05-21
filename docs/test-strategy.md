# Test Strategy Document – Sabre Rail Services Ltd

**Project:** Titan Platform  
**Document Version:** 1.0  
**Prepared by:** D. L. Pratt  
**Date:** [Insert Date]

---

## 1. Introduction

This document defines the test strategy for the Titan system, covering automated testing practices using **xUnit** for unit and integration tests, and **Playwright** for end-to-end (E2E) UI testing.

### Objectives

- Ensure code quality through automated testing.
- Standardize test tools and patterns across projects.
- Facilitate CI/CD integration and reliable regression testing.

---

## 2. Test Types

### 2.1 Unit Testing (xUnit)

- Covers individual methods and business logic.
- Mocking dependencies using **Moq**.
- Test naming convention: `MethodName_Condition_ExpectedOutcome`
- Test pattern: **Arrange–Act–Assert (AAA)**

```csharp
[Fact]
public void CalculateTotal_WithValidInputs_ReturnsCorrectSum()
{
    // Arrange
    var calculator = new Calculator();

    // Act
    var result = calculator.CalculateTotal(2, 3);

    // Assert
    Assert.Equal(5, result);
}
```

### 2.2 Integration Testing (xUnit + TestServer)

- Tests multiple components working together.
- Use `WebApplicationFactory<T>` for ASP.NET Core APIs.
- Configure test DB (e.g., SQLite in-memory or TestContainers for PostgreSQL).

### 2.3 End-to-End Testing (Playwright)

- Browser-based tests simulating real user interactions.
- Written in TypeScript or C# via Microsoft.Playwright.
- Run headless in CI pipelines, with video and trace capture enabled.
- Base URLs are injected via environment variables.

```ts
test('Visitor login should show dashboard', async ({ page }) => {
  await page.goto(process.env.BASE_URL + '/login');
  await page.fill('#username', 'testuser');
  await page.fill('#password', 'P@ssword123');
  await page.click('button[type=submit]');
  await expect(page).toHaveURL(/.*dashboard/);
});
```

---

## 3. Tooling and Frameworks

| Purpose        | Tool                                 |
| -------------- | ------------------------------------ |
| Unit Testing   | xUnit                                |
| Mocking        | Moq                                  |
| Integration    | xUnit + WebApplicationFactory        |
| E2E Testing    | Microsoft.Playwright                 |
| Test Reporting | Playwright Traces / Test Output Logs |

---

## 4. Project Structure

```plaintext
/tests
├── Titan.App.UnitTests          # xUnit unit tests
├── Titan.App.IntegrationTests   # Integration tests
└── Titan.App.E2ETests           # Playwright tests
```

---

## 5. CI/CD Integration

- All test projects are executed in GitHub Actions.
- Unit + integration tests run on `.NET 8` matrix.
- E2E tests run in parallel using `npx playwright test`.
- Playwright artifacts (videos, traces, screenshots) are uploaded on failure.

---

## 6. Quality Gates

| Metric                  | Threshold        |
| ----------------------- | ---------------- |
| Unit Test Coverage      | 80% minimum      |
| Integration Test Pass   | 100% required    |
| E2E Test Stability Rate | 95%+ over 3 runs |

---

## 7. Best Practices

- Test only public behavior, not implementation details.
- Isolate unit tests from external dependencies.
- Avoid flaky tests by ensuring stable selectors in Playwright.
- Prefer `[Theory]` + `[InlineData]` for reusable xUnit tests.
- Mark slow or unstable tests with `[Trait("Category", "Slow")]`.

---

## 8. Maintenance and Ownership

- Test ownership lies with the same team responsible for the corresponding feature.
- Tests must be updated alongside the feature.
- Broken tests in `main` block merges until resolved.

---

**End of Document**
