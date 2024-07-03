Some basical use cases of back to sequences
===========================================


Basic case
----------
.. code-block:: console

  back_to_sequences --in-kmers compacted_kmers.fasta --in-sequences reads.fasta --out-sequences filtered_reads.fasta  --out-kmers counted_kmers.txt

The `filtered_reads.fasta` file contains the original sequences (here reads) from `reads.fasta` that contain at least one of the kmers in `compacted_kmers.fasta`.
The headers of each read is the same as in `reads.fasta`, plus the estimated ratio of shared kmers and number of shared kmers.

If the `--out-kmers` option is used, the file `counted_kmers.txt` contains for each kmer in `compacted_kmers.fasta` the number of times it was found in `filtered_reads.fasta`.

Using filters
-------------
.. code-block:: console

  back_to_sequences --in-kmers compacted_kmers.fasta --in-sequences reads.fasta --out-sequences filtered_reads.fasta  --out-kmers counted_kmers.txt --min-threshold 50 --max-threshold 70


In this case only sequeces from `reads.fasta` that have more than 50% and at most 70% of their kmers in `compacted_kmers.fasta` are output.

Specifying strands
------------------
.. code-block:: console

  back_to_sequences --in-kmers compacted_kmers.fasta --in-sequences reads.fasta --out-sequences filtered_reads.fasta --stranded


In this case, the kmers found in `compacted_kmers.fasta` are indexed in their original orientation, and kmers extracted from `reads.fasta` are queried in their original orientation. 

.. Note:: 
  Without the `--stranded` option, all kmers (indexed and queried) are considered in their canonical form.


One may be interested in finding kmers from the reverse complement of the queried sequences. In this case we add the `--query-reverse` option together with the `--stranded` option:

.. code-block:: console

  back_to_sequences --in-kmers compacted_kmers.fasta --in-sequences reads.fasta --out-sequences filtered_reads.fasta --stranded


Reading sequences from standard input
-------------------------------------
.. code-block:: console

  cat reads.fasta | back_to_sequences --in-kmers compacted_kmers.fasta --out-sequences filtered_reads.fasta 

Do not provide the `--in-sequences` if your input data are read from stdin.

Using several input read sets
------------------

Say you have three input read files `1.fa`, `2.fa`, `3.fa` to which you wish to apply `back_to_sequences`. 

1. create an input and an output files:

.. code-block:: console
  ls 1.fa 2.fa 3.fa > in.fof
  echo "1_out.fa\n2_out.fa\n3_out.fa" > out.fof

2. run `back_to_sequences`

.. code-block:: console
  back_to_sequences --in-filelist in.fof --in-kmers compacted_kmers.fasta --out-filelist out.fof 


Output matching kmers
----------------------
Output the list of matching kmers with their number of occurrences
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

  `back_to_sequences` enables to output for each kmers in `in-kmers` set, its number of occurrences in the queried sequences. 

.. code-block:: console
  back_to_sequences --in-sequences sequence.fa --in-kmers kmer.fa --out-sequences /dev/null  --out-kmers out_kmers.txt

In this case the `out_kmers.txt` file contains, for each kmer from `kmer.fa` its number of occurrences in the `sequence.fa` file (canonical or not, depending on the usage of  the `--stranded` option). 

Output the list of matching kmers with their position in sequences
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

`back_to_sequences` enables to output for each kmers in `in-kmers` set, its positions in the queried sequences. 

.. code-block:: console
  back_to_sequences --in-sequences sequence.fa --in-kmers kmer.fa --out-sequences /dev/null  --out-kmers out_kmers.txt --output-kmer-positions

In this case the `out_kmers.txt` file contains, for each kmer from `kmer.fa` its occurrences in the `sequence.fa` file. An occurrence is given by a triplet `(sequence_id, position, strand)`.  
- `sequence_id`: id (starting from 0) of the sequence from `sequence.fa` where the kmer occurs.
- `position`: position (starting from 0) where the kmer occurs on the sequence
- `strand`: orientation of the canonical version of the queried kmer in the sequence. This is a bit misleading: 
    - without the ‘stranded’ option, the position of the canonical version of the kmer is given. This version can be found in the same direction (true) or in the reverse complement direction (false).
    - with the ‘stranded’ option, the position of the requested version of the kmer is given. Only the forward version of this kmer is found. 

Output for each queried sequence its location and strand of shared kmers
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
`back_to_sequences` enables to output for each queried sequence, the location and strand of its kmers shared with the `in-kmers` set.

.. code-block:: console
  back_to_sequences --in-sequences sequence.fa --in-kmers kmer.fa --out-sequences out_sequences.fa

In this case the `out_sequences.fa` contains for each queried sequence its usual header (original header number and ratio of shared kmers with the `in-kmers` set) and additionaly, it shows the location (0-based) of shared kmers. For each location (including 0), the strand is indicated by nothing or a `-` character if the `--stranded` option is given. 


