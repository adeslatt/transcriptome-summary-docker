[![Docker](https://github.com/adeslatt/transcriptome-summary-docker/actions/workflows/docker-publish.yml/badge.svg)](https://github.com/adeslatt/transcriptome-summary-docker/actions/workflows/docker-publish.yml)[![Docker Image CI](https://github.com/adeslatt/transcriptome-summary-docker/actions/workflows/docker-image.yml/badge.svg)](https://github.com/adeslatt/transcriptome-summary-docker/actions/workflows/docker-image.yml)
# Transcriptome Summary

This module prepares an isoform classification table and a gene info table 

Inputs:
* sqanti classification file
* (removed for this Container for now - kalliso tpm file)
* (removed for this Container for now -  kalliso ribodepletion tpm file)
* ensg_to_gene mapping file 
* enst_to_isoname mapping file 
* gene_len_stats table 

Outputs:
* sqanti isoform table 
* gene level info table 

For running the transcriptome summary per cluster

                                                                                                                                                                                               
THE MOST IMPORTANT THING HERE IS THE PRODUCTION OF THE PACBIO-GENE REFERENCE TABLE                                                                                                            
In our case here we will have one pacbio-gene reference per cluster                                                                                                                           
                                                                                                                                                                                               
input:                                                                                                                                                                                       
    file(sqanti_classification) from ch_sample_classification_transcriptome                                                                                                                    
    file(tpm) from ch_sample_kallisto                                                                                                                                                          
    file(ribo) from ch_normalized_ribo_kallisto                                                                                                                                                
    file(ensg_to_gene) from ch_ensg_gene                                                                                                                                                       
    file(enst_to_isoname) from ch_enst_isoname                                                                                                                                                 
    file(len_stats) from ch_gene_lens_transcriptome                                                                                                                                            
output:                                                                                                                                                                                       
  file("gene_level_tab.tsv") into ch_gene_level                                                                                                                                                
  file("sqanti_isoform_info.tsv") into ch_sqanti_isoform_info                                                                                                                                  
  file("pb_gene.tsv") into ch_pb_gene                                                                                                                                                          

PacBio IsoSeq files - in this case, these were pulled into Bar-Code Clusters determined by the partner samples short read RNAseq files.  
* Each cluster then was annotated with Sqanti
* Each cluster Filtered
* Each cluster Collapsed
* Each cluster Classified

See the [singleCellLongReadAnalysis/bin](https://github.com/Wellstein-lab/singleCellLongReadAnalysis/bin) directory for the steps broken down as separate shell scripts.

This transcriptome-summary-docker image is run after having building with GitHub actions the Dockerfile in this repository and publishing it on GitHub.

First step I pulled it locally 
```bash
docker pull ghcr.io/adeslatt/transcriptome_summary:latest
```

Docker image build with name `transcriptome_summary`.

And then I ran the command to run this locally -- this is one example of the long filenames that result as you step through the [Long-Read-Proteogenomics Nextflow Workflow main.nf](https://github.com/sheynkman-lab/Long-Read-Proteogenomics#main.nf) 

```bash
PWD=$(pwd)

docker run --rm -v $PWD:$PWD -w $PWD -it sixframe six_frame_translation.py \
--iso_annot filtered_Blin_neg_filt_ranked_BC_clust9_B_ccsids.merge5.collapsed_corrected.5degfilter_classification.5degfilter.tsv \
--ensg_gene ensg_gene.tsv \
--sample_fasta filtered_Blin_neg_filt_ranked_BC_clust9_B_ccsids.merge5.collapsed_corrected.5degfilter.fasta \
--output_fasta filtered_Blin_neg_filt_ranked_BC_clust9_B_ccsids.merge5.collapsed_corrected.5degfilter.6frame.fasta
```

