<html>
<body>
<h2>sPARTA</h2>
<b>s</b>mall RNA-<b>PA</b>RE <b>T</b>arget <b>A</b>nalyzer<br> 
Updated: version-1.20 09/30/2016    
Download link: https://github.com/atulkakrana/sPARTA.github/releases
<h3><b>Description</b></h3>
small RNA-PARE Target Analyzer (sPARTA) is a tool which utilizes
high-throughput sequencing to profile genome-wide cleavage products.
sPARTA begins with a built-in parallelized target prediction module for plant
miRNAs called</body></html> `miRferno`. <html><body>sPARTA as a whole utilizes multi-core servers to
achieve two-dimensional parallelization in order to maintain a low memory
footprint, imperative to achieve a full genome analysis. 
<h3><b>Dependencies</b></h3>
<b>sPARTA</b> requires bowtie2 in the PATH variable of the user account executing sPARTA<br>
</body>
</html>
`bowtie2` <html><body>may be downloaded here http://bowtie-bio.sourceforge.net/bowtie2/index.shtml<br>

sPARTA requires the following python3 functions to perform properly:<br></body></html>
`numpy` - <html><body>http://www.numpy.org/<br></body></html>
`scipy` - <html><body>http://www.scipy.org/<br>
<br>
These may easily be installed using (Python) </body></html>`PIP`. <html><body>Intructions to install</body></html>`PIP` -<html><body> https://pip.pypa.io/en/stable/installing.html<br>
<h3><b>Note</b></h3>
</body>
</html>
**1**. In case your libraries needs pre-processing i.e. adapter removal and trimming, use pre-processing script here: [https://github.com/atulkakrana/helper.github](https://github.com/atulkakrana/helper.github)

**2**.sPARTA uses file extensions to identify file types, naming meta-data and selectively cleaning up temp files. Therefore, it is recommended to have appropriate file extensions.
For Ex. a genome/cDNA FASTA file should have `.fa` extension.
Please see 'Arguments' section (below) for recommended file extensions.

**3**.Make sure that input fasta files do not have integers in name. For ex - test.1.fa or arabidopsis.new.2.4.fa
Files with such names are deleted sometimes while cleanup operation

**4**.All the input files 1) </body></html>`miRNAs` 2)`FASTA` file for genome or transcripts and 3) `degradome/PARE` in tag-count format should be in same directory, including sPARTA script
<html>
</body>
<h3><b>Execution</b></h3>
<p>There are command line arguments that are to be used by sPARTA for proper
execution. For the first execution, all steps must be performed, but
once this has been completed, provided the miRNAs and genome are the same,
the entire analysis will not need to be repeated.  <a href="https://github.com/sdeepti/atulChange/blob/master/README.md#examples">Examples</a> of such cases
may be seen below.</p>
<h3><b>Arguments</b></h3><br>
<table>
<tr>
<td>-annoFile</td>        
<td>GFF3 file for the species being analyzed corresponding  to the genome assembly being used. Recommended file
 extension - '.gff3' or '.gff3'</td>
</tr>
<td>-annoType</td>        
<td>The annotation file format. Currently GFF and GTF formats are supported. This option is used with and corresponds
 to the annoFile option</td>
</tr>
<tr>
<td>-genomeFile</td>      
<td>Genome file in FASTA format that will be used to extract features (genic or intergenic regions) using GFF3 file.
Recommended file extension - '.fa'</td>
</tr>
<td>-featureFile</td>
<td>FASTA file containing sequences of interest (CDS, transcript,
 intergenic regions etc.) if user already has a set of
 sequences. This option is mutually exclusive to genome file and
gff3 file. So either genomefile along with annoFile is used or
 feature set is supplied directly. Recommended file extension - '.fa'</td>
</tr>
<tr><td width="180">-genomeFeature</td>
<td>0 if prediction is to be done in genic region. 1 if prediction
 is to be done in intergenic region</td>
</tr>
<tr><td>-miRNAFile</td><td> FASTA format of miRNA sequences. Recommended file extension - '.fa'</td></tr>
<tr><td>-tarPred</td><td>Mode of target prediction. H for heuristic. E for exhaustive.
 H is default if no mode is specified</td></tr>
<tr><td>-tarScore</td><td>Scoring mode for target prediction. S for seedless. N for
normal. S is default if no mode is specified</td></tr>
<tr><td>-libs</td><td> List of PARE library files in tag count format. Data can be
 easily converted into tag count format using <a href="http://www.ebi.ac.uk/~stijn/reaper/tally.html" target="_blank">tally</a></td></tr>
<tr><td>-minTagLen</td>       
<td> Minimum length of PARE tag. Tags shorter than minTagLen will be
discarded. 20 is default</td></tr>
<tr><td>-maxTagLen</td>
<td> Maximum length of PARE tag. Tags longer than maxTagLen will be
chopped to the specified length. 30 is default
<tr><td>-accel</td> 
<td>Y to use balanced multiple process scheme or else specify the
             number of processors to be used. Y is default</td></tr>
<tr><td>--tag2FASTA</td>  
<td>Convert tag count file for PARE libraries to FASTA files for
mapping</td></tr>
<tr><td>--map2DD</td>
<td> Map the PARE reads to feature set</td>
<tr><td>--validate</td> 
<td>Flag to perform the validation of the potential cleave sites
 from miRferno</td></tr>
<tr><td>--repeats</td>      
<td> Flag to include PARE reads from repetitive regions</th></td></tr>
<tr><td>--noiseFilter</td>
<td> Flag to include all PARE validations with p-value of <=.5,
             irrespective of the noise to signal ratio at cleave site and
         category of PARE read.</td></tr>
</table>
<br>
<h3><b>Genome and Annotation Data</b></h3>
Both the </body></html>`GFF3` file and corresponding genome `FASTA` <html><body>file can be downloaded from
Phytozome [http://www.phytozome.net/]<br>

<h3><b>Examples</b></h3>
</body>
</html>
**1**. Execution on new genome/entirely new dataset
This execution should be performed any time a new genome file (along with corresponding `GFF` file) is being analyzed:
```
python3 sPARTA.py -genomeFile <genomeFile.fa> -annoType <GTF/GFF> -annoFile <annotationfile> -genomeFeature <0/1> -miRNAFile <miRNAFile.fa> -libs <Lib_A.txt Lib_B.txt> -tarPred -tarScore --tag2FASTA --map2DD --validate
```
**or**

a user provided feature set (FASTA file with sequences of interest) is being analyzed:

```
python3 sPARTA.py -featureFile <featureFile.fa> -genomeFeature <0/1> -miRNAFile <miRNAFile.fa> -libs <Lib_A.txt Lib_B.txt> -tarPred -tarScore --tag2FASTA --map2DD --validate
```
**2**. Execution on genome in which genome has already been processed
This execution should be performed if a genome file has been processed previously but the miRNAs for which targets need to be predicted are new:
```
python3 sPARTA.py -genomeFeature <0/1> -miRNAFile <miRNAFile.fa> -libs <Lib_A.txt Lib_B.txt> -tarPred -tarScore --tag2FASTA --map2DD --validate
```
**3**. Execution on data in which genome and miRNA files have been previously processed
This execution should be performed if targets for a genome file have already been predicted using a miRNA file, but new PARE libraries need to be used for validation of earlier predicted targets:
```
python3 sPARTA.py -genomeFeature <0/1> -libs <Lib_C.txt Lib_D.txt> --map2DD --validate
```

**4**. Execution of 'miRferno', just for target prediction
This execution should be performed in case only predicted targets are required or PARE libraries are not available:
```
python3 sPARTA.py -genomeFile <genomeFile.fa> -annoType <GTF/GFF> -annoFile <annotationfile> -genomeFeature <0/1> -miRNAFile <miRNAFile.fa> -tarPred -tarScore
```
**or**

a user provided feature set (FASTA file with sequences of interest) is being analyzed:
```
python3 sPARTA.py -featureFile <featureFile.fa> -genomeFeature <0/1> -miRNAFile <miRNAFile.fa> -tarPred -tarScore
```
<html>
<body>
<h3><b>Output</b></h3>
</body>
</html>
1.PARE validation results for each library can be found in `output` folder under its corresponding library name. The `output` folder also contains a combined result file `AllLibValidatedUniq.csv` from all the libraries.
Results from all libs were combined by removing redundant miRNA-target interaction with cleavage at same site.

2.Target prediction results can be found in 'predicted' folder under the name
`All.targs.parsed.csv`

## Other scripts

**revFernoMap.py** : This script generates new file with genomic co-ordinates for predicted targets i.e. targets in `All.targs.parsed.csv` file under the `predicted` folder. It is neither part of sPARTA nor required for prediction and/or validation of targets. Instead, it might be useful for specific studies that need genomic co-ordinates for predicted targets.

Predicted targets could be huge in number, depending upon the size of the genome and number of sRNAs used as query, therefore the resulting file is usually big in size. This new script to reverse map predited targets, makes use of parallel processing to return results (with genomic co-ordinates) in a reasonable time. To use `revFernoMap.py` script, simply copy it in the "predicted" generated by sPARTA or miRferno during target prediction step, and execute:

```python3 revFernoMap.py```

A successful run will create a new subfolder "revMapped" inside the "predicted" folder. This new file will have predited targets with genomic co-ordinates.

<html>
<body>
<h3>Publications</h3>
Kakrana, A., Hammond, R., Patel, P., Nakano, M. & Meyers, B. C. sPARTA: a parallelized pipeline for integrated analysis of plant miRNA and cleaved mRNA data sets, including new miRNA target-identification software. Nucleic Acids Res. gku693– (2014).  doi:10.1093/nar/gku693 <a href="http://www.ncbi.nlm.nih.gov/pubmed/25120269">[Link]</a><br><br>

Arikit, S., Xia, R., Kakrana, A., Huang, K., Zhai, J., Yan, Z., … Meyers, B. C. (2014). An Atlas of Soybean Small RNAs Identifies Phased siRNAs from Hundreds of Coding Genes. The Plant Cell. http://doi.org/10.1105/tpc.114.131847 <a href="http://www.ncbi.nlm.nih.gov/pubmed/25465409">[Link]</a>
<h3><b>Contact</b></h3>
Atul Kakrana<br>
kakrana@udel.edu<br>
Reza Hammond<br>
hammond@dbi.udel.edu<br>

</body>
</html>



