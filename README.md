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

TODO Set up some tweaks because commonmark_x adds anchors to the headings which MyST does not support.

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


## Editing Content

You can edit online using [this vscode.dev link](https://vscode.dev/github/Digital-Preservation-Coalition/digipres-publications).  You'll need to allow pop-ups so it can authenticate you and log you into GitHub.

You can even install and enable the [Live Share extension](https://marketplace.visualstudio.com/items?itemName=MS-vsliveshare.vsliveshare), and then send people a link they can use to co-author pages from their web browser.

I did find that Firefox failed to work with the shared link, due to this error:

    Firefox Cross-Origin Request Blocked: The Same Origin Policy disallows reading the remote resource at https://prod.liveshare.vsengsaas.visualstudio.com/..........

But Chrome worked fine from the same machine?! Safari should work too.

It doesn't automatically create/forward other views like a preview pane, so some instruction would be needed.

Ideally it would be possible to forward/share the live preview of the version being co-authored. i.e. run the lead session on a real computer running the preview pane, and forward the pane somehow.

GitHub Desktop
Python 3.11 from MS Store
VS Code + Extensions for Python & Live Share

For a virtual environment:

```
PS C:\Windows\system32> Set-ExecutionPolicy unrestricted

Execution Policy Change
The execution policy helps protect you from scripts that you do not trust. Changing the execution policy might expose
you to the security risks described in the about_Execution_Policies help topic at
https:/go.microsoft.com/fwlink/?LinkID=135170. Do you want to change the execution policy?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"): A
PS C:\Windows\system32>
```

```
python3 -m venv .venv
.\.venv\Scripts\activate
pip install myst
pip install mystmd
myst start
```

Tried to install Node the first time it ran, but it didn't work. Installing manually via https://nodejs.org/en/download/prebuilt-installer
Did a default install with no additional dependencies e.g. Chocolatey.

Restarted VS Code. Then node was picked up okay.

Okay, pretty sure the web version of VS Code can't share servers the way Live Share intends that to work. I can set up individual forwarded ports using the separate port forwarding system, but that doens't work because `myst start` needs two ports (!).

Yes, running full VS Code with Live Share works fine and can be set up to immediately see local changes.

So, can fully collaborate on just about anything if folks can install VS Code locally.  Can pretty easily use web VS Code to edit things together, but preview will need additional work, e.g. building the site as static files and sharing that somehow.

I mean, really, this should be a blog, right?



jupyter-book build .

I've found I can just run http.server

python -m http.server --directory ${PWD}/_build/html



export BASE_URL=foobar
nohup watchmedo shell-command -v \
        --patterns='*.md;*.ipynb' \
        --recursive \
        --command='myst build --html && pkill node' \
        --ignore-patterns='./_build/html/build/*;nohup.out' \
        --drop \
        . &
cd _build/html
printf y|npx http-server --port 8000 --cors
Ideally it would be possible to forward/share the live preview of the version being co-authored. i.e. run the lead session on a real computer running the preview pane, and forward the pane somehow. Seems like it should work, but I think not via WSL. Trying when running directly in Windows...

