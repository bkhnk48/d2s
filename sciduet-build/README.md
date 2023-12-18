## SciDuet
```console
pip install -r requirements.txt
```

### Scrape conference archives
```console
python3 scrape_urls.py
python3 collect_files.py
```
### Import nltk (Option)
```console
python3
>>>import nltk
>>>nltk.download('punkt')
>>>exit()
```

### Paper Extraction: Grobid + PDFFigures 2.0

Follow installation steps from https://grobid.readthedocs.io/en/latest/Install-Grobid/ and https://github.com/kermitt2/grobid_client_python. Please keep in mind that you should already install jdk 1.11 or above.
```console
cd grobid-0.6.2
./gradlew run
```
Then you open a new terminal (or new tab of the current terminal):
```console
cd path/to/grobid-client-python
grobid_client --input path/to/sciduet/data/papers --output path/to/sciduet/teidir --teiCoordinates processFulltextDocument
```
For example (of the grobid_client):
```console
grobid_client --input  ../data/papers/ --output ../teidir --teiCoordinates processFulltextDocument
```
Add here, you create a new folder namely teidir. The folder data/papers already is in sciduet-build.

OPTIONAL: Use pdffigures2 to extract figures and tables from scientific papers (this repo omits automatic figure selection). https://github.com/allenai/pdffigures2
```console
git clone https://github.com/allenai/pdffigures2.git
cd pdffigures2
export JAVA_HOME="/Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home"
sbt "runMain org.allenai.pdffigures2.FigureExtractorBatchCli path/to/sciduet/data/papers/ -s path/to/sciduet/stat_file.json -m sciduet/figure/image/ -d sciduet/figure/data/" 
```

Convert tei.xml files from grobid into json.

```console
python3 extract_papers.py
```

### Slide Extraction: Poppler pdftotext

Original code uses IBM Watson Discovery Package and Tesseract-OCR to extract text from slides.  
We provide code for running the open-source command-line utility _pdftotext_ to achieve comparable results.

```console
python3 extract_slides.py
```


### Merge ACL with supplemental data

Finished ACL data is provided in inputs/. The code in this dir provides functionality for preprocessing ICML and NeurIPS data.
You can supplement ACL however you like. In the paper, all supplemental data are in the train split.

Run the following to merge into inputs/ and resplit the data.

```console
python merge_acl_suppl.py
```
