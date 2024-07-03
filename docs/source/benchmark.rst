Back to sequences' Quick benchmark
==================================
Results were obtained with back to sequences, version ``v0.6.5``.

Results
-------
This benchmark is reproducible by running ``generate_data.sh`` and then ``bench.sh`` in the ``benchs`` folder. 

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


Side note: generate data for testing
------------------------------------

You may be interested by generating a specific data set.
.. code-block:: console

  # Generate 1 reference sequence of random length 50000 and minimum length 100
  python scripts/generate_random_fasta.py 1 50000 100 ref_seq.fasta

  # Extract 1000 random "reads", each of length in [100;500] from the reference sequence
  python3 scripts/extract_random_sequences.py --input ref_seq.fasta --min_size 100 --max_size 500 --num 1000 --output reads.fasta 

  # From those reads, extract 500 random sequence containing the kmers. Those kmers are stored in sequences of length in [31;70]
  python3 scripts/extract_random_sequences.py --input reads.fasta --min_size 31 --max_size 70 --num 500 --output compacted_kmers.fasta
