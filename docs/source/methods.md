
`back_to_sequences` is written in `rust`. It uses the native HashMap for storing the searched $k$-mer set,
with alternative [aHash](https://github.com/tkaitchuck/aHash) hash function. This data structure is used to index each of the searched $k$-mer set. 

Depending on the user choice, the original or the canonical version of
each $k$-mer from the "reference-$k$-mers" set is indexed. The source code
is unit-tested and functionally tested using tools from the Rust community.

At query time, given a sequence $s$ from $\mathcal{S}$, all its $k$-mers
are extracted and queried using the HashMap. Depending on the user's choice, the canonical
or the original representation of each $k$-mer from the reference set is
indexed. Again, depending on the user's choice, for each queried
$k$-mer, the original version, its reverse complement, or its canonical
representation is queried. If the queried $k$-mer belongs to the index,
its number of occurrences is increased. The number of $k$-mers extracted
from the sequence $s$ that have a match with the index is output
together with the original sequence. As an option, in addition to only
counting the number of occurrences of each $k$-mer from $\mathcal{K}$ in
$\mathcal{S}$, `back_to_sequences` enables to also record their
occurrence positions and orientation and to output this information.
Sequences are queried in parallel.

A minimal and a maximal threshold can be fixed by the user. A queried
sequence is output only if its percentage of $k$-mers that belong to the
searched $k$-mers is strictly higher than the minimal threshold and
lower or equal to the maximal threshold. While the minimal threshold
enables to focus on sequences that are similar enough with a set of
$k$-mers, the maximal threshold offers for instance a way to remove
contaminated sequences.

The sequences to be queried can be provided as a fasta or fastq file
(gzipped or not). They can also be read directly from the standard
input (*stdin*). This offers the may to stream sequences as they arrive,
for instance when they are obtained during an Oxford Nanopore sequencing
process.

The output format of queried sequences respects the input format: if the
input is in fasta (resp. fastq), the output is in fasta (resp. fastq).
