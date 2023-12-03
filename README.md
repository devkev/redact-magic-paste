redact-magic-paste
==================

_**A better way to redact text documents**_

The traditional way to redact text documents (such as PDFs) involves rasterising the pages and drawing black boxes.  This works, but makes the files much larger and unsearchable (since the text is replaced by raster images), and can be a pain (eg. working page-by-page, and stitching the pages back together).  If the resulting file is going to be digitally shared (not just printed), then it also requires care to ensure that the underlying text is actually removed, and not merely "covered up".

The `redact-magic-paste` tool solves all of these problems - redacting is easier, simpler, quicker, less risky, can be done in anything that edits text, and doesn't affect file size or searchability.

Usage
-----

1. Open the document to be redacted.  Any system that can edit text should work.  For PDFs I open them in LibreOffice, which opens them as Draw documents, which is good enough for this.  Regular LibreOffice text documents should also work.  I haven't tested systems like Google Docs, Adobe Reader, or even LaTeX, but they should also work.

2. Make sure that any "Track Changes" features are disabled.

3. In a terminal, run the script.  You need to specify the font name and size of the text being redacted.  eg.
    ```
    $ redact-magic-paste 'Times 12'
    ```

4. In your document, highlight the text you want to redact.

5. Press Ctrl-C (or whatever) to copy the text.  On X11 this step can be skipped.

6. `redact-magic-paste` will automatically and instantly convert the clipboard contents into a series of Unicode 'FULL BLOCK' ('█', U+2588) characters.  The right number of blocks is chosen so that they have roughly the same width as the original text.

7. Press Ctrl-V (or whatever) to paste.  The highlighted text gets replaced by the solid black blocks.

8. Repeat steps 4-7 for all the redactions you want to make.  Highlight -> Ctrl-C -> Ctrl-V is a quick and convenient core loop for this task (and Highlight -> Ctrl-V in X11 is even nicer).

9. Type Ctrl-C in the terminal to stop the script running.

10. When finished, for systems like Google Docs which always track all changes, save a new copy of the resulting document and confirm that the new document has no history.  Alternatively, copy-paste the entire document into a new blank document.

11. Export to PDF as usual (or whatever).


Notes
-----

Python3 and GTK4 are required.

I have only tested on Ubuntu 22.04 Linux with X11.  Wayland, MacOS, Windows, and any other systems should work, but are completely untested.

Newlines in the selected text aren't handled properly.  For now, just avoid selecting/copying multiple lines (ie. do them one at a time).

LibreOffice has some bug where the X11 primary selection clipboard doesn't get updated properly when click-dragging or using shift-arrows.  In this case, just use Ctrl-C to manually copy the text before pasting the solid blocks with Ctrl-V.

You might sometimes see errors such as below being emitted in the terminal where the script is running; they are safe to ignore.
```
gi.repository.GLib.GError: g-io-error-quark: Format text/plain not supported (1)
Traceback (most recent call last):
  File "/.../redact-magic-paste", line 25, in read_cb
    text = clipboard.read_text_finish(res)
```
