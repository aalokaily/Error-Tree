# ET
The scripts are implementation of error tree algorithms for resolving the approximate matching problem. The full description is available at the preprint https://arxiv.org/pdf/2110.13802.pdf. 
Generally speaking, Pattern matching is a fundamental process in most scientific domains. The problem involves finding the starting positions of a given pattern (usually of short length) in a reference stream of data (of considerable length) as either exact or approximate (which allows for mismatches, insertions, or deletions) matching. For exact matching, several data structures built in linear time and space can be used in practice nowadays. The solutions proposed so far for approximate matching are non-linear, impractical, or heuristics. With the use of suffix trees and a tree structure derived from suffix trees, we designed and implemented a linear structure ($O(n)$) that resolves the approximate matching problem


-------------------------------------------------------------------------------- Prerequisite ---------------------------------------------------------------------------
Install the following library that will be used to build suffix trees:
https://github.com/ptrus/suffix-trees 

Then edit ./suffix_trees/STree.py as the followings:

- comment the following line, line 249, by appending "#" at the beginning of the line. This will allow setting attributes to the suffix tree more freely.
__slots__ = ['_suffix_link', 'transition_links', 'idx', 'depth', 'parent', 'generalized_idxs']

- In some procedures, we hashed suffix tree internal nodes, so we used only the combination of node index and node depth to identify uniquely an internal node instead of the original identification of the nodes given by the library which is a bit long (shorter identification will use less space and speed up the lookup process for the hashed nodes. So replace the following two lines (which are line 264 and 265) 

return ("SNode: idx:" + str(self.idx) + " depth:" + str(self.depth) +
                " transitons:" + str(list(self.transition_links.keys())))

with the line:

return (str(self.idx) + "-" + str(self.depth))


-------------------------------------------------------------------------------- Preparation --------------------------------------------------------------------------------------------

Firstly, you need to convert the genome in fasta format to a one-line genome which remove any non A, C, G, T, and N (case is sensitive) and headers. This can be done using the script filter_DNA_file_to_4_bases_and_N.py by running the command:

python filter_DNA_file_to_4_bases_and_N.py $file.fasta 

-------------------------------------------------------------------------------- Running the tool-----------------------------------------------------------------------
The tool was tested on DNA sequences (fasta format). As a preparation process for the input data, remove headers and newlines from the fasta file so that all DNA sequence is stored in one line. The input for the tools is the converted fasta file. These tools are applicable for Hamming distance and Wildcards matching. 

Running command:
python3 {OT_index_script}.py converted_fasta_file k pattern 

OT_index_script can be ET_using_base_paths.py or ET_using_base_suffixes.py
k value must be an integer.

As an example:
python3 ET_using_base_paths.py.py some.fasta 1 AAAAAAAAAAAAA

A sample output:
Reading input data took 0.00014 seconds
------------------------------------------------------------------------------------------
Building Suffix Tree took 0.41073 seconds
------------------------------------------------------------------------------------------
Number of internal nodes is 31628
Number of leaves is 47962
Processing leaf and internal nodes took 0.6174 seconds
------------------------------------------------------------------------------------------
Number of OSHR leaf_nodes is 12029
Number of OSHR internal nodes is 19599
In total of 31628

***** Phase 1 for building OT index for base paths finished in 0.3652 seconds
Length of OT index  109853
Left and right OT index of root 0 109853

***** Phase 2 for building OT index for base paths finished in 0.42125 seconds
sum_of_all_OT_indexes 109853

***** Phase 3 for building OT index for base paths finished in 0.44562 seconds
Building OT index using base paths took 1.23214 seconds
------------------------------------------------------------------------
Approximate matchings:
------------------------------------------------------------------------
GAAAAAAAAAAAA 1 [0] 158 leaf node
AGAAAAAAAAAAA 1 [1] 157 leaf node
AAGAAAAAAAAAA 1 [2] 156 leaf node
AAAGAAAAAAAAA 1 [3] 155 leaf node
AAAAATAAAAAAA 1 [5] 14979 leaf node
AAAAAATAAAAAA 1 [6] 14978
AAAAAATAAAAAA 1 [6] 47350
AAAAAATAAAAAA 1 [6] 11694
AAAAAAGAAAAAA 1 [6] 38905 leaf node
AAAAAAATAAAAA 1 [7] 14977 leaf node
AAAAAAAGAAAAA 1 [7] 38904 leaf node
AAAAAAAAACAAA 1 [9] 532 leaf node
AAAAAAAAAACAA 1 [10] 531 leaf node
AAAAAAAAAAACA 1 [11] 530 leaf node
AAAAAAAAAAAAT 1 [12] 191 leaf node

Number of matchings is 15
Number of position_combinations in the results 11
Number of expected combinations 13
All matching results is with a distance equal to 1 as should be
Found approximate matching for k = 1 in  0.00141 seconds



For contact, please email AA.12682@KHCC.JO (the email of the first author Anas Al-okaily).