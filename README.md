# standford-ner

## Download

Link [here](https://nlp.stanford.edu/software/CRF-NER.html#Download).

## Run

### Given Example

With the given sample `stanford-ner-2018-02-27/sample.txt`:

```bash
$ cat stanford-ner-2018-02-27/sample.txt
```

Output:

```
The fate of Lehman Brothers, the beleaguered investment bank, hung in the balance on Sunday as Federal Reserve officials and the leaders of major financial institutions continued to gather in emergency meetings trying to complete a plan to rescue the stricken bank.  Several possible plans emerged from the talks, held at the Federal Reserve Bank of New York and led by Timothy R. Geithner, the president of the New York Fed, and Treasury Secretary Henry M. Paulson Jr.
```

Execute:

```bash
$ sh stanford-ner-2018-02-27/ner.sh  stanford-ner-2018-02-27/sample.txt
```

Output:

```
Invoked on Fri Apr 27 16:30:15 MYT 2018 with arguments: -loadClassifier stanford-ner-2018-02-27/classifiers/english.all.3class.distsim.crf.ser.gz -textFile stanford-ner-2018-02-27/sample.txt
loadClassifier=stanford-ner-2018-02-27/classifiers/english.all.3class.distsim.crf.ser.gz
textFile=stanford-ner-2018-02-27/sample.txt
Loading classifier from stanford-ner-2018-02-27/classifiers/english.all.3class.distsim.crf.ser.gz ... done [2.0 sec].
The/O fate/O of/O Lehman/ORGANIZATION Brothers/ORGANIZATION ,/O the/O beleaguered/O investment/O bank/O ,/O hung/O in/O the/O balance/O on/O Sunday/O as/O Federal/ORGANIZATION Reserve/ORGANIZATION officials/O and/O the/O leaders/O of/O major/O financial/O institutions/O continued/O to/O gather/O in/O emergency/O meetings/O trying/O to/O complete/O a/O plan/O to/O rescue/O the/O stricken/O bank/O ./O
Several/O possible/O plans/O emerged/O from/O the/O talks/O ,/O held/O at/O the/O Federal/ORGANIZATION Reserve/ORGANIZATION Bank/ORGANIZATION of/ORGANIZATION New/ORGANIZATION York/ORGANIZATION and/O led/O by/O Timothy/PERSON R./PERSON Geithner/PERSON ,/O the/O president/O of/O the/O New/ORGANIZATION York/ORGANIZATION Fed/ORGANIZATION ,/O and/O Treasury/ORGANIZATION Secretary/O Henry/PERSON M./PERSON Paulson/PERSON Jr./PERSON ./O
```

### Test Example


Validate: 


```bash
$ cat sample.txt
```

Output:

```txt
Paper and Pen worked in Google for 10 years
```

Execute:

```bash
$ sh stanford-ner-2018-02-27/ner.sh sample.txt
```

Output:

```bash
Invoked on Fri Apr 27 16:32:40 MYT 2018 with arguments: -loadClassifier stanford-ner-2018-02-27/classifiers/english.all.3class.distsim.crf.ser.gz -textFile sample.txt
loadClassifier=stanford-ner-2018-02-27/classifiers/english.all.3class.distsim.crf.ser.gz
textFile=sample.txt
Loading classifier from stanford-ner-2018-02-27/classifiers/english.all.3class.distsim.crf.ser.gz ... done [1.8 sec].
Paper/O and/O Pen/PERSON worked/O in/O Google/ORGANIZATION for/O 10/O years/O
CRFClassifier tagged 9 words in 1 documents at 97.83 words per second.
```

## Training own example

https://nlp.stanford.edu/software/crf-faq.shtml


### Tokenize

```bash
$ java -cp stanford-ner-2018-02-27/stanford-ner.jar edu.stanford.nlp.process.PTBTokenizer jane-austen-emma-ch1.txt > jane-austen-emma-ch1.tok
```

Output:

```
PTBTokenizer tokenized 3348 tokens at 30010,58 tokens per second.
```

### Create TSV

```bash
$ perl -ne 'chomp; print "$_\tO\n"' jane-austen-emma-ch1.tok > jane-austen-emma-ch1.tsv
```


### Actual Training

```bash
$ java -cp stanford-ner-2018-02-27/stanford-ner.jar edu.stanford.nlp.ie.crf.CRFClassifier -prop train.prop
```

Output:

```bash
Invoked on Fri Apr 27 16:45:00 MYT 2018 with arguments: -prop train.prop
usePrevSequences=true
useClassFeature=true
useTypeSeqs2=true
useSequences=true
wordShape=chris2useLC
useTypeySequences=true
useDisjunctive=true
noMidNGrams=true
serializeTo=ner-model.ser.gz
maxNGramLeng=6
useNGrams=true
usePrev=true
useNext=true
maxLeft=1
trainFile=jane-austen-emma-ch1.tsv
map=word=0,answer=1
useWord=true
useTypeSeqs=true
numFeatures = 18344
Time to convert docs to feature indices: 0.3 seconds
numClasses: 2 [0=O,1=PERS]
numDocuments: 1
numDatums: 3953
numFeatures: 18344
Time to convert docs to data/labels: 0.1 seconds
numWeights: 46850
QNMinimizer called on double function of 46850 variables, using M = 25.
               An explanation of the output:
Iter           The number of iterations
evals          The number of function evaluations
SCALING        <D> Diagonal scaling was used; <I> Scaled Identity
LINESEARCH     [## M steplength]  Minpack linesearch
                   1-Function value was too high
                   2-Value ok, gradient positive, positive curvature
                   3-Value ok, gradient negative, positive curvature
                   4-Value ok, gradient negative, negative curvature
               [.. B]  Backtracking
VALUE          The current function value
TIME           Total elapsed time
|GNORM|        The current norm of the gradient
{RELNORM}      The ratio of the current to initial gradient norms
AVEIMPROVE     The average improvement / current value
EVALSCORE      The last available eval score

Iter ## evals ## <SCALING> [LINESEARCH] VALUE TIME |GNORM| {RELNORM} AVEIMPROVE EVALSCORE

Iter 1 evals 1 <D> [11M 8.925E-5] 1.030E4 0.11s |3.542E3| {6.178E-1} 0.000E0 -
Iter 2 evals 4 <D> [M 1.000E0] 8.662E3 0.14s |6.243E2| {1.089E-1} 9.467E-2 -
Iter 3 evals 5 <D> [M 1.000E0] 8.548E3 0.16s |4.675E2| {8.154E-2} 6.840E-2 -
Iter 4 evals 6 <D> [M 1.000E0] 8.237E3 0.18s |3.730E2| {6.505E-2} 6.265E-2 -
Iter 5 evals 7 <D> [M 1.000E0] 7.807E3 0.20s |1.742E2| {3.038E-2} 6.390E-2 -
Iter 6 evals 8 <D> [M 1.000E0] 7.620E3 0.22s |1.319E2| {2.300E-2} 5.866E-2 -
Iter 7 evals 9 <D> [M 1.000E0] 7.168E3 0.24s |1.207E2| {2.105E-2} 6.245E-2 -
Iter 8 evals 10 <D> [M 1.000E0] 4.483E3 0.27s |9.737E1| {1.698E-2} 1.622E-1 -
Iter 9 evals 11 <D> [M 1.000E0] 1.903E3 0.28s |6.892E1| {1.202E-2} 4.905E-1 -
Iter 10 evals 12 <D> [M 1.000E0] 1.654E3 0.30s |5.755E1| {1.004E-2} 5.227E-1 -
Iter 11 evals 13 <D> [M 1.000E0] 8.832E2 0.32s |6.511E1| {1.136E-2} 8.807E-1 -
Iter 12 evals 14 <D> [1M 5.867E-2] 7.732E2 0.37s |5.461E1| {9.525E-3} 1.005E0 -
Iter 13 evals 16 <D> [1M 7.741E-2] 6.673E2 0.41s |5.041E1| {8.793E-3} 1.134E0 -
Iter 14 evals 18 <D> [1M 1.585E-1] 5.206E2 0.44s |3.899E1| {6.801E-3} 1.400E0 -
Iter 15 evals 20 <D> [1M 8.830E-2] 4.658E2 0.47s |3.834E1| {6.688E-3} 1.536E0 -
Iter 16 evals 22 <D> [1M 1.761E-1] 3.942E2 0.50s |3.469E1| {6.050E-3} 1.718E0 -
Iter 17 evals 24 <D> [1M 1.590E-1] 3.547E2 0.54s |3.934E1| {6.862E-3} 1.164E0 -
Iter 18 evals 26 <D> [1M 1.943E-1] 3.071E2 0.57s |4.739E1| {8.265E-3} 5.196E-1 -
Iter 19 evals 28 <D> [M 1.000E0] 2.624E2 0.59s |3.307E1| {5.768E-3} 5.304E-1 -
Iter 20 evals 29 <D> [M 1.000E0] 2.443E2 0.61s |6.867E1| {1.198E-2} 2.616E-1 -
Iter 21 evals 30 <D> [M 1.000E0] 2.275E2 0.63s |3.593E1| {6.267E-3} 2.399E-1 -
Iter 22 evals 31 <D> [M 1.000E0] 2.083E2 0.65s |3.005E1| {5.242E-3} 2.204E-1 -
Iter 23 evals 32 <D> [M 1.000E0] 1.651E2 0.67s |7.884E1| {1.375E-2} 2.154E-1 -
Iter 24 evals 33 <D> [M 1.000E0] 1.261E2 0.69s |2.680E1| {4.674E-3} 2.694E-1 -
Iter 25 evals 34 <D> [M 1.000E0] 9.698E1 0.71s |2.318E1| {4.043E-3} 3.065E-1 -
Iter 26 evals 35 <D> [M 1.000E0] 7.305E1 0.73s |2.587E1| {4.512E-3} 3.855E-1 -
Iter 27 evals 36 <D> [M 1.000E0] 6.099E1 0.75s |1.203E1| {2.098E-3} 4.035E-1 -
Iter 28 evals 37 <D> [M 1.000E0] 5.038E1 0.77s |1.077E1| {1.879E-3} 4.210E-1 -
Iter 29 evals 38 <D> [M 1.000E0] 4.271E1 0.81s |2.585E1| {4.509E-3} 4.720E-1 -
Iter 30 evals 39 <D> [M 1.000E0] 3.327E1 0.83s |1.007E1| {1.756E-3} 5.838E-1 -
Iter 31 evals 40 <D> [M 1.000E0] 2.718E1 0.85s |7.592E0| {1.324E-3} 6.664E-1 -
Iter 32 evals 41 <D> [M 1.000E0] 2.308E1 0.87s |6.888E0| {1.201E-3} 6.152E-1 -
Iter 33 evals 42 <D> [M 1.000E0] 2.059E1 0.89s |6.184E0| {1.079E-3} 5.123E-1 -
Iter 34 evals 43 <D> [2M 5.003E-1] 1.920E1 0.92s |5.342E0| {9.317E-4} 4.051E-1 -
Iter 35 evals 45 <D> [M 1.000E0] 1.845E1 0.94s |1.070E1| {1.867E-3} 2.959E-1 -
Iter 36 evals 46 <D> [M 1.000E0] 1.688E1 0.96s |3.501E0| {6.106E-4} 2.613E-1 -
Iter 37 evals 47 <D> [M 1.000E0] 1.612E1 0.98s |3.287E0| {5.734E-4} 2.125E-1 -
Iter 38 evals 48 <D> [M 1.000E0] 1.536E1 1.00s |4.514E0| {7.874E-4} 1.780E-1 -
Iter 39 evals 49 <D> [M 1.000E0] 1.476E1 1.03s |2.773E0| {4.837E-4} 1.254E-1 -
Iter 40 evals 50 <D> [M 1.000E0] 1.446E1 1.05s |1.637E0| {2.854E-4} 8.799E-2 -
Iter 41 evals 51 <D> [M 1.000E0] 1.422E1 1.07s |1.587E0| {2.768E-4} 6.229E-2 -
Iter 42 evals 52 <D> [M 1.000E0] 1.413E1 1.08s |1.030E0| {1.797E-4} 4.575E-2 -
Iter 43 evals 53 <D> [M 1.000E0] 1.407E1 1.10s |6.003E-1| {1.047E-4} 3.645E-2 -
Iter 44 evals 54 <D> [M 1.000E0] 1.405E1 1.13s |6.294E-1| {1.098E-4} 3.137E-2 -
Iter 45 evals 55 <D> [M 1.000E0] 1.403E1 1.15s |3.767E-1| {6.571E-5} 2.030E-2 -
Iter 46 evals 56 <D> [M 1.000E0] 1.402E1 1.17s |1.537E-1| {2.682E-5} 1.494E-2 -
Iter 47 evals 57 <D> [M 1.000E0] 1.402E1 1.19s |9.200E-2| {1.605E-5} 9.584E-3 -
Iter 48 evals 58 <D> [M 1.000E0] 1.402E1 1.21s |7.287E-2| {1.271E-5} 5.290E-3 -
Iter 49 evals 59 <D> [M 1.000E0] 1.402E1 1.23s |2.389E-2| {4.167E-6} 3.126E-3 -
Iter 50 evals 60 <D> [M 1.000E0] 1.402E1 1.25s |1.532E-2| {2.673E-6} 1.466E-3 -
Iter 51 evals 61 <D> [M 1.000E0] 1.402E1 1.27s |1.154E-2| {2.013E-6} 7.875E-4 -
Iter 52 evals 62 <D> [M 1.000E0] 1.402E1 1.29s |6.196E-3| {1.081E-6} 3.883E-4 -
Iter 53 evals 63 <D> [M 1.000E0] 1.402E1 1.32s |3.105E-3| {5.415E-7} 2.005E-4 -
QNMinimizer terminated due to average improvement: | newest_val - previous_val | / |newestVal| < TOL
Total time spent in optimization: 1.34s
CRFClassifier training ... done [1.9 sec].
Serializing classifier to ner-model.ser.gz... done.
```

### Validate

Modify the `sample.txt` to contain the following:

```txt
Paper and Pen and Emma Woodhouse worked in Google for 10 years
```

Run:

```bash
$ sh stanford-ner-2018-02-27/ner.sh sample.txt
```

Output:

```bash
Invoked on Fri Apr 27 16:46:20 MYT 2018 with arguments: -loadClassifier stanford-ner-2018-02-27/classifiers/english.all.3class.distsim.crf.ser.gz -textFile sample.txt
loadClassifier=stanford-ner-2018-02-27/classifiers/english.all.3class.distsim.crf.ser.gz
textFile=sample.txt
Loading classifier from stanford-ner-2018-02-27/classifiers/english.all.3class.distsim.crf.ser.gz ... done [1.9 sec].
Paper/O and/O Pen/PERSON and/O Emma/PERSON Woodhouse/PERSON worked/O in/O Google/ORGANIZATION for/O 10/O years/O
CRFClassifier tagged 12 words in 1 documents at 123.71 words per second.
```

From the output, we verify that *Emma Woodhouse* is tagged as a *PERSON* successfully.

## Classes Available

### From NLTK

```bash
ORGANIZATION - Georgia-Pacific Corp., WHO
PERSON - Eddy Bonte, President Obama
LOCATION - Murray River, Mount Everest
DATE - June, 2008-06-29
TIME - two fifty a m, 1:30 p.m.
MONEY - 175 million Canadian Dollars, GBP 10.40
PERCENT - twenty pct, 18.75 %
FACILITY - Washington Monument, Stonehenge
GPE - South East Asia, Midlothian  
```

### From Spacy

https://spacy.io/usage/linguistic-features#named-entities

```bash
PERSON	People, including fictional.
NORP	Nationalities or religious or political groups.
FACILITY	Buildings, airports, highways, bridges, etc.
ORG	Companies, agencies, institutions, etc.
GPE	Countries, cities, states.
LOC	Non-GPE locations, mountain ranges, bodies of water.
PRODUCT	Objects, vehicles, foods, etc. (Not services.)
EVENT	Named hurricanes, battles, wars, sports events, etc.
WORK_OF_ART	Titles of books, songs, etc.
LAW	Named documents made into laws.
LANGUAGE	Any named language.
DATE	Absolute or relative dates or periods.
TIME	Times smaller than a day.
PERCENT	Percentage, including "%".
MONEY	Monetary values, including unit.
QUANTITY	Measurements, as of weight or distance.
ORDINAL	"first", "second", etc.
CARDINAL	Numerals that do not fall under another type.
```