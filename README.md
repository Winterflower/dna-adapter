#DNAdapter

DNAdapter is a Python library for processing FASTA formatted DNA sequences
into formats suitable for Machine Learning libraries such as scikit-learn and
Shogun.

##License
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
Let's print them out just to verify that the code is doing what we thing
it is doing.

```python
>>> positive_sequences
['AATTCGCGCG', 'GGCCTGTGTG', 'AAATAATATA']
>>> positive_sequence_ids
['chr1:80-90', 'chr2:95-105', 'chr3:89-99']

>>>negative_sequences
['ATGTGGGTTTG', 'ATGTAAGG', 'GGCCAATG']

>>>negative_sequence_ids
['chr2:89-90', 'chr3:80-88', 'chr4:50-58']
```
Looks good!
Now let's convert each of these sequence into a feature vector!


##Make a feature representation out of each sequence
In order to use the scikit-learn library to analyse out DNA sequences, we must
convert each DNA sequence into a feature vector. There are many potential
ways in which you can represent a DNA sequences as a feature vector.
At the moment, the preprocess module only supports a kmer-representation.

###What is a kmer-reprsentation of a DNA sequence?
A kmer-representation simply counts the number of times each of the possible
kmers (strings of DNA nucleotides of length k) occurs in the given DNA sequence.
Let's go through a toy example.

All of the possible 2-mers in DNA are listed in the list below.

```python
['AA', 'AC', 'AG', 'AT', 'CA', 'CC', 'CG', 'CT', 'GA', 'GC', 'GG', 'GT', 'TA', 'TC', 'TG', 'TT']
```

To represent the DNA sequence ATGTGGGTTTG as a 2-mer feature vector, we
simply count the number of occurrences of each of the 2-mers in the list above
(this is a bit oversimplified given that we have to take into account
palindromes and reverse_complements, but that's a topic for a later tutorial :) ).

This gets tedious really quickly. You probably would not want to do this
for a DNA sequence of over a 1000 nucleotides!
The preprocess module contains a Matrix class, which takes four arguments:

* a boolean value which specifies whether the values in the matrix are normalised
* a list of positive sequences
* a list of positive sequence ids
* a list of negative sequences
* a list of positive ids
* the value of k (ie the kmer length)

Let's create a kmer matrix with kmers of length 2 from the two files we imported in
the previous step.

```python
dna_matrix=preprocess.Matrix(True, positive_sequences, positive_sequence_ids,
negative_sequences, negative_sequence_ids, 2)
```

Let's also check that our code didn't go completely bonkers while
processing the files.

```python
>>> dna_matrix
```

This will create a matrix, ready to be used with Scikit learn applications.
