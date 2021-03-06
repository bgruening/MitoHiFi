# MitoHiFi 

------ This is v1 -------

MitoHiFi distributed under the [license](https://github.com/marcelauliano/MitoHiFi/blob/master/scripts/LICENSE)


ATTENTION!!!

If you want to use MitoHifi, please clone the branch mitohifi_v2


```
git clone --branch mitohifi_v2 https://github.com/marcelauliano/MitoHiFi.git
```
--------------------------------------


<b>MitoHiFi</b> circularises, cuts and annotates the mitogenome from contigs assembled with PacBio HiFi reads and softwares such as HiCanu or Hifiasm.

The dissemination of high-quality long reads - such as PacBio HiFi - makes the assembly of high-quality mitogenome straight forward. Because of the circular nature of the molecule, however, the mitocontig is usually assembled redundantly resulting in multiple-copy mitogenome-contigs. This pipeline was developed to finalise the assembly and annotation of the mitogenome by:

(i) finding it among complete genome assembled contigs     
(ii) circularising and cutting it to represent only one copy of the circular molecule and  
(iii) producing an annotation and presenting it in fasta and genbank format.

- This pipeline can also be ran starting from polished PacBio CLR contigs or ONT contigs. You must be sure your contigs are well polished.

### Dependencies

- BLAST+ (makeblastdb and blastn) have to be installed and export to your PATH
- MitoFinder: has to be installed [MitoFinder](https://github.com/RemiAllio/MitoFinder) and export to your PATH 
- Biopython
- Python Pandas

<b>Installation</b>

### Get and install MitoHiFi (Linux)

```

git clone https://github.com/marcelauliano/MitoHiFi.git

```

### Test run

```
cd MitoHiFi
cd exampleFiles
ln -s ../scripts
ln -s ../run_MitoHiFi.sh
sh run_MitoHiFi.sh -c test.fa -f NC_016067.1.fasta -g NC_016067.1.gb -t 1 -o 5

```
### Required arguments

- To run this pipeline you need 3 inputs: (i) your multifasta contig files, and a close-related species mitochondrial genome in (ii) fasta and in (iii) genbank format.

```
Usage: 'sh run_mitoHiFi.sh -c contigs.fasta -f close-related_mitogenome.fasta -g close-related_mitogenome.gb -t threads -o <integer>'
Parameters:	
	-c: assemnbled fasta contigs/scaffolds to be searched to find mitogenome
	-f: Close-related species mitogenome in fasta format
	-g: Close-related species mitogenome in genbank format 
	-t: Number of threads for the blast search 
	-o: <integer> MitoFinder parameter: Organism genetic code following NCBI table (integer):
                        1. The Standard Code 2. The Vertebrate Mitochondrial
                        Code 3. The Yeast Mitochondrial Code 4. The Mold,
                        Protozoan, and Coelenterate Mitochondrial Code and the
                        Mycoplasma/Spiroplasma Code 5. The Invertebrate
                        Mitochondrial Code 6. The Ciliate, Dasycladacean and
                        Hexamita Nuclear Code 9. The Echinoderm and Flatworm
                        Mitochondrial Code 10. The Euplotid Nuclear Code 11.
                        The Bacterial, Archaeal and Plant Plastid Code 12. The
                        Alternative Yeast Nuclear Code 13. The Ascidian
                        Mitochondrial Code 14. The Alternative Flatworm
                        Mitochondrial Code 16. Chlorophycean Mitochondrial
                        Code 21. Trematode Mitochondrial Code 22. Scenedesmus
                        obliquus Mitochondrial Code 23. Thraustochytrium
                        Mitochondrial Code 24. Pterobranchia Mitochondrial
                        Code 25. Candidate Division SR1 and Gracilibacteria
 ```
 
 ### Output
 
 ```
 mitogenome.fasta  - this is your final mitogenome in fasta format
 
Inside 'mitogenome.annotation/mitogenome.annotation_Final_Results/' you find mitogenome.annotation_mtDNA_contig.gb, which is the annotation of your mitogenome performed by mitofinder and outputed in genbank format. 
 
```
### Further

If you would like to rotate your mitogenome to start at tRNA-Phe, check the coordinates of it on your genbank file and run:
```
python scripts/rotate.py -i mitogenome.fasta -r <coordinate> > mitogenome.rotated.fa
```
If you would like to finalise another contig as the mitogenome - for studies of heteroplasmy for example - check the output <b>parsed_blast.txt</b> , choose the contig ID, use the scripts/filterfasta.py to extract only that contig and run the pipeline with it again.

```
python scripts/filterfasta.py -i contig.id <your_initial_input> > contig.id.fa

sh run_MitoHiFi.sh -c contig.id.fa -f <close-related-mito>.fasta -g <close-related-mito>.gb -t <int> -o <int>
```

 ### Description intermediate outputs
 
<b>parsed_blast.txt</b>   - tab separated file with 4 columns as follows


 - qseqid - the ID of your input contigs
 - %q_in_match - a percentage of the length of your contig in a blast match with the close related species  
 - leng_query - length of your contig
 - s_length  lenght of the close-related mitogenome given
 
 <b>contigs.blastn</b> - outfmt 6 blast output plus 2 extra columns containing respectively length_of_query and length_of_subject 
 
<b>circularisationCheck.txt</b>  - one liner separated by commas containing: the id of contig, if it circularises or not (True or False), start coordinate of circularisation, end coordinate of circularisation
 

### Citations ####



When using MitoHifi, please cite this github page and

Please cite MitoFinder:

- Allio, R, Schomaker‐Bastos, A, Romiguier, J, Prosdocimi, F, Nabholz, B, Delsuc, F. MitoFinder: Efficient automated large‐scale extraction of mitogenomic data in target enrichment phylogenomics. Mol Ecol Resour. 2020; 00: 1– 14. https://doi.org/10.1111/1755-0998.13160

And for tRNAs annotation:

- Laslett, D., & Canbäck, B. (2008). ARWEN: a program to detect tRNA genes in metazoan mitochondrial nucleotide sequences. Bioinformatics, 24(2), 172-175.

-------------------
 
 For more information mu2@sanger.ac.uk
