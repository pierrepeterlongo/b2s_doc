Results
=======

Results were obtained with back to sequences, version ``v0.6.5``.

Benchmark on synthetic data
---------------------------

Results
~~~~~~~

This benchmark is reproducible by running ``generate_data.sh`` and then ``bench.sh`` in the ``benchs`` folder. 
Running time is given by the ``\usr\bin\time`` command, considering the ``real`` field.

Presented results were obtained on 
* the GenOuest platform on a node with 32 threads Xeon 2.2 GHz, denoted by "genouest" in the table below.
* a MacBook, Apple M2 pro, 16 GB RAM, with 10 threads, denoted by "mac" in the table below.
* AMD Ryzen 7 4.2 GHz 5800X 64 GB RAM,  with 16 threads, denoted by "AMD" in the table below.

We indexed: one million kmers (exactly 995,318) of length 31.
We queried: from 10,000 reads to 200 million reads (+ 1 billion on the cluster), each of length 100.

===============  =============  ========  ========  =======
Number of reads  Time genouest  Time mac  Time AMD  max RAM
===============  =============  ========  ========  =======
10,000           0.7s           0.54s     0.4s      0.13 GB
100,000          0.8s           0.8s      1.2s      0.13 GB
1,000,000        2.0s           3.5s      7.1s      0.13 GB
10,000,000       7.1s           11s       16s       0.13 GB
100,000,000      47s            58s       48s       0.13 GB
200,000,000      1m32s          1m52s     1m44      0.13 GB
===============  =============  ========  ========  =======

Reproduce the benchmark
~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: console

  cd benchs
  ./generate_data.sh
  ./bench.sh

In case you are running out of space, you can limit the bench to a smaller number of reads by running:

.. code-block:: console

  cd benchs
  ./generate_data.sh 100000
  ./bench.sh 100000

This will generate and bench only 10,000 and 100,000 reads.

After running the benchmark, you may remove the generated data by running:

.. code-block:: console

  cd benchs
  ./clean.sh

Benchmark data generation details
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We generated a random sequence  :math:`s` of size 100 million base pairs on the alphabet (:math:`A,C,G,T`). 
From this sequence  :math:`s`, we randomly extracted 50,000 sub-sequences each of size 50. We consider these sequences as
containing the set :math:`K` of :math:`k`-mers to be searched. As we used
:math:`k=31`, each subsequence contains :math:`50-31+1 = 20` :math:`k`-mers. Doing so, we
consider a set of at most :math:`50000\times 20 = 1,000,000` :math:`k`-mers.

We also generated six sets composed respectively of 10 thousand, 100 thousand, one million, 10
million, 100 million, and 200 million sequences, each of length 100
nucleotides. Each sequence is randomly sampled from :math:`s`.

Generate data for testing
~~~~~~~~~~~~~~~~~~~~~~~~~

You may be interested by generating a specific data set. For instance

.. code-block:: console

  # Generate 1 reference sequence of random length 50000 and minimum length 100
  python scripts/generate_random_fasta.py 1 50000 100 ref_seq.fasta

  # Extract 1000 random "reads", each of length in [100;500] from the reference sequence
  python3 scripts/extract_random_sequences.py --input ref_seq.fasta --min_size 100 --max_size 500 --num 1000 --output reads.fasta 

  # From those reads, extract 500 random sequence containing the kmers. Those kmers are stored in sequences of length in [31;70]
  python3 scripts/extract_random_sequences.py --input reads.fasta --min_size 31 --max_size 70 --num 500 --output compacted_kmers.fasta

Benchmark on *Tara* ocean seawater metagenomic data
---------------------------------------------------

The previously proposed benchmark shows the scalability of the proposed
approach. Although performed on random sequences, there is no objective
reason why performance should differ on real data, regardless of the
number of $k$-mers actually detected in the data. We verify this claim
by applying `back_to_sequences` to real complex metagenomic sequencing
data from the [Tara](https://www.nature.com/articles/s41579-020-0364-5) ocean project.

We downloaded one of the *Tara* ocean read sets: station number 11
corresponding to a surface Mediterranean sample, downloaded from the
European Nucleotide Archive, identifier [ERS488262](https://www.ebi.ac.uk/ena/browser/view/ERS488262). We extracted the
first 100 million reads, which are all of length 100. Using
`back_to_sequences` we searched in these reads each of the 69 31-mers
contained in its first read. On the GenOuest node, `back_to_sequences`
enabled to retrieve all reads that contain at least one of the indexed
:math:`k`-mers in 5m17 with negligible RAM usage of 45MB. As expected, these
scaling results are in line with the results presented
in the previous Table.

Out of curiosity, we ran `back_to_sequences` on the full read set,
composed of ~26.3 billion :math:`k`-mers, and 381 million reads,
again for searching the 69 :math:`k`-mers contained in its first read. This
operation took 20m11.
