# Plan: Categorize 120 Images → category.json

## JSON Schema per entry
{
  "filename": "airalo-LsoE1ebr2EQ-unsplash.jpg",
  "category": "Travel",
  "description": "Brief English description of image content"
}

## Batches (20 images each — sequential)
- Batch 1: files 1–20   (airalo... → pexels-ana-hidalgo...)
- Batch 2: files 21–40  (pexels-anandhu... → pexels-bezalens...)
- Batch 3: files 41–60  (pexels-blackstoneray... → pexels-dmitry93...)
- Batch 4: files 61–80  (pexels-dursen... → pexels-ivan-kazlouski...)
- Batch 5: files 81–100 (pexels-jagaba... → pexels-mikhail-nilov-8297855...)
- Batch 6: files 101–120 (pexels-mkaynarfoto... → pexels-will-louis...)

## Steps
1. View each batch of 20 images visually → classify → accumulate JSON entries
2. After all 6 batches → write category.json to images/ folder (120 entries)

## Output
- File: images/category.json (alongside catalog.json)
- Fields: filename, category, description (English)
- Categories: free-form English (Travel, Sports, Food, Architecture, Nature, People, Art, Holidays, Business, Culture, Animals, etc.)