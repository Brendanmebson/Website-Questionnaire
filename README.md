# Website Questionnaire

A Google Forms-style website discovery and requirements questionnaire for gathering everything needed to plan, design, build and launch a client's website. It is a single HTML file with no framework and no build step. Submissions, including uploaded images and files, can be sent straight to a Google Drive folder through a free Google Apps Script.

## What it does

- Contains the full 210-question discovery questionnaire across 18 sections, plus a confirmation section.
- Shows one section per page with Back and Next buttons, a progress bar and a "jump to section" menu.
- Uses the right input for each question: text boxes, yes/no radios, single-choice radios, multi-select checkboxes and a date picker. Choice questions also have an optional "Details or other" line.
- Lets clients upload images (logo, brand assets, team photos, example sites) and other files (brand guidelines, documents, reference material) by clicking or dragging, with thumbnails and a remove button. Each file is limited to 10 MB.
- Marks optional questions with a red asterisk: the e-commerce and booking sections, every upload question, and a few questions the questionnaire itself qualifies ("if applicable", "if yes").
- Saves typed answers in the browser as the client goes, so they can close the tab and come back. Uploaded files are held in memory only and are lost on refresh.
- Uses a light theme only.

## Files

| File | Purpose |
| --- | --- |
| `website-questionnaire.html` | The whole questionnaire: markup, styles and logic in one file. |
| `drive-upload.gs` | Google Apps Script that receives each submission and saves it to Google Drive. |

## Quick start

Open `website-questionnaire.html` in a browser, or host it anywhere that serves static files (GitHub Pages, Netlify, Vercel and so on). With no endpoint configured, submitting downloads a JSON file with all answers and uploads.

## Saving submissions to Google Drive

Each submission creates a folder named after the business:

```
Your Drive folder/
  Business Name/
    Business Name - Website Questionnaire Answers   (Google Doc)
    Images/       uploaded images
    Documents/    other uploaded files (only if any were uploaded)
```

Setup:

1. Create a folder in Google Drive and copy its ID from the URL (the part after `/folders/`).
2. Go to [script.google.com](https://script.google.com), create a project and paste in the contents of `drive-upload.gs`. Set `FOLDER_ID` to your folder ID.
3. Click Deploy, then New deployment, and choose type Web app. Set "Execute as" to Me and "Who has access" to Anyone. Authorise the Drive and Docs permissions when prompted.
4. Copy the web app URL into `ENDPOINT` near the top of the script in `website-questionnaire.html`:

   ```js
   const ENDPOINT = 'https://script.google.com/macros/s/XXXXXXXX/exec';
   ```

5. Submit a test entry and check that the folder appears in Drive.

After editing `drive-upload.gs`, redeploy with Deploy, Manage deployments, Edit, then New version. Otherwise the live URL keeps running the old code.

## Customising

- **Questions:** all questions live in the `S` array in the script. Each line is one question. Prefix a line to set its type: `r:` radio, `c:` checkboxes, `u:` image upload, `f:` file upload, `s:` short text, `d:` date. Options follow `=` and are separated by `|`. Lines with no prefix become yes/no radios if they start with a word like "Do", "Are" or "Should", and text boxes otherwise. Question numbers are generated from the order.
- **Optional asterisks:** controlled by the `OPT` list and the `opt:` rule in the parsing code.
- **Colours:** change the CSS variables at the top of the `<style>` block (`--p` is the main purple).
- **Title and icon:** the tab title is the `<title>` tag, and the favicon is an inline SVG in the `<link rel="icon">` tag.

## Limitations

- Anyone with the web app URL can post to it, so don't publish the URL anywhere other than the form.
- Apps Script limits request sizes, so keep total uploads to a few MB per submission.
- The browser can't confirm that Drive received the data. The success message appears even if the script fails, so check the Executions page in Apps Script when troubleshooting.
- Uploaded files are not kept if the page is refreshed before submitting.

## Credit

Made by [brendanmebson](https://github.com/Brendanmebson). Contact for website development: brendanmebson@gmail.com
