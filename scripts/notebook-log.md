Savannah elephants, Loxodonta africana, were debated to be a different species than forest elephants, Loxodonta cyclotis. This paper sequenced mitochondrial DNA from each species and found diverging molecular clades. 
Two sets of samples were taken: hair/blood from 26 wild-born elephants and bone/tooth from another 23 specimens, representing both central and western Africa. 
About 3700 bp fragments along the cytochrome b gene, the control region, 12S rRNA gene, and three tRNAs were studied.


I will choose to use MUSCLE because it is overall the best sequencer between itself, ClustalW, and TeaCoffee. A limitation it has is that it doesn't do well with high diversity data, which I don't have. My data should be pretty similar. Another shining quality is that it has improved progressive alignment and has a large size capability. Because of these factors, I will choose MUSCLE.



(base) gannonkreuser@Gannons-MBP GannonPLPATH % muscle -align gannon_elephant.fasta -output gannon_elephant_aligned.fasta

muscle 5.3.osxarm64 [-]  17.2Gb RAM, 8 cores
Built Dec 12 2024 04:49:44
(C) Copyright 2004-2021 Robert C. Edgar.
https://drive5.com

[align gannon_elephant.fasta]
Input: 50 seqs, avg length 1889, max 1990, min 1141

00:00 4.3Mb   100.0% Derep 48 uniques, 2 dupes
00:00 4.4Mb  CPU has 8 cores, running 8 threads
02:13 250Mb   100.0% Calc posteriors
02:14 137Mb   100.0% UPGMA5         
02:15 323Mb   100.0% Consistency (1/2)
02:17 354Mb   100.0% Consistency (2/2)
02:19 359Mb   100.0% Refining         







Maximum likelihood:

RAxML-ng 

Software:
RAxML is a widely used software for phylogenetic tree inference using the Maximum Likelihood method. It is designed to efficiently analyze large datasets and supports various models of nucleotide and protein evolution.

Description:
RAxML implements the ML method for inferring phylogenies from sequence data. It supports partitioned models, bootstrapping, and different types of substitution models. The software is optimized for high-performance computing and parallel processing.

Strengths:

Fast and scalable for large datasets.
Optimized for multi-threading and distributed computing.
Supports various substitution models and partitioning strategies.
Well-documented and widely used in evolutionary biology.
Offers bootstrapping for tree support assessment.

Weaknesses:

Can be computationally expensive for very large datasets.
Requires careful model selection to avoid bias.
Limited support for mixed-model analyses compared to Bayesian approaches.
May not handle highly recombining sequences well.

Assumptions:

The mutation process is the same at every branch of the tree
We assume sites evolve independently
All sites evolve the same 
The phylogenetic tree follows a bifurcating structure.




Checking the version:
% ./raxml-ng -v

Checking for MSA: 
% ./raxml-ng --check --msa gannon_elephant_aligned4_single_lineNAMED.fasta --model GTR+G

Using the --parse command: 
% ./raxml-ng --parse --msa gannon_elephant_aligned4_single_lineNAMED.fasta --model GTR+G

Infer: 
./raxml-ng --msa gannon_elephant_aligned4_single_lineNAMED.fasta --model GTR+G --prefix T1 --threads 2 --seed 5

Final LogLikelihood: -4527.454623

AIC score: 9266.909247 / AICc score: 9279.144414 / BIC score: 9858.517488
Free parameters (model + branch lengths): 106

WARNING: Best ML tree contains 11 near-zero branches!


Infer using jukes cantor model: 
% ./raxml-ng --msa gannon_elephant_aligned4_single_lineNAMED.fasta --model JC --prefix jc_run --threads 2 --seed 5

Final LogLikelihood: -4998.827247

AIC score: 10191.654495 / AICc score: 10201.859540 / BIC score: 10733.031848
Free parameters (model + branch lengths): 97

WARNING: Best ML tree contains 11 near-zero branches!


Infer using HKY-G model: 
./raxml-ng --msa gannon_elephant_aligned4_single_lineNAMED.fasta --model HKY+G --prefix hky_run --threads 2 --seed 5

Final LogLikelihood: -4530.830609

AIC score: 9265.661218 / AICc score: 9276.970152 / BIC score: 9834.944620
Free parameters (model + branch lengths): 102

WARNING: Best ML tree contains 11 near-zero branches!

Run the GTR+G model again but include bootstrapping support:
% ./raxml-ng --msa gannon_elephant_aligned4_single_lineNAMED.fasta --model GTR+G --bootstrap 1000 --threads 2 --prefix bootstraps --seed 12345


./raxml-ng --check --msa gannon_elephant_aligned4_single_lineNAMED.fasta --model GTR+G


./raxml-ng --check --msa gannon_elephant_aligned4_single_lineNAMED.fasta.raxml.reduced.phy --model GTR+G 

./raxml-ng --parse --msa gannon_elephant_aligned4_single_lineNAMED.fasta.raxml.reduced.phy --model GTR+G

./raxml-ng --msa gannon_elephant_aligned4_single_lineNAMED.fasta.raxml.reduced.phy --model GTR+G --prefix T3 --threads 2 --seed 123  



test: DELETE
raxml-ng --bootstrap --msa prim.phy --model GTR+G --prefix T8 --seed 2 --threads 2 --bs-trees 

./raxml-ng --bootstrap --msa gannon_elephant_aligned4_single_lineNAMED.fasta.raxml.reduced.phy --model GTR+G --prefix T4 --threads 2 --seed 123 --bs-trees 200

 raxml-ng --bsconverge --bs-trees T7.raxml.bootstraps --prefix T9 --seed 2 --threads 2 --bs-cutoff 0.01

raxml-ng --all --msa prim.phy --model GTR+G --prefix T15 --seed 2 --threads 2 --bs-metric fbp,tbe

./raxml-ng --all --msa gannon_elephant_aligned4_single_lineNAMED.fasta.raxml.reduced.phy --model GTR+G --prefix T5 --seed 2 --threads 2 --bs-metric fbp,tbe


MrBayes

Description: 

A program for Bayesian inference of phylogeny using Markov Chain Monte Carlo (MCMC) methods. It estimates the posterior probability of phylogenetic trees and model parameters.

Strengths:

- Provides probability-based estimates of trees
- Can incorporate complex evolutionary models
- Supports mixed-model analysis
- Well-documented and widely used

Weaknesses:

- Computationally intensive
- Convergence can be hard to assess
- Slower than some alternatives for large datasets

Assumptions:

- Assumes a model of sequence evolution
- Assumes priors for parameters and tree topology
- MCMC sampling reflects the posterior distribution

I followed the tutorial provided here: 
https://github.com/crsl4/phylogenetics-class/blob/master/exercises/notebook-log.md 

Downloading MrBayes: 

% brew tap brewsci/bio
% brew install mrbayes --with-open-mpi

I create the MrBayes block in accordance with the research paper's data (the researchers did not use bayesian methods so all priors and such are derived from the data)

% touch mbblock2.txt
% open mbblock2.txt

begin mrbayes;
 set autoclose=yes;
 prset brlenspr=unconstrained:exp(10.0);
 prset shapepr=exp(1.333);
 prset tratiopr=beta(24.0,1.0);
 prset statefreqpr=dirichlet(3.011,2.703,1.347,2.939);
 lset nst=2 rates=invgamma ngammacat=4;
 mcmcp ngen=1000000 samplefreq=100 printfreq=100 nruns=2 nchains=4 savebrlens=yes;
 outgroup Elephas-maximus;
 mcmc;
 sumt;
end;

I create an additional text block to run the priors only, with an additional line to tell the program to ignore the data and run only the priors

% touch mbblockPriorstest.txt
% open mbblockPriorstest.txt

begin mrbayes;
 set autoclose=yes;
 prset brlenspr=unconstrained:exp(10.0);
 prset shapepr=exp(1.333);
 prset tratiopr=beta(24.0,1.0);
 prset statefreqpr=dirichlet(3.011,2.703,1.347,2.939);
 lset nst=2 rates=invgamma ngammacat=4;

 prset sampleprob=0.0;

 mcmcp ngen=1000000 samplefreq=100 printfreq=100 nruns=2 nchains=4 savebrlens=yes;
 outgroup Emi_Burma1;
 mcmc;
 sumt;
end;

% cat gannon_elephant_aligned4_single_lineNAMED.nex mbblockPriorstest.txt > gannon_elephant_aligned_single_lineNAMED-mb.nex 

% cat gannon_elephant_aligned4_single_lineNAMED.nex mbblock2.txt > gannon_elephant_aligned_single_lineNAMED-mb.nex

we run mrbayes

% mb

we execute which runs mcmc

% execute gannon_elephant_aligned_single_lineNAMED-mb.nex




Coalescent

Software Description:
ASTRAL infers species trees from a set of gene trees under the multi-species coalescent (MSC) model. It is designed to handle gene tree discordance caused by incomplete lineage sorting (ILS).

Strengths:
It is very fast, scalable to large datasets, and statistically consistent under the MSC model. ASTRAL can handle missing data and partially resolved gene trees.

Weaknesses:
ASTRAL is sensitive to errors in the input gene trees. It assumes ILS is the only cause of discordance and does not model gene flow or hybridization.

Assumptions:
The method assumes that gene trees are independent and that ILS is the primary source of discordance. It also assumes that gene tree topologies are reasonably accurate.


% java -jar astral.5.7.8.jar
This is ASTRAL version 5.7.8


We use the best tree derived from RAxML and plug it into Astral:
% java -jar astral.5.7.8.jar -i T3.raxml.bestTree\ copy -o species_tree.tre 





