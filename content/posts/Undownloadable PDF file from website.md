---
title: "Undownloadable PDF file from website"
date: 2025-10-28T08:37:56+02:00
lastmod: 2025-10-28T08:37:56+02:00
draft: false
tags: []
slug: "undownloadable-pdf-file-from-website"
---

So I was browsing this course that had an integrated PDF file in the webpage. But the issue was I couldn't download it as it had no download Button.

Here's how to do it:

1. Open developer tools using `CTRl + SHIFT + I`.
2. Locate the `<iframe>` or `<object>` tag containing the PDF file.
3. Inside it should be a link containing your PDF file. It should look like this:

```html
<iframe src="https://cdn.filestackcontent.com/preview=css:%22https%3A%2F%2Fassets.teachablecdn.com%2Fcss%2Ffilestack-pdf-viewer.css%22/0AdoewrnQty0kvu0qO8j" width="100%" height="100%"></iframe>
```

4. Extract the link from the `src` attribute: `https://cdn.filestackcontent.com/preview=css:%22https%3A%2F%2Fassets.teachablecdn.com%2Fcss%2Ffilestack-pdf-viewer.css%22/0AdoewrnQty0kvu0qO8j`
5. Open that link in a new tab then open Console tab in Developer tools.
6. Paste the following JavaScript line:

```js
document.getElementById('download').click()
```

7. Your PDF file should be downloaded!

## OR:
### Or just do `CTRL+S` . Does the job!
