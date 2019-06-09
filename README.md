# Fix Papers 3 export library (XML, RIS, BibTeX) to contain annotation

This notebook is used as a one-off script for me to leave Papers 3. The problem of Papers 3 export is the either you export the library (e.g. in XML) or the PDFs with annotations, but not both.

This notebook fix the situation by "merging" the information from both of these export options from Papers 3. Moreover, it takes the lazy approach that whenever a PDF is not modified, the original (rather than the output from Papers 3) is used to reduced file size inflation. In my test my library inflated ~2.7 times after Papers 3 export with annotation (even for those PDFs without any annotations!)

Configure these in cell 3:

```py
# path to the XML export from Papers 3: "EndNote XML Library"
path_xml = Path('~/Downloads/temp-papers-export-tidy.xml').expanduser()
# export from Papers 3 "PDF Files and Media", without annotation
path_original = Path('~/Downloads/temp-papers-export-original').expanduser()
# export from Papers 3 "PDF Files and Media", with annotation
path_annotated = Path('~/Downloads/temp-papers-export').expanduser()
# input path of the library file
# it can be the same as the path_xml, or another one such as RIS or BibTeX
in_path = path_xml
# output path of the modified library file, extension should be the same as in_path
out_path = Path('~/Downloads/temp-papers-export-annotated.xml').expanduser()
```

The comments should be self-explanatory.

# Requirements

This notebook imports the following

```py
import xmltodict
from glom import glom
import pandas as pd
import fitz # pip install PyMuPDF
```

# Potential Improvements and notes

Hopefully I don't need to run this script anymore, which also means I probably won't maintain it. A few things that could improve this:

Improvements:

- use `argparse` or something like that (e.g. `defopt`) to provide command line options (or chain with `gooey` for GUI)
- use parallel map. A couple of `map` are used throughout and can be trivially parallelized.
- a few assumption made (mainly shown by the `assert` statements), run the notebook to fix your library (or fix my code) to change that

Notes:

- note that if you use APFS, the export of the original export should not takes extra space and is very fast (by the nature of copy in APFS)
- the 2nd last cell shows how many PDFs are modified. If you pre-tidy up your XML input files (or use RIS/BibTeX), then you can take a diff between the input file and output file to verify that the changed paths are of the same no.
- note that the output library has file paths pointing inside both the original Papers 3 library and the `path_annotated` above. Only delete these directories after you use the `out_path` to import into another software. (And leave the Papers 3 library around for a while until you're certain migrated library aren't broke.)

Won't fix:

- collections from Papers 3 are missing. Since the exported library (be it XML, RIS, BibTeX) doesn't include this information, it can't be done. May be AppleScript can help. See <https://github.com/extracts/mac-scripting/blob/master/Papers3/Papers_To_Bookends/Papers_To_Bookends.applescript>.
