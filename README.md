# Image Hoster

This repository serves as a free, fast CDN for hosting images (such as PNG icons) using GitHub and jsDelivr. Since these assets are updated frequently (e.g., training plans updated 3 times per week), this setup keeps assets organized alongside code or notes while functioning as a professional CDN.

## Workflow: Hosting Assets via jsDelivr

### 1. Upload Your Icons
1. Add your PNG icons or images into this repository.
2. Commit and push the changes to the `main` branch.

### 2. Construct Your jsDelivr URL
jsDelivr automatically serves any file from a public GitHub repository. You do not need to sign up for jsDelivr.

The base URL format is:
`https://cdn.jsdelivr.net/gh/USER/REPO@VERSION/FILE_PATH`

**Examples for this repo:**
* **Latest version (simplest, but subject to caching):**
  `https://cdn.jsdelivr.net/gh/W3sley0Bi/image-hoster/ebp-power-connector/ebp-power-connector-icon.png`
* **Specific Branch (e.g., 'main'):**
  `https://cdn.jsdelivr.net/gh/W3sley0Bi/image-hoster@main/ebp-power-connector/ebp-power-connector-icon.png`

### 3. Adding the URL to Supabase

Once you have your jsDelivr URL, you can store it in your Supabase database so your client applications can load the images.

**Option A: Using the Supabase Dashboard (UI)**
1. Open your project in the [Supabase Dashboard](https://app.supabase.com/).
2. Navigate to the **Table Editor** and select the relevant table (e.g., `training_plans`, `exercises`).
3. Locate the row you wish to update.
4. Paste the jsDelivr URL into the appropriate `text` or `varchar` column (e.g., `icon_url`, `image_url`).
5. Save the changes.

**Option B: Using SQL (SQL Editor)**
```sql
UPDATE training_plans
SET icon_url = 'https://cdn.jsdelivr.net/gh/W3sley0Bi/image-hoster@main/ebp-power-connector/ebp-power-connector-icon.png'
WHERE id = 'target-record-id';
```

**Option C: Using the Supabase JavaScript/TypeScript Client**
```typescript
const { data, error } = await supabase
  .from('training_plans')
  .update({ icon_url: 'https://cdn.jsdelivr.net/gh/W3sley0Bi/image-hoster@main/ebp-power-connector/ebp-power-connector-icon.png' })
  .eq('id', 'target-record-id');

if (error) {
  console.error('Error updating icon URL:', error);
} else {
  console.log('Successfully updated icon URL:', data);
}
```

## Key Tips & Limitations for 2026
* **Caching:** jsDelivr has a heavy cache. If you update an icon but keep the same filename, it might take **up to 24 hours** for the CDN to serve the new version.
* **Version Control (Cache Busting):** To bypass the cache immediately when updating an existing image, append a specific release tag or commit hash to the URL:
  `https://cdn.jsdelivr.net/gh/W3sley0Bi/image-hoster@1.0.1/ebp-power-connector/ebp-power-connector-icon.png`
* **File Limits:** Keep individual PNGs under **20MB**. (Standard icons will easily fall below this limit).
* **Production Usage:** While reliable, jsDelivr's terms of service suggest avoiding "general-purpose file hosting" for massive traffic. For a personal project or a medium-sized site, this solution is perfectly fine.
