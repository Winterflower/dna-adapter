#DNAdapter

DNAdapter is a Python library for processing FASTA formatted DNA sequences
into formats suitable for Machine Learning libraries such as scikit-learn and
Shogun.

##License

"""
	kmersvm_classify.py; classify sequences using SVM
	Copyright (C) 2015 Noora Camilla Montonen
        Incorporating pieces of "kmersvm" by Dongwon Lee (2011)

	This program is free software: you can redistribute it and/or modify
	it under the terms of the GNU General Public License as published by
	the Free Software Foundation, either version 3 of the License, or
	(at your option) any later version.

	This program is distributed in the hope that it will be useful,
	but WITHOUT ANY WARRANTY; without even the implied warranty of
	MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
	GNU General Public License for more details.

	You should have received a copy of the GNU General Public License
	along with this program.  If not, see <http://www.gnu.org/licenses/>.
"""

##Acknowledgements
This project incorporates materials from the following academic and open-source projects
* kmersvm by Dongwon Lee (2011)

##Installation
At the moment, the data adapter consists of a single Python module `preprocess`.
To use it, simply download the preprocess.py file by cloning the repo
and make sure it is in Python import's search path. 
One of the simplest ways to make sure the import statement can find preprocess
is to add the path to preprocess.py to your PYTHONPATH environment variable.

 


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
    >chr2:89-90
    ATGTGGGTTTG

    >chr3:80-88
    ATGTAAGG

    >chr4:50-58
    GGCCAATG

representing some regions in the DNA sequence.
In order to apply ML algorithms, we have to represent these sequence in terms of attribute-value
pairs. One way to represent a DNA sequence is to break it down into k-mers and count
the occurence of each k-mer in the sequence. This has some disadvantages, which we will discuss
in the future, but at the moment, but is a
good enough feature representation for our current purposes. First, however, let's read the negative and
positive files into variables.

```python

    #import the preprocess module
    import preprocess

    #the 'read_fastafile' method in the preprocess module returns the sequences and their ids
    positive_sequences, positive_sequence_ids = preprocess.read_fastafile('positive_sequences.fasta')
    negative_sequences, negative_sequence_ids = preprocess.read_fastafile('negative_sequences.fasta')
```

Now we have four arrays: positive sequences and ids and negative sequences and ids.


##Make a feature representation out of each sequence
In order to use the scikit-learn library to analyse out DNA sequences, we must
convert each DNA sequence into a feature vector, which will contain
the counts of the occurences of each kmer. In reality, you may have
a fasta file with thousands of DNA sequences and you will not be able to perform this by hand.

The Matrix class in the preprocess module takes as input the positive and negative sequence and id
arrays we generated in the previous code snippet. Additionally, the matrix class also
takes as input a boolean value (whether or not to normalise the matrix) and the length of the
k-mer.

Let's create a kmer matrix with kmers of length 2 from the two files we imported in
the previous step.

```python
   dna_matrix=Matrix(True, positive_sequences, positive_ids, negative_sequences, negative_ids, 6)
```

This will create a matrix, ready to be used with Scikit learn applications.
