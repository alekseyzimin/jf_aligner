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
jf_aligner_cmdline [options]

jf_aligner computes approximate alignments between reference and query DNA sequences 



Options (default value in (), *required):
 -s, --size=uint64                       *Approximatge size of the reference sequence
 -m, --mer=uint32                        *Mer size
 -F, --fine-mer=uint32                    Mer size for fine alignment
     --psa-min=uint32                     Min suffix length in SA. Increase for speed up at the cost of memory (13)
 -t, --threads=uint32                     Number of threads (1)
     --stretch-constant=int               Constant tolerated stretch between matching k-mer in LIS (10)
     --stretch-factor=double              Factor tolerated stretch between matching k-mer in LIS (1.3)
     --stretch-cap=double                 Maximum distance between two consecutive k-mers in LIS (10000.0)
     --window-size=uint32                 Check stretch on every window of k-mer this size (1)
 -f, --forward                            Show all matches forward (reverse super read name if needed) (false)
 -B, --bases-matching=double              Filter base on percent of bases matching (17.0)
 -M, --mers-matching=double               Filter base on percent of k-mer matching (0.0)
     --details=path                       Output files with detail k-mer information
     --coords=path                        Output files with math coordinate information (stdout)
     --max-match                          Output secondary matches (false)
 -H, --no-header                          Do not output header (false)
 -0, --zero-match                         Output header even if query has no match (false)
     --max-count=uint32                   Maximum mer count in super read to be used for alignment (5000)
 -l, --unitigs-lengths=path               Length of k-unitigs
 -u, --unitigs-sequences=path             Fasta file containing the sequence of the k-unitigs
     --compact                            Compact output format (true)
 -k, --k-mer=uint32                       Length of k-mer used to create k-unitigs
 -r, --superreads=path                    reference sequence file
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
