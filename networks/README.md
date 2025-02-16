## Networks

### Summary files for ranks/domains

Generate ranks.json/domains.json file that contains all details about each rank/domain, including its listed name on the cellmap, matching go term etc, all GO terms with p-values and domains with p-values

Requires:
* summary file
  * rank-summary.txt
  * domain-summary.txt
* GO terms for eacn rank with (and p-values for NMF)
 * SAFE:
  * terms_perdomain.txt
  * this used to use attribute_properties_annotation-highest.txt (need to delete the first 4 rows of this file)
 * NMF: terms_perrank.txt
* Domain, motif and/or diesease file associated with each rank
* NMF
  * a file with the order for ranks (rank_order.txt)
  * a file with top value for each rank (top-rank.txt)
* SAFE
  * a map of go terms to ids (go-map.txt), produced from GO formatting step.
* Starting index for color array (should be set to 0 for SAFE and 1 for NMF)

1. Run script

NMF
```
"$CMSCRIPTS"/network/rank-profile.pl -g terms_perrank.txt -r rank-summary.txt -s rank_order.txt -t n -diseases diseases.txt -domains domains.txt -motifs motifs.txt -b top-rank.txt -c 1
```

SAFE
```
"$CMSCRIPTS"/network/rank-profile.pl -g terms_perrank.txt -o go-map.txt -r domain-summary.txt -t s -diseases diseases.txt -domains domains.txt -motifs motifs.txt -c 0
```

2. Output
* `ranks.json`: NMF rank details
* `domains.json`: SAFE domain details

### NMF tSNE network

Requires:
* coordinates file from tsne (tsne_nmf.txt)
* file with rank assigned to each gene (gene-localizations.txt)
* ranks.json file
* SAINT file (to get list of baits)
* annotations matrix used for SAFE (make sure this matches GO namespace of network). This file indicates whether a GO term is known or not for each gene (use this as opposed to goa-human.gaf because it will contain all parents as well as the GO terms themselves)

1. Run script
```
"$CMSCRIPTS"/network/network-tsne.pl -a cc_annotations.txt -g gene-localization.txt -n tsne_nmf.txt -r ranks.json -s saint.txt -t n
```

2. Output
* nmf-tsne-network-legend.json
* nmf-tsne-network.json

### Correlation network

Requires:
* network in .cyjs format
* file with rank/domain assigned to each gene
  * NMF: gene-localizations.txt
  * SAFE: node_properites_annotation-highest.txt, need to remove top 4 lines
* ranks.json file
* SAINT file (to get list of baits)
* annotations matrix used for SAFE (make sure this matches GO namespace of network). This file indicates whether a GO term is known or not for each gene (use this as opposed to goa-human.gaf because it will contain all parents as well as the GO terms themselves)
* Starting index for color array (should be set to 0 for SAFE and 1 for NMF)
* prefix for output file (nmf-corr or safe-corr-0.6, etc)

1. Run script
```
"$CMSCRIPTS"/network/network-corr.pl -a cc_annotations.txt -g gene-localization.txt -n nmf.cyjs -r ranks.json -s saint.txt -t n -c 1 -o "nmf-corr"
```

2. Output
* corr-network-legend.json
* corr-network.json

### Minify networks for website

#### Manually

1. Minify any network using jq (https://github.com/stedolan/jq)
```
jq -c . < newtwork.json > newtwork.min.json
```

#### Batch

This script will minify all json files in a folder, excluding those ending in .min.json

```
"$CMSCRIPTS"/network/minify-network.pl
```