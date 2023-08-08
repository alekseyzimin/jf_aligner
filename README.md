# JF_aligner -- ultra fast tool to compute approximate alignments between genomic DNA sequences

JF_aligner uses novel ultrafast data staructure called Partial Suffix Array (PSA) to compute approximate alignments (start/end) between DNA sequences.  Only the start and end coordinares of the alignment are computed.  The alignment is computed bu finding a longest common sequence of k-mers that reference and query sequences have in common.  

# Installation insructions

To install, first download the latest distribution from the github release page https://github.com/alekseyzimin/jf_aligner/releases.Then untar/unzip the package jf_aligner-X.X.X.tgz, cd to the resulting folder and run `./install.sh`.  The installation script will configure and make all necessary packages.  The jf_aligner executables will appear under bin/

Only for developers:  you can clone the development tree, but then there are dependencies such as swig and yaggo (http://www.swig.org/ and https://github.com/gmarcais/yaggo) that must be available on the PATH:

```
git clone https://github.com/alekseyzimin/jf_aligner
git submodule init
git submodule update
make
```
Note that on some systems you may encounter a build error due to lack of xlocale.h file, because it was removed in glibc 2.26.  xlocale.h is used in Perl extension modules used by jf-aligner.  To fix/work around this error, you can upgrade the Perl extensions, or create a symlink for xlocale.h to /etc/local.h or /usr/include/locale.h, e.g.:
```
ln -s /usr/include/locale.h /usr/include/xlocale.h
```

# Usage:
```
Usage: jf_aligner [options]

jf_aligner computes approximate alignments between reference and query DNA sequences


Options (default value in (), *required):
 -s, --size=uint64                       *Approximate size of the reference sequence
 -m, --mer=uint32                         Alignment K-mer size -- the best practice is to use k-mer size such that the number of distinct k-mer s of that size was about greater or equal to the size of the reference.  4^15 ~ 1 Billion, so 15-mers are good for reference up to 1 Billion in size, 16-mers are good for references up to 4 Billion, etc.  (15)
 -F, --fine-mer=uint32                    Mer size for fine alignment (experimental)
     --psa-min=uint32                     Min suffix length in SA. Increase for speed up at the cost of memoryi, minimum 12 (13)
 -t, --threads=uint32                     Number of threads (1)
     --stretch-constant=int               Constant tolerated stretch between matching k-mer in LIS (10)
     --stretch-factor=double              Factor tolerated stretch between matching k-mer in LIS (1.3)
     --stretch-cap=double                 Maximum distance between two consecutive k-mers in LIS (10000.0)
     --window-size=uint32                 Check stretch on every window of k-mer this size (1)
     --forward                            Show all matches in forward direction (false)
 -B, --bases-matching=double              Minimum percent of bases matching exactly  (17.0)
 -M, --mers-matching=double               Minimum percent of k-mers matching (0.0)
 -N, --num-matches=uint32                 Maximum number of best matches output 0:unlimited, for PAF output format only (0)
     --details=path                       Output files with detail k-mer information
 -o, --coords=path                        Output file with approximate alignment coordinates information (stdout)
     --max-match                          Output more than a single match between any two sequences (false)
 -H, --no-header                          Do not output header (for standard or compact output format only) (false)
 -0, --zero-match                         Output header even if query has no match (for standard or compact output format only) (false)
     --max-count=uint32                   Maximum count of K-mer in reference sequence to be used for alignment. K-mers with count above this value are not used for alignment  (5000)
 -l, --unitigs-lengths=path               Length of k-unitigs (uncommon use case)
 -u, --unitigs-sequences=path             Fasta file containing the sequence of the k-unitigs (uncommon use case)
 -f, --format=uint32                      Output format 0:standard 1:compact 2:PAF (1)
 -k, --k-mer=uint32                       Length of k-mer used to create k-unitigs (uncommon use case)
 -r, --reference=path                     reference sequence file
 -q, --pacbio=path                        query sequence file
 -U, --usage                              Usage
 -h, --help                               This message
 -V, --version                            Version

 ```

# Development 
To clone and set up the development tree, please issue the following commands:
```
$ git clone https://github.com/alekseyzimin/jf_aligner
$ cd jf_aligner
$ git submodule init
$ git submodule update
$ cd jellyfish/ && git checkout develop
$ cd ../PacBio && git checkout jf_aligner
$ cd ../ufasta && git checkout master
$ cd ..
```
after that you can run 'make' to compile the package.  The binaries will appear under build/inst/bin.  To create a distribution, run 'make install'.
