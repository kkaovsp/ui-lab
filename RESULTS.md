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
- robot --version:
- Step 3.2, failure message for the wrong title:
- Step 3.8, run 1, result, elapsed time and pizza name:
- Step 3.8, run 2, result, elapsed time and pizza name:

## Part 4: Cypress
- node --version, npm --version and npx cypress version:
- Step 4.3, failure message for the wrong title:
- Step 4.6, tool and calories in the answer of POST /api/pizza:
- Step 4.7, the line of the admin list that shows your pizza:
- Step 4.8, run 1, result, duration and pizza name:
- Step 4.8, run 2, result, duration and pizza name:

## Part B
1. 
2. 
3. 
4. 
5. 
6. 
7. 
