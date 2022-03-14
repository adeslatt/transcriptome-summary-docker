# six_frame_translation
[![Docker](https://github.com/adeslatt/six_frame_translation/actions/workflows/docker-publish.yml/badge.svg)](https://github.com/adeslatt/six_frame_translation/actions/workflows/docker-publish.yml)[![Docker Image CI](https://github.com/adeslatt/six_frame_translation/actions/workflows/docker-image.yml/badge.svg)](https://github.com/adeslatt/six_frame_translation/actions/workflows/docker-image.yml)

Six frame translation - as a container attribution [Long-Read-Proteogenomics/modules/six_frame_translation](https://github.com/sheynkman-lab/Long-Read-Proteogenomics#readme)


Pulled this out from the nextflow workflow so masterfully created by Ben Jordan of the Sheynkman Lab

## Six-Frame Translation
 * Generates a fasta file of all possible protein sequences
 * derivable from each PacBio transcript, by translating the
 * fasta file in all six frames (3+, 3-). This is used to examine
 * what peptides could theoretically match the peptides found via
 * a mass spectrometry search against GENCODE.

## Assumptions

All of the steps were hand run by mannually running each of the steps of the [Long-Read-Proteogenomics Nextflow Workflow main.nf](https://github.com/sheynkman-lab/Long-Read-Proteogenomics#main.nf) -- Future will be pulling this into a Nextflow Workflow -- but these steps are useful to understand in their completion before assembling them into a masterful Nextflow Workflow.

The data we are working with are Single Cell Long Read RNA-seq or PacBio ISOSeq files.

* PacBio IsoSeq files - in this case, these were pulled into Bar-Code Clusters determined by the partner samples short read RNAseq files.  
* Each cluster then was annotated with Sqanti
* Each cluster Filtered
* Each cluster Collapsed
* Each cluster Classified

See the [singleCellLongReadAnalysis/bin](https://github.com/Wellstein-lab/singleCellLongReadAnalysis/bin) directory for the steps broken down as separate shell scripts.

This sixframe docker image is run after having building with GitHub actions the Dockerfile in this repository and publishing it on GitHub.

First step I pulled it locally 
```bash
docker pull ghcr.io/adeslatt/six_frame_translation:sha256-ae3c04adb8bdeb613ededc737b555af5740a2329293a3549014d1c8873390d43.sig
```

Docker image build with name `sixframe`.

And then I ran the command to run this locally -- this is one example of the long filenames that result as you step through the [Long-Read-Proteogenomics Nextflow Workflow main.nf](https://github.com/sheynkman-lab/Long-Read-Proteogenomics#main.nf) 

```bash
PWD=$(pwd)

docker run --rm -v $PWD:$PWD -w $PWD -it sixframe six_frame_translation.py \
--iso_annot filtered_Blin_neg_filt_ranked_BC_clust9_B_ccsids.merge5.collapsed_corrected.5degfilter_classification.5degfilter.tsv \
--ensg_gene ensg_gene.tsv \
--sample_fasta filtered_Blin_neg_filt_ranked_BC_clust9_B_ccsids.merge5.collapsed_corrected.5degfilter.fasta \
--output_fasta filtered_Blin_neg_filt_ranked_BC_clust9_B_ccsids.merge5.collapsed_corrected.5degfilter.6frame.fasta
```

