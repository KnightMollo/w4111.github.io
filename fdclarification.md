---
layout: page
---

# Clarification about Functional Dependency Decomposition

This document clarifies decomposition for functional dependencies.  What I demonstrated in class was misleading.

## BCNF

In BCNF, you perform decomposition based on FDs implied by armstrong's axioms.

        DecomposedRelations = { R }

        while DecomposedRelations is not empty
          R = pop a relation from DecomposedRelations
          FR = subset of FDs in R's projection
          Does there exist an fd X->Y in FR's closure that violates BCNF?
          if yes:
            NewRs = decompose R using X->Y
            add NewRs to DecomposedRelations
            continue while loop


Thus, you continue until all fds implied by your set of functional dependencies do not violate BCNF for the decomposition.  

What this means is that your decomposition may have been lossy and thus lose potential keys!  Take the example from the lecture notes:

        R:   ABCDE
        FDS: A->BCDE, BC->A, D->B, C->D

        # suppose we first check D->B:

        D doesn't determine R, thus we decompose into: BD, ACDE

        Let's check BD:
        
          Only D->B is in its projection, thus its in BCNF.  
          
        Let's check ACDE:
        
          Only C->D is in its projection, so ACDE is redundant.
          Decompose into: CD, ACE

        ACE has empty projection

        Thus, there are no FDs that violate BCNF for the decompositon: BD, CD, ACE


## 3NF

In 3NF, you first compute the minimal cover of the FDs.  Note that the minimal cover may not always be unique.  Once you have the minimal cover, the procedure is similar to BCNF, with two key differences

1. checking violation of `X->Y` in 3NF also allows Y to be part of a key (minimal key).
2. once you have performed BCNF decomposition, any lost dependencies are added as new relations.
