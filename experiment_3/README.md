
# Parts

## ErmC flanks
The goal is to put a landing pad for any plasmid (in particular, `pOpen_v3`) into *Bacillus subtilis*. We will accomplish that using the *Bacillus subtilis REG120* strain [1]. To disrupt the current antibiotic resistance gene, ermC, we have to figure out where it came from to get the sequence. In the REG120 paper, there is mention of the original strain being derived from a transformation of `pJOE6577.1` into *B.subtilis 168*[2]. `pJOE6577.1` was derived from `pSUN346.1`, and the pSUN series plasmids ermC marker was derived from `pDG1730`[3]. Fortunately, the sequence for `pDG1730` is on genbank under the accession number [U46199.1](https://www.ncbi.nlm.nih.gov/nuccore/U46199).

In order to get the ermC marker in ref3, the gene was amplified with the following primer set:
```
s5069,AAAAAAGAATTCGATATCAGATCTACGCGTTAACCCGGGC
s5070,AAAAAACAATTGAATCGATTCACAAAAAATAGG
```

The CDS of that region we can safely assume will be present in the strain REG120, and will be our target for integration. Dr. Zeigler from the BGSC recommnends [at least 500bp of homology](https://www.researchgate.net/post/Can_Bacillus_subtilis_only_be_transformed_using_multimeric_DNA_molecules_can_you_also_digest_the_plasmid_without_religation) when doing transformations, so we will use that number as well. We don't want to have multiple resistance markers in our end strain, so we will delete a 275bp region of the ermC gene during recombination. This could potentially be repaired by environmental DNA, but this would cause the excision of our recombinant DNA, so we won't worry about repair of that resistance marker. 275bp was deleted because some of the ermC gene had extremely low GC, which would cause problems with synthesis.


## AmpR flanks
`pOpen_v3`'s ampicillin resistance marker (bla), with it's promoter, is 966bp long. So, we will cheat a little and have 483bp of homology for integration of `pOpen_v3` into the *B.subtilis REG120* genome.

## manP
In ref2, `pJOE6743.1` is the plasmid used for creating clean and marker-free deletions (also used by the [Mini Bacillus project](http://www.minibacillus.org/methods) for this use case). For the Mini Bacillus project, plasmid [`pJOE8739.1`](https://www.ncbi.nlm.nih.gov/nuccore/KY200664) was created to allow for easy insertion of DNA in *Escherichia coli* using ccdB prior to being transformed into *Bacillus subtilis*.

## Kanamycin resistance 
A selection marker is necessary to integrate the landing pad, but it is undecided whether we want to excise this positive selection marker during integration of plasmid DNA or not. On one hand, it is extremely attractive to distribute strains with no antibiotic resistance marker, but on the other hand, this possibly increases the chance of environmental contamination by a huge amount in under-resourced labs. We plan on using Kanamycin since it was [removed from the WHO Model List of Essential Medicines and the WHO Model List of Essential Medicines for Children](https://www.who.int/selection_medicines/committees/expert/22/applications/s6.2.4_kanamycin-capreomycin_MSF.pdf?ua=1).

We are going to use the Kanamycin resistance gene aph3 from [`pBSc291K`](https://www.ncbi.nlm.nih.gov/nuccore/1275510555). Most integration vectors with kanamycin resistance seem to be derived from `pDG780`, but the sequence for that plasmid isn't available, so this one will have to do. 

# Synthetic design

To review, we have 4 different moving parts:
1. The *Bacillus subtilis REG120* (ermC) homologous recombination sites (where the construct will enter our strain)
2. The ampicillin resistance (bla) homologoous recombination sites (where any plasmid will get integrated into our strain)
3. The manP negative selection marker
4. The kanamycin positive selection marker

Generally, we are going to be looking for two different constructs:
1. `ermC_1 - bla_1 - kanR - manP - bla_2 - ermC_2`
2. `ermC_1 - kanR - bla1 - manP - bla_2 - ermC_2`

The first will allow for completely antibiotic-free distribution of DNA. However, if while testing the system we find that the antibiotic-free methods are difficult for labs to handle, we will use the second construct, where plasmid integration will not remove the kanamycin resistance site. 

Both constructs come out to 5147bp, which is larger than our Twist synthesis budget. In order to get around this limitation, we are synthesizing 3 different parts which recombine to create our two constructs:

1. prefix_1 : `ermC_1 - bla_1 - kanR`
2. prefix_2 : `ermC_1 - kanR - bla_1`
3. suffix : `manP - bla_2 - ermC_2`

In order to physically put the parts together, flanking the prefixs and the suffix are BbsI sites, which will allow `prefix - suffix` combinations. There are also sticky sites that allow for `suffix - prefix` combination, which allows for concatemerization of the constructs in a `- prefix - suffix - prefix - suffix - ` pattern, which [has been shown to increase transformation efficiency in *Bacillus subtilis*](https://dx.doi.org/10.1111%2Fj.1751-7915.2010.00230.x). 

# References
Plasmids, fragments for synthesis, and final constructs are available in this directory in .dna files. Please note that slightly different notation is used in the construct and synthesis .dna files than are used in this document. 

1. [Construction of a Super-Competent Bacillus subtilis 168 Using the PmtlA-comKS Inducible Cassette](https://dx.doi.org/10.3389%2Ffmicb.2015.01431)
2. [Development of a markerless gene deletion system for Bacillus subtilis based on the mannose phosphoenolpyruvate-dependent phosphotransferase system](https://doi.org/10.1099/mic.0.000150)
3. [Characterization of a Mannose Utilization System in Bacillus subtilis](https://dx.doi.org/10.1128%2FJB.01673-09)
4. [Large-scale reduction of the Bacillus subtilis genome: consequences for the transcriptional network, resource allocation, and metabolism](https://dx.doi.org/10.1101%2Fgr.215293.116)
