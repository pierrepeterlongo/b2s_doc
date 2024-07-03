Welcome to back to sequences's documentation!
===================================

**back to sequences** is a rust program for bioinformaticians. 
It's purpose is to find where a set of :math:`k`-mers occurs in a set of input genomic sequences.

More precisely: 

Given a set :math:`K` of :math:`k`-mers (fasta / fastq [.gz] format) and a set of sequences  (fasta / fastq [.gz] format), this tool will extract the sequences containing some of those kmers.

A minimal (:math:`m`) and a maximal (:math:`M`) thresholds are proposed. A sequence whose percentage of kmers shared with :math:`K` are in :math:`]m, M]` is output with its original header + the number of shared kmers + the ratio of shared kmers:
```
>original_header 20 6.13
TGGATAAAAAGGCTGACGAAAGGTCTAGCTAAAATTGTCAGGTGCTCTCAGATAAAGCAGTA...
```
In this case 20 kmers are shared with the indexed kmers. This represents 6.13% of the kmers in the sequence.



Check out the :doc:`usage` section for further information, including
how to :ref:`installation` the project.

Contents
--------

.. toctree::
   :maxdepth: 1

   usage
   use cases
   benchmark
   citation
   
