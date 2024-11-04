Usage
=====

.. _installation:

Installation
------------

To use back to sequences, first install it using cargo.

.. code-block:: console

   git clone https://github.com/pierrepeterlongo/back_to_sequences.git
   cd back_to_sequences
   RUSTFLAGS="-C target-cpu=native" cargo install --path .

Rust has to be installed locally: 

.. code-block:: console

   curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

.. note::

   Test the installation: ``cd tiny_test; sh tiny_test.sh; cd -``

back to sequences parameters
----------------------------

Get the list of command parameters and options: ``back_to_sequences --help``.

.. note::
   See :doc:`use cases` for commands lines associated to typical use cases 

.. code-block:: console

    Back to sequences: find the origin of kmers

    Usage: back_to_sequences [OPTIONS] --in-kmers <IN_KMERS>

    Options:
        --in-kmers <IN_KMERS>
            Input fasta file containing the original kmers
                Note: back_to_sequences considers the content as a set of kmers
                This means that a kmer is considered only once, 
                even if it occurs multiple times in the file.
                If the stranded option is not used (default), a kmer 
                and its reverse complement are considered as the same kmer.
        --in-sequences <IN_SEQUENCES>
            Input fasta or fastq [.gz|zst] file containing the original sequences (eg. reads). 
                The stdin is used if not provided 
                (and if `--in_filelist` is not provided neither) [default: ]
        --in-filelist <IN_FILELIST>
            Input txt file containing in each line a path to a fasta or fastq [.gz] file 
            containing the original sequences (eg. reads). 
                Note1: if this option is used, the `--out_filelist` option must be used.
                        The number of lines in out_filelist must be the same as in_filelist
                Note2: Incompatible with `--in_sequences` [default: ]
        --out-sequences <OUT_SEQUENCES>
            Output file containing the filtered original sequences (eg. reads).
            It will be automatically in fasta or fastq format depending on the input file.
            If not provided, only the in_kmers with their count is output [default: ]
        --out-filelist <OUT_FILELIST>
            Output txt file containing in each line a path to a fasta or fastq [.gz] file 
            that will contain the related output file from the input files list  [default: ]
        --out-kmers <OUT_KMERS>
            If provided, output a text file containing the kmers that occur in the reads 
            with their 
            * number of occurrences 
                or 
            * their occurrence positions if the --output_kmer_positions option is used
                Note: if `--in_filelist` is used the output counted kmers are 
                those occurring the last input file of that list [default: ]
        --counted-kmer-threshold <COUNTED_KMER_THRESHOLD>
            If out_kmers is provided, output only reference kmers whose number of occurrences 
            is at least equal to this value.
            If out_kmers is not provided, this option is ignored [default: 0]
        --output-kmer-positions
            If out_kmers is provided, either only count their number of occurrences (default)
            or output their occurrence positions (read_id, position, strand)
        --output-mapping-positions
            If provided, output matching positions on sequences in the
            out_sequence file(s) 
    -k, --kmer-size <KMER_SIZE>
            Size of the kmers to index and search [default: 31]
    -m, --min-threshold <MIN_THRESHOLD>
            Output sequences are those whose ratio of indexed kmers is in ]min_threshold; max_threshold]
            Minimal threshold of the ratio  (%) of kmers that must be found in a sequence to keep it (default 0%).
            Thus by default, if no kmer is found in a sequence, it is not output. [default: 0]
        --max-threshold <MAX_THRESHOLD>
            Output sequences are those whose ratio of indexed kmers is in ]min_threshold; max_threshold]
            Maximal threshold of the ratio (%) of kmers that must be found in a sequence to keep it (default 100%).
            Thus by default, there is no limitation on the maximal number of kmers found in a sequence. [default: 100]
        --stranded
            Used original kmer strand (else canonical kmers are considered)
        --query-reverse
            Query the reverse complement of reads. Useless without the --stranded option
        --no-low-complexity
            Do not index low complexity kmers (ie. with a Shannon entropy < 1.0)
    -t, --threads <THREADS>
            Number of threads
                Note: if not provided, the number of threads is set to the number of logical cores [default: 0]
    -h, --help
            Print help
    -V, --version
            Print version

