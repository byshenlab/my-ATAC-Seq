- 01- create conda env

```bash
conda create -n ATAC_seq python=3.8

conda activate ATAC_seq
conda install -c bioconda -y fastqc
conda install -c bioconda -y trimmomatic
conda install -c bioconda -y bwa
conda install -c bioconda -y samtools
conda install -c bioconda -y picard
conda install -c bioconda -y macs2
conda install -c bioconda -y bedtools
conda install -c bioconda -y deeptools
conda install -c bioconda -y homer
```

- 02- download data

```bash
# GSM2640269 Mouse normal pancreatic organoid (N5)
wget https://www.ncbi.nlm.nih.gov/geo/download/?acc=GSM2640269&format=file&file=GSM2640269%5FN5%5FATAC%2EbigWig
```

- 03- TSS/TES visulization

```bash
# TSS
computeMatrix reference-point --referencePoint TSS  -p 15 \
    -b 5000 -a 5000 \
    -R /dssg/home/acct-medsby/medsby-cdj/Reference/ATAC_seq/mm10.refseq.bed \
    -S ./GSM2640269_N5_ATAC.bigWig \
    -o TSS.gz \
    --skipZeros \
    --outFileSortedRegions Heatmap1sortedRegions.bed
    
plotHeatmap -m TSS.gz -out TSS.Heatmap.pdf --plotFileFormat pdf
plotProfile -m TSS.gz -out TSS.Profile.pdf --plotFileFormat pdf --perGroup

# TES
computeMatrix scale-regions  -p 15  \
    -b 5000 -a 5000 \
    -R ./refseq.bed \
    -S *.bw \
    --skipZeros -o body.gz
plotHeatmap -m body.gz -out body.Heatmap.pdf --plotFileFormat pdf
plotProfile -m body.gz -out body.Profile.pdf --plotFileFormat pdf --perGroup
```





