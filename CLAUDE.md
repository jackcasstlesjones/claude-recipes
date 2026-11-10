# claude-recipes

Jack's recipes. A web app (laozi) reads every `.json` file in this repo, on the `main` branch, and shows it in its Food tab. You manage the recipes here; the app only displays them.

## Files

- One recipe per file, in the repo root. Name the file after the recipe in kebab-case, e.g. `tuna-egg-hash.json`.
- A recipe with versions gets its own folder, with one file per version, e.g. `smashes/ham-egg/basic.json` and `smashes/ham-egg/full.json`. The app treats every `.json` file in a folder as a version of that folder's recipe, and shows a toggle between the version names (currently `basic` and `full`). So don't put loose recipe files in a grouping folder like `smashes/`: each recipe needs its own subfolder, even if it has only one version.
- Smashes: `basic` is Jack's cut-down version (smash only, no bread, ingredients with kcal). `full` is the Mob original with Mob's per-serving figures, bread included where the recipe serves it on bread; its ingredients and method go in `notes`, since Mob doesn't give per-ingredient calories.
- Folders starting with `_` (e.g. `_legacy/`) or `.` are ignored. Don't put any other `.json` files anywhere else, or the app will try to read them as recipes.
- To rename a recipe, rename its file (or folder) too. Renaming changes its link and its slug in the food log.

## Schema

```json
{
  "name": "Tuna Egg Hash",
  "kcal": 575,
  "serving": "1 portion",
  "macros": { "protein_g": 75, "carbs_g": 8, "fat_g": 25 },
  "ingredients": [
    { "item": "Tuna in brine (2 tins, ~145g each drained)", "kcal": 220 }
  ],
  "notes": "Optional free text: method, swaps, tips.",
  "tags": ["breakfast", "high-protein"]
}
```

| Field         | Required | Rules                                                                                          |
| ------------- | -------- | ---------------------------------------------------------------------------------------------- |
| `name`        | yes      | The display name, in Title Case                                                                |
| `kcal`        | yes      | A whole number of calories for one `serving`                                                   |
| `serving`     | yes      | What `kcal` and `macros` refer to, e.g. `"1 portion"`, `"1 gyoza"` or `"1 portion (batch makes 4)"` |
| `macros`      | yes      | `protein_g`, `carbs_g` and `fat_g` in grams, for one serving. Round to 1 decimal place at most |
| `ingredients` | no       | Each one has an `item` (include the quantity in brackets) and its `kcal` as a whole number     |
| `notes`       | no       | A plain string                                                                                 |
| `tags`        | no       | Lowercase strings                                                                              |

No other fields: the app ignores anything else.

## Rules

- When there are ingredients, their `kcal` values should add up to `kcal`, give or take 5%. If they don't, fix the numbers rather than leave them inconsistent.
- For foods eaten by the item (gyozas, dumplings, biscuits), use `"serving": "1 gyoza"` and give the per-item calories. For a mix of products, use the average and explain it in `notes`.
- For a batch recipe, give `kcal` and `macros` per portion and say how many the batch makes in `serving`. Give each ingredient its per-portion share of the kcal, with the batch quantity, e.g. `"Boiled eggs, large (¼ of 6)"`.
- For a template with swappable parts (e.g. a tray bake), give calories for the default version and list the swaps in `notes`.
- Use product names and pack sizes where Jack gives them. Don't invent brands.
- Calorie figures come from what Jack provides, or from the product label. If you had to estimate, say so in `notes`.

## Workflow

- Commit straight to `main` with a short message, e.g. `add: tuna egg hash` or `update: gyozas kcal`. The app picks changes up within about 5 minutes.
- Check that the JSON is valid before committing, e.g. with `python3 -m json.tool file.json`. The app skips invalid files.
