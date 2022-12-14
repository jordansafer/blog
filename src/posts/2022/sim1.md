---
title: Cell Simulation
date: 2022-12-13
layout: post.pug
---

Just quickly listing off parts of the puzzle for cell simulation, I need to review Noble's "Towards Cell Simulation" review and the Stanford papers on initial versions of full cell sims. Also +1 for the biosimulators site and work in that direction which is exciting!

## Starting from the Top
So we have a cell, which contains organelles, peptides, and small molecules. There are potentially other things too.

We need to know what all the things are within the cell. We need to know what all the interactions are between all of these things. This includes organelles and small molecules. When we have complicated systems/combinations, ala protein complexes and organelles, we need good abstractions so we can simulate at different speeds and granularities.

### Things in a cell
We have rough lists of proteins via DNA, protieomics etc. We have info on RNA from RNASeq and other techniques. We have information on organelles from imaging/visual and info on small molecules via various techniques and chemistry tests. 

FAQ: Roughly what % of things do we know about? How do we know if we are missing a thing? It's hard to know if we're missing a thing if we don't know that it exists and don't see it through our existing tools

### Interactions
We have early graphs/networks that were computationally generated, like PREPPI. These cover a lot of PPIs and some other types of interactions. *Interaction networks are highly incomplete, some reasons for this include: 

1. Limited structure data, single rigid conformations for proteins
1. Challenges with simulating interactions, mdsimulations

FAQ: Do we have networks on small molecule interactions? Would we consider these annotations? How about complexes & abstractions?

### Abstractions
There are tons of simulators/modeling tools out there, ranging from md simulation tools for single molecule, to differential equation solvers for complex reaction rates/states like BioNetGen/nfsim. We probably still need tons more!

FAQ: Do we have the parameters we need for these? The review paper called out that we are missing reaction rates, which makes since given how far we are from having a complete PPI graph, let alone being able to say the rates of the PPIs. While RNASeq is giving us relative proportions, we can also get better and better answers if we know exactly where in a cell things are located, so we could have local concentration numbers.

## Starting from the bottom
At the core, we have the DNA. We've made amazing progress with DNA, with millions of sequences, lots of WGS too, and constant progress in increasing accuracy and coverage. Lots of progress in learning about expression, structure, and mutations, with great databases on mutations, sequences, and more.

Next up in the classic trio is RNA. RNASeq is great for telling us relative proportions for cell types, and potentially starting parameters for relative concentrations of proteins/rna in our cell sims!

Above that, we have proteins. From DNA/RNA and proteiomic/chem techniques like mass-spec and NMR, we have protein sequences, and this is pretty comprehensive for human nuclear DNA encoded proteins, don't overlook mitochondrial, microbiome simulation, etc. We have tons of structures for 1 conformation from XRC and AlphaFold2. There is tons of work on structures and modeling for complexes still to do!

Above this we have interaction networks and mdsimulations, so we connected up with "starting from the top"!

## Systems
Need to read up on different systems in the cell! Ie metabalomics, transcription, ubiquination, and all other systems taking place in the cell. Where do we need extra attention and where can we generalize?

Ok! This is enough for today-
