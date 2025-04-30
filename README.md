# <a name="documentation"></a>Human metapneumovirus (HMPV) genome annotation

## [How to annotate HMPV genomes with VADR](#howto)

## [HMPV VADR models](#hmpvmodel)

## [Additional VADR documentation](#docs)

## [References](#reference)


---
## <a name="howto"></a>How to annotate HMPV genomes with VADR

Installation instructions:

**1. Install VADR**

Option A: Use a pre-built Docker image
   
   You can use the [StaPH-B Docker image](https://github.com/StaPH-B/docker-builds/tree/master/vadr/1.6.3-hav-flu2) for VADR 1.6.3-hav-flu2 created by Curtis Kapsak (docker image names:
   `staphb/vadr:1.6.3-hav-flu2` and `staphb/vadr:latest`).
   This is available from: 
   [DockerHub](https://hub.docker.com/r/staphb/vadr/tags)
   [Quay](https://quay.io/repository/staphb/vadr?tab=tags)
   You can pull the image using:
    ```
    docker pull --platform linux/amd64 staphb/vadr:1.6.3-hav-flu2
    ```

Option B: Install VADR from source
   
   Alternatively, yo can download and install the latest version of VADR, following the
   instructions on the [VADR GitHub](https://github.com/ncbi/vadr/tree/master).

**2. Download the HMPV VADR Model**

   Clone the latest HMPV VADR model (release v1.0)
   <br/>
   `git clone https://github.com/greninger-lab/vadr-models-hmpv.git`
   <br/>

**3. Run HMPV annotation**
Note: Nucleotide sequences must be in **FASTA format** and should not be aligned. The software only recognizes IUPAC nucleotide codes and does not accept symbols such as - (which indicate deletions in alingments).
Remove any **terminal** ambiguous nucleotides (e.g. "N") which typically represent regions with no sequencing coverage. You can use teh script `fasta-trim-terminal-ambigs.pl` located in `$VADRSCRIPTSDIR/miniscripts/` to clean your sequences accoridngly.
   To remove too short and too long sequences to create a new trimmed file `<trimmed-fasta-file>`, execute:

```
$VADRSCRIPTSDIR/miniscripts/fasta-trim-terminal-ambigs.pl --minlen 50 --maxlen 16000 <input-fasta-file> > <trimmed-fasta-file>
```        

Run the `v-annotate.pl` program on the trimmed fasta file with HMPV sequences using the recommended command below. 
   Note the path to the directory name including the /hmpv subdirectory as `<hmpv-models-dir-path>`

```
v-annotate.pl -r --mkey hmpv --mdir <hmpv-models-directory-path> <fasta-file-to-annotate> <output-directory-to-create>
```

After running the `v-annotate.pl` command in step 4, there will be a number of files generated in the `<output-directory-to-create>`. Among these files, there are 5-column tab-delimited feature table files that end with the suffix `.tbl`. 
   There is a separate file for passing (`XXXXX.vadr.pass.tbl`) and failing (`XXXXX.vadr.fail.tbl`) sequences.
   The format of the `.tbl` files is described here:
   https://www.ncbi.nlm.nih.gov/genbank/feature_table/

   More information about understanding failures and error alerts can be found in the VADR
   documentation here: https://github.com/ncbi/vadr/blob/master/documentation/annotate.md

---
## <a name="hmpvmodel"></a>HMPV VADR models
* The VADR model library for HMPV annotation includes 6 HMPV models representing 6 different subgroups: A1, A2a, A2b1, A2b2, B1 and B2.
---

## <a name="docs"> Additional VADR documentation

* [VADR README](https://github.com/ncbi/vadr/blob/master/README.md#top)
* [VADR installation instructions](https://github.com/ncbi/vadr/blob/master/documentation/install.md#top)
  * [Installation using `vadr-install.sh`](https://github.com/ncbi/vadr/blob/master/documentation/install.md#install)
  * [Setting environment variables](https://github.com/ncbi/vadr/blob/master/documentation/install.md#environment)
  * [Verifying successful installation](https://github.com/ncbi/vadr/blob/master/documentation/install.md#tests)
  * [Further information](https://github.com/ncbi/vadr/blob/master/documentation/install.md#further)
* [`v-build.pl` example usage and command-line options](https://github.com/ncbi/vadr/blob/master/documentation/build.md#top)
  * [`v-build.pl` example usage](https://github.com/ncbi/vadr/blob/master/documentation/build.md#exampleusage)
  * [`v-build.pl` command-line options](https://github.com/ncbi/vadr/blob/master/documentation/build.md#options)
  * [Building a VADR model library](https://github.com/ncbi/vadr/blob/master/documentation/build.md#library)
  * [How the VADR 1.0 model library was constructed](https://github.com/ncbi/vadr/blob/master/documentation/build.md#1.0library)
* [`v-annotate.pl` example usage, command-line options and alert information](https://github.com/ncbi/vadr/blob/master/documentation/annotate.md#top)
  * [`v-annotate.pl` example usage](https://github.com/ncbi/vadr/blob/master/documentation/annotate.md#exampleusage)
  * [`v-annotate.pl` command-line options](https://github.com/ncbi/vadr/blob/master/documentation/annotate.md#options)
  * [Basic Information on `v-annotate.pl` alerts](https://github.com/ncbi/vadr/blob/master/documentation/annotate.md#alerts)
  * [Additional information on `v-annotate.pl` alerts](https://github.com/ncbi/vadr/blob/master/documentation/annotate.md#alerts2)
* [Explanations and examples of `v-annotate.pl` detailed alert and error messages](https://github.com/ncbi/vadr/blob/master/documentation/alerts.md#top)
  * [Output fields with detailed alert and error messages](https://github.com/ncbi/vadr/blob/master/documentation/alerts.md#files)
  * [Explanation of sequence and model coordinate fields in `.alt` files](https://github.com/ncbi/vadr/blob/master/documentation/alerts.md#coords)
  * [`toy50` toy model used in examples of alert messages](https://github.com/ncbi/vadr/blob/master/documentation/alerts.md#toy)
  * [Examples of different alert types and corresponding `.alt` output](https://github.com/ncbi/vadr/blob/master/documentation/alerts.md#examples)
  * [Posterior probability annotation in VADR output Stockholm alignments](https://github.com/ncbi/vadr/blob/master/documentation/alerts.md#pp)
* [VADR output file formats](https://github.com/ncbi/vadr/blob/master/documentation/formats.md#top)
  * [VADR output files created by all VADR scripts](https://github.com/ncbi/vadr/blob/master/documentation/formats.md#generic)
  * [`v-build.pl` output files](https://github.com/ncbi/vadr/blob/master/documentation/formats.md#build)
  * [`v-annotate.pl` output files](https://github.com/ncbi/vadr/blob/master/documentation/formats.md#annotate)
  * [VADR `coords` coordinate string format](https://github.com/ncbi/vadr/blob/master/documentation/formats.md#coords)
  * [VADR sequence naming conventions](https://github.com/ncbi/vadr/blob/master/documentation/formats.md#seqnames)


## Reference <a name="reference"></a>
* The recommended citation for using VADR is:
  Alejandro A Schäffer, Eneida L Hatcher, Linda Yankie, Lara Shonkwiler,
  J Rodney Brister, Ilene Karsch-Mizrachi, Eric P Nawrocki; *VADR:
  validation and annotation of virus sequence submissions to
  GenBank.* BMC Bioinformatics 21, 211
  (2020). https://doi.org/10.1186/s12859-020-3537-3

* This page was adapted for HMPV from [Mpox virus annotation](https://github.com/ncbi/vadr/wiki/Mpox-virus-annotation)

---
