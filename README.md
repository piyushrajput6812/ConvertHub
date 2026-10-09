# ConvertHub — browser-based image and PDF studio

Responsive static website for GitHub Pages. The application processes selected files in the browser for supported operations; it does not send files to a ConvertHub backend.

## Files
- `index.html`: UI and CDN library references.
- `styles.css`: responsive design.
- `app.js`: image and PDF tools.
- `.nojekyll`: static publishing helper.

## Included in this release

### Image studio
- Convert to JPG, PNG, or WebP.
- Quality control for JPG/WebP.
- Target file size in KB/MB: the converter reduces quality and, if needed, dimensions, then pads the encoded file to the requested byte count when the image data can fit. Extremely small targets may be impossible without reducing dimensions substantially.
- Resize width/height and aspect-ratio presets.
- Centered rectangular, square, or circular crop.
- White or black background fill.
- Optional AI background removal using a browser-loaded third-party model; first use may download a large model and can fail if the browser/CDN/network blocks it.
- Per-file downloads and ZIP bundle.

### PDF studio
- Merge PDFs.
- Extract selected pages.
- Remove selected pages.
- Split into individual one-page PDFs (ZIP).
- Live PDF page thumbnails, page selection, and drag-to-reorder.
- Rotate selected pages (or all if no selection/range is specified).
- Images to PDF with A4 fit, A4 fill, Letter fit, or original image dimensions.
- PDF pages to JPG/PNG.
- Word `.docx` to PDF using browser-side DOCX-to-HTML plus HTML-to-PDF conversion.
- Watermark text and page numbers.
- Experimental raster-based PDF compression.

## Known limitations — read before publishing
1. This is a browser-only starter, not a complete clone of iLovePDF. Their catalogue includes many advanced functions and formats; some require server-side engines, OCR, Office rendering, PDF structure transforms, or commercial APIs.
2. `.docx` conversion reflows document content via HTML and may not preserve complex Word layouts, tables, fonts, headers, footers, or page breaks perfectly. Legacy `.doc` is not supported.
3. Target image size: the converter attempts quality reduction (JPG/WebP) and downscaling, then pads the file to the exact requested byte count when possible. A very small target may require a much smaller image or may be impossible. Padding targets the file byte count; it does not mean the image content itself uses every byte for visual information.
4. AI background removal loads code/model from a third party. It needs internet access, can be slow, and may not work on every browser. Files are sent to the model runtime only in the browser; no ConvertHub server is used.
5. Circular crop works best with PNG or WebP and transparent background. JPG cannot contain transparency and uses a solid fill.
6. The PDF compression action rasterizes every page to JPEG. It can reduce file size, but loses selectable text, vector quality, accessibility, and some metadata; it may also increase size for some PDFs.
7. PDF rendering, page previews, and large files can use significant browser memory.
8. Existing PDF page-size conversion renders pages to images and rebuilds an A4/Letter PDF. This can change searchable text into images and reduce vector quality. Choose Original size to keep the original PDF unchanged. A4/Letter fit is also supported for images-to-PDF.
9. Files with encryption, malformed structures, signatures, forms, unusual fonts, or complex layouts may fail or be altered when re-saved.
10. The app loads PDF-lib, PDF.js, JSZip, html2pdf.js, Mammoth and optional background-removal code from CDNs. An internet connection is needed. Third-party library scripts are fetched by the browser, but the app does not upload selected documents to a ConvertHub server.

## Test locally
Do not open `index.html` by double-clicking it: PDF.js workers may not work under `file://`.
Run:
```bash
python -m http.server 8000
```
Open `http://localhost:8000`.

## Deploy to GitHub Pages
1. Download and extract `ConvertHub-v2.zip`.
2. Create a GitHub repository, for example `converthub`.
3. Upload the *contents* of the extracted folder into the repository root (ensure `index.html` is at the root).
4. Go to **Settings → Pages**.
5. Set Source to **Deploy from a branch**, branch `main`, folder `/(root)`, then Save.
6. Wait for the Pages deployment to finish. URL pattern: `https://YOUR-USERNAME.github.io/converthub/`.
7. Open the live site and test image conversion, PDF previews, merge, extract and download on a small sample first.

For GitHub's official branch deployment instructions, see https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site.

## Future improvements
Possible next modules: page-size transform, PDF crop, blank-page removal, OCR, edit/annotate, password protection/removal (where legally permitted), Office/PPT/Excel conversion, PDF to Word/Excel/PPT, PDF/A, metadata tools and offline/PWA support. Evaluate dependencies and licences before commercial deployment.
