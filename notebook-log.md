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
% ./raxml-ng --check --msa gannon_elephant_aligned.fasta\ copy  --model GTR+G

Using the --parse command: 
% ./raxml-ng --parse --msa gannon_elephant_aligned.fasta\ copy --model GTR+G

Infer: 
./raxml-ng --msa gannon_elephant_aligned.fasta\ copy --model GTR+G --prefix T3 --threads 2 --seed 5

Final LogLikelihood: -4527.690451

AIC score: 9267.380902 / AICc score: 9279.616069 / BIC score: 9858.989144
Free parameters (model + branch lengths): 106

WARNING: Best ML tree contains 12 near-zero branches!




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

% cat gannon_elephant_aligned4_single_line.nex mbblock2.txt > gannon_elephant_aligned-mb.nex
% mb gannon_elephant_aligned-mb.nex




