[![GloBI Review by Elton](../../actions/workflows/review.yml/badge.svg)](../../actions/workflows/review.yml) [![GloBI](https://api.globalbioticinteractions.org/interaction.svg?accordingTo=globi:globalbioticinteractions/eupollinationhub&refutes=true&refutes=false)](https://globalbioticinteractions.org/?accordingTo=globi:globalbioticinteractions/eupollinationhub)

Configuration to help Global Biotic Interactions (GloBI, https://globalbioticinteractions.org) index: 

Rubinigg M. 2023 Data on economic dependence of pollination by animals in crops as well as observations of pollinators on plant species, inon economic dependence of pollination by animals in crops as well as observations of pollinators on plant species, in particular crops. EU Pollinator Hub. [2025-08-28] app.pollinatorhub.eu

This is one of many datasets available through https://app.pollinatorhub.eu .

## Provenance

The indexed dataset by Rubinigg 2023 was found via

```
preston track https://app.pollinatorhub.eu/api/v1/discovery/datasets\
 | grep hasVersion\
 | preston cat\
 | jq -c '.data[]'\
 > datasets.json
```

with the Preston BOM (Bill of Material) retrieved via 

```
preston head
```

being 

```
hash://sha256/b0adc21a6830af0f9f4601fefece84a3962d53c5c2ae6fa32e7ae88381dd6349
```

With tracking of individual dataset metadata through:

```
preston track\
 -f <(\
preston cat hash://sha256/b0adc21a6830af0f9f4601fefece84a3962d53c5c2ae6fa32e7ae88381dd6349\
 | grep hasVersion\
 | preston cat\
 | jq --raw-output '.data[].links.self.href'\
)
```

resulting in a Preston BOM (Bill of Material) ```preston head``` being:

```
hash://sha256/14cc0975aab4c441923aeb187323505888bef22175987c63d3b08a25762cd6ca
```

and

```
preston head\
 | preston cat\
 | grep hasVersion\
 | preston cat\
 | jq -c . \
> datasets-meta.json
```

Now, the list of datasets was narrowed by searching for "Pollination", through

```
cat datasets-meta.json\
  | grep Pollination\
 | jq --raw-output .data.parts[].data_files[].name
```

yielding the following table names:

```
crop_species.csv
insect_plant.csv
pollination_dependency.csv
imagelist.csv
```

then, a manual download was initiated after consulting https://app.pollinatorhub.eu/dataset-discovery/PLLNT11.0.0 via

```
preston track "https://app.pollinatorhub.eu/download/csv/11/75?expires=1756337079&signature=52470730dfe7a008e879fa45c35cf7715bb6b2d1efbff2b477b5f973001e52e4"
```

resulting in the associated data file:

```
preston cat\
 hash://sha256/1b4d590622d4764c443dd0bb7437b93e329c514566de5259591b10a6cc54f34e
 | preston cat\
 > Insect_Plant.csv
```

The bibliographic citations were extracted into biblio.bib via:

```
cat Insect-Plant.csv\
 | mlr --icsv --otsvlite cut -f dcterms:bibliographicCitation\
 | tail -n+2\
 | sort\
 | uniq\
 | sed 's/^article/@article/g'\
 > biblio.bib
```

An alternate way to get the data (up to 1000 rows) by using endpoint (or path) 

```
https://app.pollinatorhub.eu/api/v1/discovery/data/PLLNT11.NSCTP75.0
```

where ```PLLNT11.NSCTP75.0``` is the uid of the data part 75 of dataset 11. 


See https://github.com/globalbioticinteractions/globalbioticinteractions/issues/991#issuecomment-3228566047 . 

