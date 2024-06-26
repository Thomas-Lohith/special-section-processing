# Content

The scripts in this repository allow the analysis of the abstracts of the articles of the Special Sections of a scientific journal.
The scripts in this repository allow the analysis of the abstracts of conference track. (in this README "track" and "Special Section" are used as synonyms)
The abstracts are processed with the [`slr-kit`](https://github.com/robolab-pavia/slr-kit) tool.
Each script can be run independently of the others.
The scripts are presented in the order of execution expected during the workflow.

## `Scopus2csv.py`
## `cleaningETFACsv.py`

Converts a CSV file exported from Scopus into a `slr-kit` compatible CSV file.
Removes papers we are not interested for.

* INPUT: CSV file exported from Scopus
* OUTPUT: CSV file `slr-kit_abstracts.csv` in `slr-kit` compatible format
- INPUT: CSV file containing all papers
- OUTPUT: CSV file containing the papers we are interested for

Positional arguments:

* `input_file`: CSV exported from Scopus
* `file`: CSV file containing all papers

Example of usage:

```
python Scopus2csv.py Scopus.csv
python3 cleaningETFACsv.py "conference file".csv
```

## `cleaningINDINCsv.py`

Removes papers we are not interested for.

- INPUT: CSV file containing all papers
- OUTPUT: CSV file containing the papers we are interested for

Positional arguments:

* `file`: CSV file containing all papers

Example of usage:

```
python3 cleaningINDINCsv.py "conference file".csv
```

## `PreprocBySpecSec.py`

Splits the articles preprocessed by `slr-kit` according to the Special Section they belong to.
Splits the articles postprocessed by `slr-kit` according to the track they belong to.

- INPUT: CSV file containing articles and names of the Special Sections; CSV file containing the items preprocessed by `slr-kit`
- OUTPUT: set of folders containing the preprocessed articles divided by Special Section; executable file named `run_all_process.bat` to be able to postprocess articles and then LDA (postprocessing and LDA are provided by `slr-kit`)
- INPUT: CSV file containing the items postprocessed by `slr-kit`
- OUTPUT: set of folders containing the postprocessed articles divided by Special Section

Positional arguments:

* `spec_sec_csv`: CSV file containing articles and names of the Special Sections
* `preproc_file`: CSV file containing the preprocessed articles of all the Special Sections
* `spec_sec_csv`: CSV file containing the postprocessed articles of all the Special Sections

Example of usage:

```
python PreprocBySpecSec.py Spec_Sec.csv SpecSec_preproc.csv
python3 PreprocBySpecSec.py "conference name"_postproc.csv
```

## `SpecSecFake.py`
@@ -59,7 +75,7 @@ Positional arguments:
Example of usage:

```
python SpecSecFake.py (Get-ChildItem -Path "SpecSec\SpecSec*").FullName SpecSec_postproc.csv
python3 SpecSecFake.py SpecSec/SpecSec* "conference name"_postproc.csv
```

## `SpecSecHist.py`
@@ -76,8 +92,8 @@ Positional arguments:
Example of usage:

```
python SpecSecHist.py (Get-ChildItem -Path "SpecSec\SpecSec*").FullName
python SpecSecHist.py (Get-ChildItem -Path "SpecSecFake\SpecSecFake*").FullName
python3 SpecSecHist.py SpecSec/SpecSec*
python3 SpecSecHist.py SpecSecFake/SpecSecFake*
```

## `SpecSecGraph.py`
@@ -96,8 +112,8 @@ Positional arguments:
Example of usage:

```
python SpecSecGraph.py (Get-ChildItem -Path "SpecSec\SpecSec*").FullName
python SpecSecGraph.py (Get-ChildItem -Path "SpecSecFake\SpecSecFake*").FullName
python3 SpecSecGraph.py SpecSec/SpecSec*
python3 SpecSecGraph.py SpecSecFake/SpecSecFake*
```

## `SpecSecElab.py`
@@ -119,7 +135,7 @@ Positional arguments:
Example of usage:

```
python SpecSecElab.py --spec_sec (Get-ChildItem -Path "SpecSec\SpecSec*").FullName --spec_sec_fake (Get-ChildItem -Path "SpecSecFake\SpecSecFake*").FullName
python3 SpecSecElab.py --spec_sec SpecSec/SpecSec* --spec_sec_fake SpecSecFake/SpecSecFake*
```

## `SpecSecBoxPlot.py`
@@ -137,7 +153,7 @@ Positional arguments:
Example of usage:

```
python SpecSecBoxPlot.py Spec_Sec_metrics.csv Spec_Sec_fake_metrics.csv
python3 SpecSecBoxPlot.py Spec_Sec_metrics.csv Spec_Sec_fake_metrics.csv
```

## `SpecSecPlot.py`
@@ -155,5 +171,91 @@ Positional arguments:
Example of usage:

```
python SpecSecPlot.py Spec_Sec_metrics.csv Spec_Sec_fake_metrics.csv
python3 SpecSecPlot.py Spec_Sec_metrics.csv Spec_Sec_fake_metrics.csv
```

## `TrackAsPaper.py`

Merges every paper under a track in a single "track paper" and puts every "track paper" in a CSV file.

* INPUT: folders containing the postprocessed articles divided by Special Section
* OUTPUT: CSV file called `Biggest_Paper.csv` containing track papers (only 'id' and 'abstract_filtered' columns)

Positional arguments:

* `directories_special_section`: list of Special Section folders

Example of usage:

```
python3 TrackAsPaper.py SpecSec/SpecSec*
```

## `TrackCluster.py`

Creates clusters between track papers and an histogram of the intersections between track papers.

* INPUT: CSV file containing track papers generated by `TrackAsPaper.py`
* OUTPUT: graph named `BiggestPaper_graph.png` and histogram named `intersection_histogram.png`

Positional arguments:

* `file`: CSV file containing track papers

Example of usage:

```
python3 TrackCluster.py Biggest_Paper.csv
```

## `matrix.py`

Creates a CSV file which has in the first column every unique words among every track paper and in the others columns the frequency of that word in each track.

* INPUT: CSV file containing track papers generated by `TrackAsPaper.py`
* OUTPUT: CSV file called `matrix.csv` which has in the first column every unique words among every track paper and in the others columns the frequency of that word in each track

Positional arguments:

* `file`: CSV file containing track papers

Example of usage:

```
python3 matrix.py Biggest_Paper.csv
```

## `clusterAfterMatrix.py`

Creates clusters based on the matrix between words and track papers and an histogram of the intersections between track papers with the gaussian test.

* INPUT: CSV file which is the matrix generated by `matrix.py`
* OUTPUT: graph named `clusterOnMatrix.png` and histogram named `after_matrix_intersection_histogram.png`

Positional arguments:

* `file`: matrix CSV file

Example of usage:

```
python3 clusterAfterMatrix.py matrix.csv
```

## `classificationMismatch.py`

Creates a new CSV file with mismatches between the terms classification made by an expert and mine.

* INPUT: CSV file with the expert terms classification; CSV file with my terms classification
* OUTPUT: CSV with mismatches named `file_to_check.csv`

Positional arguments:

* `checked_file`: CSV file with the expert terms classification
* `file_to_check`: CSV file with my terms classification

Example of usage:

```
python3 classificationMismatch.py "expert's classification".csv "my classification".csv
```


## `h_index.py`

Calculates the h-index of tracks and intersections and plots everything in a graph which contains the histogram, the curve and the h-index.

* INPUT: CSV file which is the matrix generated by `matrix.py`
* OUTPUT: graph named `h_index.png`

Positional arguments:

* `file`: matrix CSV file

Example of usage:

```
python3 h_index.py matrix.csv

```
