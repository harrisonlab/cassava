# cassava
Commands used in the analysis of cassava genotyping data



Cassava DartSeq data is present in the following directory, along with intial analyses as run by Bruno Santos:

```bash
ls /main/projects/cassava/results/bruno/coanalysis_iita
```


A specific project directory folder was created on the cluster
for further analyses.


```bash
  ProjDir=/projects/cassava
  mkdir -p $ProjDir
  cd $ProjDir
```

Raw data was copied to the project directory as a symbolic link from its location where it had been used for initial analyses.

```bash
DataDir=raw_dna/dartseq/cassava/run1
mkdir -p $DataDir
cp -s /main/projects/cassava/results/bruno/coanalysis_iita/DCas18_3499_cat_Target_SNPs_names_corrected.csv $DataDir/.
cp -s /main/projects/cassava/results/bruno/coanalysis_iita/individuals_duplicated.txt $DataDir/.
```

Bruno's analysis script was re-run. This was done from a new directory for data analysis.

```bash
screen -a
srun --mem=40gb --cpus-per-task=10 --partition=unlimited --time=4-0:0:0 --pty bash
mkdir -p analysis/dartseq/cassava/run1
cd analysis/dartseq/cassava/run1
# conda activate r-env
# R
/projects/software/R-3.6.1/bin/R
```

The input file listed in the R script was modified to:

```R
library("dartR")
genotypes <- gl.read.dart("/projects/cassava/raw_dna/dartseq/cassava/run1/DCas18_3499_cat_Target_SNPs_names_corrected.csv",topskip=5,lastmetric="RepAvg")
```

There was an error on line 204 that was solved by removing those options that were the same as the default settings:

```R
genotypes <- gl.filter.callrate(genotypes, method = "ind", threshold = 0.5, recalc = TRUE, v = 5)
```

A similar problem was encountered with "threshold" but this was overcome using the shortened 't'
```R
genotypes <- gl.filter.repavg(genotypes, t = 0.985, v = 2)

genotypes <- gl.filter.callrate(genotypes, method = "loc", t = 0.5, recalc = TRUE, v = 5)
```
