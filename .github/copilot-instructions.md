# Puzzle Images — Copilot Instructions

## Project Overview
This workspace organizes puzzle images by classifying them into categories and maintaining a master catalog (`category.json`) at the workspace root.

---

## Workflow: Classify & Organize Images

When asked to classify and organize a folder of images, follow these steps in order:

### Step 1 — Visually inspect and classify images
- View each image using `view_image` tool.
- Assign each image a **category** (one of the established categories listed below, or a new one if clearly needed).
- Write a concise **description** (one sentence) of what the image shows.
- Process images in batches of ~20 to avoid context limits. Mark any that fail to load as `PENDING` and revisit them in a dedicated catch-up pass at the end.

### Step 2 — Write `category.json`
- Save the catalog at the **workspace root**: `category.json` (not inside any subfolder).
- Each entry must follow this exact schema:
  ```json
  {
    "filename": "example.jpg",
    "category": "Nature",
    "description": "A mountain lake at sunset with reflections of pine trees",
    "path": "categories/Nature/example.jpg"
  }
  ```
- The `path` field uses **forward slashes**, relative to workspace root.

### Step 3 — Create category folders
- Create all needed category subfolders under `categories/` at the workspace root:
  ```
  categories/{CategoryName}/
  ```
- Use PowerShell `New-Item -ItemType Directory -Force` to create them.

### Step 4 — Move images into category folders
- Read `category.json` and move each image from its source folder to `categories/{category}/{filename}`.
- Use a PowerShell loop:
  ```powershell
  $json = Get-Content "category.json" | ConvertFrom-Json
  $json | ForEach-Object {
      $dest = "categories\$($_.category)"
      Move-Item -Path "source_folder\$($_.filename)" -Destination "$dest\$($_.filename)" -Force
  }
  ```

### Step 5 — Add / update the `path` field
- After images are moved, ensure every entry in `category.json` has the correct `path` value:
  ```powershell
  $json = Get-Content "category.json" | ConvertFrom-Json
  $json | ForEach-Object {
      $_ | Add-Member -NotePropertyName "path" -NotePropertyValue "categories/$($_.category)/$($_.filename)" -Force
  }
  $json | ConvertTo-Json -Depth 3 | Set-Content "category.json" -Encoding UTF8
  ```

---

## Established Categories
Animals, Architecture, Art, Business, Children, Culture, Digital Art, Flowers, Food, Garden, Halloween, Holidays, Interior, Lifestyle, Market, Nature, People, Sports, Street Art, Technology, Toys, Travel, Urban, Vehicles, Vintage, Wedding

Add new categories only when an image clearly does not fit any of the above.

---

## JSON File Locations
| File | Location |
|------|----------|
| `category.json` | workspace root |
| `images/catalog.json` | legacy catalog (do not overwrite) |

---

## Notes
- Always use **UTF-8 encoding** when writing JSON files.
- Always use **forward slashes** in `path` values, even on Windows.
- Source image folders (e.g., `new 900/`) should be left empty after migration, not deleted.
- Batch image classification in groups of ~20 to avoid context budget issues.
