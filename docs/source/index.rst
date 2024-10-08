Welcome to "back to sequences" documentation!
===================================

`back to sequences <https://github.com/pierrepeterlongo/back_to_sequences>`_ is a rust program for bioinformaticians. 
It's purpose is to find where a set of :math:`k`-mers occurs in a set of input genomic sequences.

More precisely: 

Given a set :math:`K` of :math:`k`-mers (fasta / fastq [.gz] format) and a set of sequences  (fasta / fastq [.gz] format), this tool will extract the sequences containing some of those kmers.

A minimal (:math:`m`) and a maximal (:math:`M`) thresholds are proposed. A sequence whose percentage of kmers shared with :math:`K` are in :math:`]m, M]` is output with its original header + the number of shared kmers + the ratio of shared kmers:
```
>original_header 20 6.13
TGGATAAAAAGGCTGACGAAAGGTCTAGCTAAAATTGTCAGGTGCTCTCAGATAAAGCAGTA...
```
In this case 20 kmers are shared with the indexed kmers. This represents 6.13% of the kmers in the sequence.


Contents
--------

.. toctree::
   :maxdepth: 2

   usage
   use cases
   results
   methods
   citation
   
