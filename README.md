# fx3-cmix
The fx2-cmix is a updated implementation of [fx2-cmix](https://github.com/kaitz/fx-cmix). 


# Submission Description

## More detailed changes

### cmix changes:
* Adjusted WUS (weight update skipping). Around ~10% of updates are skipped.
* Windows SFX for testing (PPM in memory)
* Removed 16 predictors
* Removed 4 mixers
* LSTM cell count reduced by ~30.

### fxcm changes:
* Add ~487 predictions to cmix fp mixers
* fxcm has about ~610 predictions
* Known dictionary words are compared with their codeword. Previously, text strings were compared.
* Adjusted global StateMap prediction.
* ContextMap (HT 128) reduced predictions from 6/5 to 5/4 per context. Use single internal StateMap. All context states are update with that.
* ContextMap (HT 32) reduced predictions from 5/4 to 4/3 per context. Removed StateMap based predictions.
* StationaryMap for 2 context
* In WordsContext use also codeword for dictionary word.
* Added SentenceContext for sentance managment. Max 64 sentances (WordsContexts). Search for similarity is performed by compareing codewords (default 53% means match found).
* In stemmer add Pronoun word type.
* Add InDirectStateMap with order-w mixing of primary predictions (similar to Paq9a/zpaq)
* Partial sentance contexts.
* Group of SentenceContexts for: lists ('*'), table, wikilinks and regular sentances. Total 4.
* Removed SparseMatchModel.
* Removed 4 SmallStationaryContextMap contexts
* Mixer count from 12 to 24
* Added 7 new ContextMap's
* Added 22 new InDirectStateMap contexts
* Adjusted mixer parameters and contexts
* Adjusted ContextMap memory usage
* There are 3 mixers layers (+1 in every InDirectStateMap).
* For layer 0 mixers about ~40% of updates are skipped.
* Some predictions are skipped if line is Category link, after topic 'See also', 'References', 'Bibliography' or 'External links'.
* Some low memory ContextMaps are reset after every page (wikipedia article). The StateMap is preserved if it exists.

## Article order:
* Moved all articles with title 'Wikipedia:' after images.

# Authors
* Kaido Orav

# Google Cloud Compute Engine parameters

# Results

# Time

# Instructions
The installation and usage instructions for fx2-cmix are the same as for fast-cmix.

One important note: it is recommended to change one variable in the source code for PPM. From line 26 in src/models/ppmd.cpp:

```
// If mmap_to_disk is set to false (recommended setting), PPM will only use RAM
// for memory.
// If mmap_to_disk is set to true, PPM memory will be saved to disk using mmap.
// This will reduce RAM usage, but will be slower as well. *Warning*: this will
// write a *lot* of data to disk, so can reduce the lifespan of SSDs. Not
// recommended for normal usage.
bool mmap_to_disk = true;
```

This variable is set to true by default, to comply with the Hutter Prize RAM limit.

# Installing packages required for compiling fx2-cmix compressor from sources on Ubuntu
Building fx2-cmix compressor from sources requires clang-17, upx-ucl, and make packages.
On Ubuntu, these packages can be installed by running the following scripts:
```bash
./install_tools/install_upx.sh
./install_tools/install_clang-17.sh
```

# Compiling fx2-cmix compressor from sources
A bash script is provided for compiling fx2-cmix compressor from sources on Ubuntu. This script places the fx2-cmix executable file named as `cmix` in `./run` directory. The script can be run as
```bash
./build_and_construct_comp.sh
```

# Running fx2-cmix compressor
To run the cmix-hp compressor use
```bash
cd ./run
cmix -e <PATH_TO_ENWIK9> enwik9.comp
```
`enwik9.comp` is used to store intermediate data output. The final decompressor will be created as a file named `archive9`.

# Running fx2-cmix decompressor
The compressor is expected to output an executable file named `archive9` in the same directory (`./run`). The file `archive9` when executed is expected to reproduce the original enwik9 as a file named `enwik9_restored`. The executable file `archive9` should be launched without argments from the directory containing it.
```bash
cd ./run
./archive9
```

# Expected output on compression


# Expected output on decompression
