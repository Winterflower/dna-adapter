#DNAdapter

DNAdapter is a Python library for processing FASTA formatted DNA sequences
into formats suitable for Machine Learning libraries such as scikit-learn and 
Shogun.


##Acknowledgements
This project incorporates materials from the following academic and open-source projects
* kmersvm by Dongwon Lee (2011)


##A simple tutorial
The purpose of this tutorial is to help you get you FASTA formatted DNA sequence
file into a format suitable for use with machine learning libraries. 

###The sample file
We have the following FASTA files. 

'positive_sequences.fasta'

    >chr1:80-90
    AATTCGCGCG

    >chr2:95-105
    GGCCTGTGTG

    >chr3:89-99
    AAATAATATA

'negative_sequences.fasta'




representing some regions in the DNA sequence.
In order to apply ML algorithms, we have to represent these sequence in terms of attribute-value 
pairs. One way to represent a DNA sequence is to break it down into k-mers and count
the occurence of each k-mer in the sequence. This has some disadvantages, but is a
good enough feature representation for our current purposes.
