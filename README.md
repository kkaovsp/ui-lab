# UI Lab (`ui-lab`)

Automated UI testing lab for ITCS386 using **Part 3: Robot Framework** and **Part 4: Cypress**.

## Repository Contents & How to Run Tests

### `robot/` (Part 3: Robot Framework)
Contains Robot Framework test suites (`first.robot`, `web_form.robot`, `pizza_scenario.robot`), the QuickPizza page-object resource file (`resources/quickpizza.resource`), `requirements.txt`, and two headless evidence runs (`results/run1/` and `results/run2/`).
Requires the `CHROMEDRIVER` environment variable and the Python virtual environment in `robot/.venv`:
```bash
cd robot && source .venv/bin/activate && robot -d results/run1 -v BROWSER:headlesschrome pizza_scenario.robot
```

### `cypress/` (Part 4: Cypress)
Contains Cypress configuration (`cypress.config.js`), E2E specs (`cypress/e2e/web-form.cy.js`, `cypress/e2e/pizza-scenario.cy.js`), custom admin login command (`cypress/support/commands.js`), and evidence (`cypress/screenshots/`, `cypress/videos/`).
Run all Cypress specs in headless Chrome:
```bash
cd cypress && npx cypress run --browser chrome
```
