# Pandoc

Pandoc conversion from Microsoft Word to Markdown:

```bash
myfilename="aprs101.docx"
pandoc -t markdown_strict --extract-media='_static/$myfilename' $myfilename.docx -o $myfilename.md
```
