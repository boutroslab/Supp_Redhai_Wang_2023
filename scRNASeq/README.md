# Summary 

This part of the repository contains Rmd notebooks for the sc-RNA-Seq analysis. 
Raw data contains preprocessed Seurat objects with annotated cell types for each 
sample as rds files. 
To perform DE analysis and plot umaps the full analysis does not need to be run, 
only if one wants to recapitulate the analysis steps.

# Overview of scripts

## predict_celltypes.Rmd

Uses flycellatlas data as reference to automatically annotate cell types. 
Cell types that are not expected in the gut are manually removed. Cells whose 
type could not predicted are named unk and annotated in manual_celltype_assigment.Rmd.

## integrate_replicates.Rmd

Integrates the replicates for each condition using singleR.

## manual_celltype_assignment.Rmd

Annotate unk cells whose type could not be predicted on the integrated conditions.
Annotation is done manually based on clustering and marker genes.

## DE_analysis.Rmd

Loads all samples and aggregates to pseudobulks per cell type 
(use $flyatlas_annotation_clean_noLFC in Seurat object). DESeq2 is used 
to get DE genes between conditions and control and conditions and FBNR. Results 
are plotted as volcano plots. Based on the DE results GSEA is performed and 
barplots are plotted.

## plot_umap

Plots umap of controls and shows integration of replicates.

# Full Analysis workflow

1) use {id}_preprocessed_srt.rds as input
2) run predict_celltypes.Rmd 
3) run integrate_replicates.Rmd
4) use integrated replicates {cond}_seurat_integrated.rds to rename unk with manual_celltype_assignment.Rmd
5) add unk annotations to {id}_preprocessed_srt.rds
6) perform DE analysis on $flyatlas_annotation_clean_noLFC and plot UMAP
