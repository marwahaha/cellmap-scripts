## Interaction assessment

### Recovered ineractions per FDR cutoff

This script will take all interaction pairs (BioGRID, Intact or merged). For each FDR probability it will calculate the percentage of recovered known interactions.

Requires:
* SAINT file
* interaction file from BioGRID, Intact or merged
* optional: a list of baits to include in analysis (default will assume all baits)
* bait to gene name map

| bait  | gene  |
|-------|-------|
| bait1 | genea |
| bait2 | geneb |
| ...   | ...   |

1. Run script
```
"$CMSCRIPTS"/interaction-assessment/complex-validation.pl -b interactions.txt -m bait-gene.txt -s saint.txt
```

2.	Output:
* `fraction-recovered.txt`

### Bait assessment

Generate a bait interaction summary of known and new interactors using an FDR of 0.01 when comparing against BioGrid, Intact or merged.

Requires
* SAINT file
* interaction file from BioGRID, Intact or merged
* FDR cutoff, default 0.01
* bait to gene name map

| bait  | gene  |
|-------|-------|
| bait1 | genea |
| bait2 | geneb |
| ...   | ...   |

1. Run script
```
"$CMSCRIPTS"/interaction-assessment/bait-summary.pl -b interactions.txt -m bait-gene.txt -s saint.txt
```

2.	Output:
* bait-level-recovery.txt
* edge-recovered.txt for plotting known bait-prey interaction partners on Cytoscape

### Bait assessment, top 25 preys by abudance

* SAINT file
* interaction file from BioGRID, Intact or merged
* FDR cutoff, default 0.01
* adjust spectral counts to length, default is yes
* subtract control average from spectral counts, default is yes
* number of top preys to use, default is 25
* bait to gene name map

| bait  | gene  |
|-------|-------|
| bait1 | genea |
| bait2 | geneb |
| ...   | ...   |

1. Run script
```
"$CMSCRIPTS"/interaction-assessment/bait-summary-abundance.pl -b interactions.txt -m bait-gene.txt -s saint.txt
```

2.	Output:
* `bait-level-recovery_top25_lengthAdjusted.txt`

### Bait assessment, top 25 preys by specificity

* SAINT file
* interaction file from BioGRID, Intact or merged
* FDR cutoff, default 0.01
* number of top preys to use, default is 25
* bait to gene name map

| bait  | gene  |
|-------|-------|
| bait1 | genea |
| bait2 | geneb |
| ...   | ...   |

1. Run script
```
"$CMSCRIPTS"/interaction-assessment/bait-summary-specificity.pl -b interactions.txt -m bait-gene.txt -s saint.txt
```

2.	Output:
* `bait-level-recovery_top25_specificity.txt`