# Ideas and Tips From Working
I'm creating this file to be a list of notes and history of what I've tried/completed learning new systems: [GCP](https://cloud.google.com/) and [SRA](https://www.ncbi.nlm.nih.gov/sra).

## Accessing data from the SRA
Sending information [directly to a google bucket](https://www.ncbi.nlm.nih.gov/sra/docs/data-delivery/) requires google bucket admin permission which I don't have. Instead I [downlowded the SRA toolkit](https://github.com/ncbi/sra-tools/wiki/02.-Installing-SRA-Toolkit) to my home directory on the GU hpc (Note: I used the ubuntu version of the toolkit and it works fine).

Tried fasterq-dump to bring a file down (converts sra file into fastq file for you). Two things to note: 1) Took ~13 minutes for transfer of two 11G files and 2) files are named with accession number and no identifying data. Additionally, fasterq-dump can only do a single file at a time... Outputs files into working directory
```
fasterq-dump SRR1663689
```

Tried prefetch which brings down the sra file and deposits file into the sra folder of the chosen local directory (set when configuring the sratoolkit). Bringing the sra file takes just over a minute. You can also use prfetch with an accession list. After you have the sra file you can use fasterq-dump to get the \*.fastq files (took ~6 minutes to convert 2 7.8G files)
```
prefetch SRR1663685
fasterq-dump sra_directory/sra/SRR1663685.sra
```

Considering you can only use fasterq-dump one file at a time, I'm not really sure what the most effective way to bring fastq files down from the sra site...

### Most efficient way
Apparently you can run fasterq-dump with a wildcard if you have prefetched the sra files to a local directory
```
prefetch --option-file SraAccList.txt
fasterq-dump sra_directory/sra/*.sra
```
