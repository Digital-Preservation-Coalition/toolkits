# digipres-publications
An experiment in using a Markdown workflow for DPC publications.

## How it works

This is all pretty much vanilla [MystMD](https://mystmd.org/).  The system also generates PDFs via Typst, and Pandoc is used to bring in publications from other sources.

Start by setting up a virtual environment to keep the Python dependencies in one place:

```
python3 -m venv .venv
source .venv/bin/activate
```

Then install MystMD and check it's installed okay, as per the instructions:

```
pip install mystmd
myst -v
```

You can now run a local development server using:

```
myst start
```

or use

```
myst build --all
```

which will build the PDFs as well (assuming Typst CLI is installed).

Pandoc commands can be used to generate Markdown versions from source DOCX files and ePub versions from Markdown versions.  _MORE DOCS NEEDED!_

### Generating Markdown from DOCX

Using something like:

```
pandoc source.docx -o index.md --wrap=none -t commonmark_x --extract-media=.
```

TODO Set up some tweaks because `commonmark_x` adds anchors to the headings which MyST does not support. `commonmark_x-attributes` seems to work a bit better.

This makes a simple `index.md` Markdown file and places any images in a `./media/` folder.

It's still necessary to tweak some things and check the output looks okay:

- Move some information like title, abstract and authors/affiliations into structured front matter.
- Add exports, downloads (see below) and thumbnails (for social cards) to the front matter too.
- Remove styling inside hyperlinks (e.g. underlines or italics).
- Break up big tables so the PDF layout is okay.
- Simplify more complex markup to use MystMD features instead, if possible.

### Generating ePub from DOCX

With the appropriate metadata in the Markdown, it's possible to generate an acceptable ePub version like this:

```
pandoc index.md -o exports/index.epub
```

This can then be connected in by configuring the downloads in the front matter for that page. For example, this supports generating a PDF version directly in MystMD, and making that and the ePub version available:

```
exports:
  - format: pdf
    template: lapreprint-typst
    output: exports/imaging-floppy-disks.pdf
downloads:
  - file: index.md
    title: Markdown Source
  - file: exports/imaging-floppy-disks.pdf
    title: PDF
  - file: exports/imaging-floppy-disks.epub
    title: ePub
```

## Limitations

- Currently, as it's running on GitHub Pages, this repo needs to be public in order for the results to be visible. We'll probably want to keep source files separate, or limit access to works-in-progress.
- Typst makes nice PDFs but can be a bit picky. It especially does not like styling within hyperlinks (e.g. `[<u>link</u>]` or `[_link_]`). That is bad practice anyway, but it would be good to work on the DOCX-to-Markdown transformation to make this as clean as possible.

## Features To Consider Adding

- Plugin that extends cards to add scaled image floats, rather than having to repeat fragments of HTML in cards. This might just be a case of completing the functionality of the current [image directive](https://mystmd.org/guide/figures#image-directive) which IIRC does not behave the same way as [the original one](https://docutils.sourceforge.io/docs/ref/rst/directives.html#image), which does float the image to one side (as per the [Codex homepage](https://anjackson.net/codex/)).
- Plugin that can place a Status banner at the top of the page.


