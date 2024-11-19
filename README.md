# digipres-publications
An experiment in using a Markdown workflow for DPC publications.

## How it works:

This is all pretty much vanilla [MystMD](https://mystmd.org/).  The system also generates PDFs via Typst, and Pandoc is used to bring in publications from other sources.

## Limitations

- Currently, as it's running on GitHub Pages, this repo needs to be public in order for the results to be visible. In practice, we'll need to manage author reviews and member releases, so it'll need to be deployed separately.
- Generation of ePubs will need a little work, using Pandoc to generate them from the source file(s), and integrating into the downloads.