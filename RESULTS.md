# UI lab results

Tool parts that I chose: Part 3 (Robot Framework) and Part 4 (Cypress)

## Part 0
- Step 0.1, Chrome version: Google Chrome 157.0.8103.51 (Official Build) (arm64)
- Step 0.3, ChromeDriver version: ChromeDriver 157.0.8103.51
- Step 0.4, locator of Pizza, Please!: `button[name="pizza-please"]`
- Step 0.4, locator of Excluded tools: `select`
- Step 0.4, locator of Must be vegetarian: `input#default-checkbox`
- Step 0.4, locator of Custom Pizza Name: `input#pizza-name`
- Step 0.5, pizza name, tool and calories of the run by hand: `Lab Pizza 20261113-142400`, `Pizza cutter`, `500`

| Step | Actual result | Pass or fail |
|------|---------------|--------------|
| 1 | Heading shows `Looking to break out of your pizza routine?` | Pass |
| 2 | All 4 advanced fields (`Max Calories per Slice`, `Excluded tools`, `Must be vegetarian`, `Custom Pizza Name`) appear | Pass |
| 3 | Fields show `600`, `Knife`, vegetarian ticked, `Lab Pizza 20261113-142400` | Pass |
| 4 | Recommendation shows `Name: Lab Pizza 20261113-142400`, `Tool: Pizza cutter`, `Calories per slice: 500` | Pass |
| 5 | Message shows `Please log in first.` | Pass |
| 6 | Admin page logs in and shows `Latest pizza recommendations` | Pass |
| 7 | Admin list shows `Lab Pizza 20261113-142400 (tool=Pizza cutter, ingredients_number=6)` | Pass |

## Part 3: Robot Framework
- robot --version: Robot Framework 7.5.1 (Python 3.14.7 on darwin)
- Step 3.2, failure message for the wrong title: `Title should have been 'Web Form' but was 'Web form'.`
- Step 3.8, run 1, result, elapsed time and pizza name: `PASS`, `11.88s` (`11.878831s`), `Lab Pizza 20261113-142741`
- Step 3.8, run 2, result, elapsed time and pizza name: `PASS`, `7.32s` (`7.324483s`), `Lab Pizza 20261113-142752`

## Part 4: Cypress
- node --version, npm --version and npx cypress version: Node `v22.14.0`, npm `10.9.2`, Cypress package/binary `16.2.1`
- Step 4.3, failure message for the wrong title: `Timed out retrying after 10000ms: expected 'Web form' to equal 'Web Form'` (`-'Web form'`, `+'Web Form'`)
- Step 4.6, tool and calories in the answer of POST /api/pizza: Run 1 `Tool: Pizza cutter`, `Calories per slice: 525`; Run 2 `Tool: Scissors`, `Calories per slice: 350`
- Step 4.7, the line of the admin list that shows your pizza: Run 1 `Lab Pizza 20261113-143102 (tool=Pizza cutter, ingredients_number=7) (newest)`; Run 2 `Lab Pizza 20261113-143142 (tool=Scissors, ingredients_number=5) (newest)`
- Step 4.8, run 1, result, duration and pizza name: `All specs passed!`, `00:12` (`pizza-scenario.cy.js` `00:11`, `web-form.cy.js` `00:01`), `Lab Pizza 20261113-143102`
- Step 4.8, run 2, result, duration and pizza name: `All specs passed!`, `00:09` (`pizza-scenario.cy.js` `00:08`, `web-form.cy.js` `00:01`), `Lab Pizza 20261113-143142`

## Part B
1. On QuickPizza, all three numeric inputs under Advanced share the same id `floating_filled`, so `$$("#floating_filled").length` returns `3` instead of `1`. Because an id selector cannot distinguish `Max Calories per Slice` from the min/max toppings inputs, the tests use an XPath expression anchored to the visible label text (`//label[contains(.,'Max Calories')]/../input`). Giving each input its own unique id, such as `id="max-calories"`, `id="min-toppings"`, and `id="max-toppings"`, would let tests locate the box directly with `#max-calories`. This helps a tester because id selectors are shorter, faster, and do not break if the label text or surrounding HTML structure changes.

2. Comparing the tool shown on the admin list against the tool returned in the customer recommendation is a consistency check oracle. It compares two views of the same system against each other to verify they agree on the stored data. For example, if QuickPizza accidentally saved a default tool (like `Knife`) or swapped the tool field when persisting the recommendation to the backend database, the recommendation page would look fine to the customer while the admin list would show the wrong tool. Step 7 catches that mismatch immediately when checking `(tool=Scissors,` or `(tool=Pizza cutter,` on the admin line.

3. Right now, QP-01 only ticks `Must be vegetarian` in Step 3 without verifying that the generated pizza actually avoids meat. A vegetarian check could verify the ingredients list shown inside `#recommendations` (or the `pizza.ingredients` / `vegetarian` field in the `POST /api/pizza` response) to assert that no meat items such as pepperoni, prosciutto, or bacon appear. To make that assertion reliable, the test would need either a defined reference list of non-vegetarian ingredients to reject, or a `vegetarian` / `dietary` flag displayed directly on the recommendation card or exposed in the API response. Checking the API response's `vegetarian: true` flag together with scanning the rendered ingredient list would catch both backend filtering bugs and UI display bugs.

4. Robot Framework required explicit `Wait Until ...` keywords (such as `Wait Until Element Is Visible css:h1`, `Wait Until Page Contains Element id:svelte-announcer`, and `Wait Until Element Contains id:recommendations Name:`), whereas Cypress automatically retried every `cy.get()` and `.should()` assertion for up to `10000ms` without explicit wait statements. Writing QP-01 was faster in Cypress because there was no need to configure ChromeDriver or write manual waits, and `cy.intercept('POST', '/api/pizza')` made checking both the request payload and server response straightforward. For evidence, Robot Framework produced `log.html` and `report.html` showing every keyword step, elapsed time (`11.88s` and `7.32s`), and embedded screenshots, while Cypress produced `00:12` / `00:09` spec summaries, full-page screenshots (`qp01-step4-recommendation.png`, `qp01-step7-admin-list.png`), and `.mp4` videos showing the Command Log side by side with the live browser. Both outputs were clear, though Cypress's video made debugging timing and state transitions even easier to inspect visually.

5. For the automated UI tests of Project 2, I would choose Cypress. First, Cypress manages the browser directly without needing a separate ChromeDriver binary or `CHROMEDRIVER` environment variable, and its built-in automatic retrying (`defaultCommandTimeout: 10000`) eliminates flaky timing code while keeping test scripts concise. Second, Cypress provides `cy.intercept()` to verify backend API payloads alongside UI assertions, plus built-in MP4 video recording and failure screenshots that give clear evidence of full test runs for Project 2.

6. In Part 3 (Robot Framework), if QuickPizza changed the locator of the `Max Calories per Slice` box, I would only need to update the `${MAX CALORIES}` variable in `robot/resources/quickpizza.resource` (currently `xpath://label[contains(.,'Max Calories')]/../input`). The test file `robot/pizza_scenario.robot` never references XPath or CSS selectors directly; it only calls high-level keywords like `Fill In The Pizza Request`. Because all page locators and low-level `SeleniumLibrary` steps are encapsulated inside the resource file, the test scenario file stays untouched when the page markup changes.

7. In Part 4 (Cypress), Cypress automatically clears cookies and browser storage before each `it(...)` test block to keep tests isolated. QuickPizza distributes requests across backend instances and uses a session/visitor cookie so that the `/admin` page shows recommendations created on the same server instance as the customer session. If Steps 6 and 7 were placed in a second `it(...)` block, Cypress would wipe that cookie before Step 6, so the admin page could connect to a different server instance or lose the session context and fail to find `Lab Pizza ...` in `Latest pizza recommendations`. Keeping Steps 1 to 7 in a single `it(...)` test preserves the cookie from the customer run through the admin verification.
