# claude-recipes

Jack's recipes. A web app reads every `.json` file in the root of this repo, on the `main` branch, and shows it in its Food tab. You manage the recipes here; the app only displays them.

## Files

- One recipe per file, in the repo root. Name the file after the recipe in kebab-case, e.g. `tuna-egg-hash.json`.
- Don't put any other `.json` files in the root, or the app will try to read them as recipes.
- To rename a recipe, rename its file too.

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
| `serving`     | yes      | What `kcal` and `macros` refer to, e.g. `"1 portion"` or `"1 gyoza"`                           |
| `macros`      | yes      | `protein_g`, `carbs_g` and `fat_g` in grams, for one serving. Round to 1 decimal place at most |
| `ingredients` | no       | Each one has an `item` (include the quantity in brackets) and its `kcal` as a whole number     |
| `notes`       | no       | A plain string                                                                                 |
| `tags`        | no       | Lowercase strings                                                                              |

No other fields: the app ignores anything else.

## Rules

- When there are ingredients, their `kcal` values should add up to `kcal`, give or take 5%. If they don't, fix the numbers rather than leave them inconsistent.
- For foods eaten by the item (gyozas, dumplings, biscuits), use `"serving": "1 gyoza"` and give the per-item calories. For a mix of products, use the average and explain it in `notes`.
- For a template with swappable parts (e.g. a tray bake), give calories for the default version and list the swaps in `notes`.
- Use product names and pack sizes where Jack gives them. Don't invent brands.
- Calorie figures come from what Jack provides, or from the product label. If you had to estimate, say so in `notes`.

## Workflow

- Commit straight to `main` with a short message, e.g. `add: tuna egg hash` or `update: gyozas kcal`. The app picks changes up within about 5 minutes.
- Check that the JSON is valid before committing, e.g. with `python3 -m json.tool file.json`. The app skips invalid files.
