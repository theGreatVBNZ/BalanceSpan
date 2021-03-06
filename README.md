# BalanceSpan ReadMe [![Build Status](https://travis-ci.org/guillaume-bosc/BalanceSpan.svg?branch=master)](https://travis-ci.org/guillaume-bosc/BalanceSpan)

## Introduction
*BalanceSpan* is an algorithm designed to extract frequent patterns (strategies) from StarCraft II replays. Since it is generic, *BalanceSpan* could be applied to many other datasets. It computes a measure that evaluates the balance of a strategy, *i.e.*, if the strategy is likely to win or to lose. BalanceSpan allows the user to identify some imbalanced strategies.
*BalanceSpan* is based on the *PrefixSpan* framework that only extracts frequent patterns from a set of sequences.

For further information about the aims of *BalanceSpan*, please go to http://guillaume-bosc.github.io/BalanceSpan

## Repository contents
To run *BalanceSpan*, you need to download the projet from this GitHub repository. Below, we detail the content of each folder contained in this repository.
- The "algo" folder contains the source code of the BalanceSpan algorithm:
  - The folder "BalanceSpan_Mac" for Mac users
  - The folder "BalanceSpan_Linux" for Linux users
- The "data" and "data-nointer" folders contain the datasets used in our experiments. The files "sequences-XX-WW.txt.bin" are the binary version of the files "sequences-XX-WW.txt", where "XX" is related to the type of match-up (e.g. XX = PP for games opposing Protoss to Protoss), and "WW" is duration of the window for the sequence we used (if WW = 30, it means that buildings created during the first 30 secondes of the game are included in the first itemset of the sequence, buildings created between the 31th and the 1st minute are included in the second itemset of the sequence, and so on). The files "dico-XX-WW.txt" correspond to the dictionnary required in the experiments.
- The "script" folder contains an example of the command to launch a run.
- The "results" folder contains the result of the above script.
- The "binarize" folder contains the source to binarize the input data required by *BalanceSpan*.

## BalanceSpan Workflow
This part details the process to run *BalanceSpan*.

1. **Compile *BalanceSpan*.**
Depending on the OS you are on, we provide you a version dedicated to Linux users, or to Mac users. You can find these versions in the "algo" folder. Each of these versions include a Makefile to compile the sources. To compile it, just execute the following command line:
`make` within the folder of the desired algorithm: BalanceSpan_Linux or BalanceSpanMac. It will create the executable BalanceSpan in the folder "algo".

2. **Binarize data.** If you do not use the data we provide, and you want to experiment on your own data, you should binarize you data. Indeed, *BalanceSpan* requires binarized data to run. We provide a simple algorithm to easily binarize data. With a terminal go into the binarize folder, compile with the command line `make` and run it with `./Binarize <filename>`. It will produce a new binarized file named filename.bin.
Note that your data file has to be encoded in a certain way:
  - each sequence is written on a single line
  - each itemset of the sequence ends by -1
  - each sequence ends by -2
  - each original item is represented as a unique number between 0 and the number of different items minus 1
  - each original item labelled as + is represented by a even number (*e.g.*, `i`) and its dual item (labelled as -) is represented by the odd number `i+1`. For example, the sequence `<(SupplyDepot-)(Barracks+)(Barracks-, CommandCenter+)>` is represented by the line `3 -1 0 -1 1 4 -1 -2` where `SupplyDepot-` is represented by `3` (`SupplyDepot+` is `2`), `Barracks+` is `0 `, (so `Barracks-` is `1`) and `CommandCenter+` is `4`.

3. **Run *BalanceSpan*.** Once you've compiled *BalanceSpan* and your data is binarized, you are ready to run it on this data. To do that, you can have a look at the script we provide as an example in the folder "script". *BalanceSpan* requires several parameters to be launched. The prototype of the command line is as follows: `./BalanceSpan <filename> <support> <itemcount> <dictionnary_file>`. Here, we explain these parameters:
  - `<filename>` is the path to the data binarized file.
  - `<support>` is the value of the threshold for the support. It is a double included in 0 and 1.
  - `<itemcount>` is the number of different items in your data.
  - `<dictionnary_file>` is the path to the dictionnary file, for the mapping.


4. **Get the results.** Once the execution is over, two files are generated: `result.txt` and `error.tmp`. `result.txt` contains the result of the run of *BalanceSpan*, and `error.tmp` contains a log file if you encountered some errors.

## References
- Guillaume Bosc, Mehdi Kaytoue, Chedy Raïssi, Jean-Francois Boulicaut and Philip Tan. Mining Balanced Patterns in Real-Time Strategy Games. In Torsten Schaub, editor, 21st European Conference on Artificial Intelligence, pages 975–976, 2014. (Short paper). Online version: https://hal.archives-ouvertes.fr/hal-01100933v1
- Guillaume Bosc, Philip Tan, Jean-François Boulicaut, Chedy Raïssi and Mehdi Kaytoue. A Pattern Mining Approach to Study Strategy Balance in RTS Games. In IEEE Transactions on Computational Intelligence and AI in Games, 2015. Online version: https://hal.archives-ouvertes.fr/hal-01252728v1

For any questions/remarks, contact Guillaume BOSC: guillaume.bosc@insa-lyon.fr

[![DOI](https://zenodo.org/badge/18678/guillaume-bosc/BalanceSpan.svg)](https://zenodo.org/badge/latestdoi/18678/guillaume-bosc/BalanceSpan)
