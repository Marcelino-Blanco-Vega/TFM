# README

## Project overview

This repository contains the code used for the segmentation, quantification, visualization, and statistical analysis of three-dimensional epithelial monolayers of Madin-Darby Canine Kidney II cells. The workflow was developed to extract morpho-topological features from individual cells, identify spatial tissue patterns, and detect potential cellular subpopulations using unsupervised clustering approaches.

The analysis pipeline combines deep learning-based segmentation, manual curation, quantitative feature extraction, dimensionality reduction, clustering, and tissue-level representations.

---

## Directory structure

Before executing the scripts, directories should be organized as follows:

```
{date}_MDCKII/
├── masks_intersect/
│   └── *.tiff          # Label images obtained after label intersection
│
├── coding/
│   ├── *.Rmd           # Analysis scripts
│   └── images/         # Generated figures and image outputs
```

where `{date}` corresponds to the identifier assigned to each experiment.

---

## Workflow execution

Scripts should be executed sequentially according to the numbering in their filenames.

### '0CellposeSAM_final'

This script was used as a testing environment for optimization of CellPose-SAM segmentation parameters.

It was employed to:

* Explore segmentation settings.
* Evaluate segmentation quality.
* Adjust CellPose-SAM hyperparameters.
* Segmentation intersection: intersection of independent segmentation outputs obtained from different cellular markers to improve the reconstruction of three-dimensional cell boundaries. Nuclei-based segmentation and cytoskeleton-based segmentation were intersected using this method.

**Important:** Segmentations were not directly generated from this file.

```
masks_intersect/
```

and constitute the input for the subsequent analyses.

---

### 1Label_preprocessing

This script performs the preprocessing and curation of intersected labels.

The following steps are implemented:

* Removal of obvious segmentation artefacts.
* Automatic fusion of fragmented labels based on overlap criteria.
* Manual correction of incorrectly split objects.
* Volume filtering to eliminate debris.
* Binary closing to fill internal holes and smooth cell contours.
* Manual refinement of selected cell boundaries.
* Removal of disconnected voxel clusters.
* Exclusion of labels located at image borders.

As outputs, two final label images are generated:

* a complete curated label image containing all validated cells,
* a border-filtered label image used for downstream quantitative analyses.

---

### 2Label_quantification

This script extracts morpho-topological features from the curated labels.

Quantified measurements include:

#### Morphological features

* Cell volume.
* Cell surface area.
* Sphericity.
* Cell elongation.
* Orientation relative to the imaging plane.
* Cell height.
* Position along the apico-basal axis.

#### Apical and basal features

* Apical surface area.
* Basal surface area.
* Surface perimeters.
* Shape index.
* Apico-basal area ratios.
* Relative differences between apical and basal measurements.

#### Curvature measurements

Curvatures are calculated at two spatial scales:

* tissue-level curvature,
* cell-level curvature.

Both apical and basal curvatures are quantified.

#### Topological features

* Number of apical neighbours.
* Number of basal neighbours.
* Number of three-dimensional neighbours.
* Apico-basal neighbour exchanges.
* Identification of potential scutoid cells.

#### Local tissue context

* Local cell density.
* Tissue base determination.

The resulting measurements are exported as data tables for statistical analyses.

---

### 3Statistical_methods_UMAP

This script integrates quantitative measurements from all images and performs unsupervised analyses.

The implemented procedures include:

* Data integration across samples.
* Feature selection.
* Dimensionality reduction using Uniform Manifold Approximation and Projection.
* Clustering using Hierarchical Density-Based Spatial Clustering of Applications with Noise.
* Identification of noise points.
* Cluster assignment to individual cells.
* Bootstrap validation of clustering robustness.
* Internal clustering validation using Silhouette score, and other alternative scores not used for the final mansucript (Davies-Bouldin index, Calinski-Harabasz index)

Outputs include:

* low-dimensional embeddings,
* cluster assignments,
* validation metrics,
* files linking cluster identities to original labels and source images.

---

### 4Label_representations

This script generates visual representations of the extracted data.

Outputs include:

* Apical and basal feature maps.
* Tissue cartographies.
* Curvature representations.
* Neighbourhood maps.
* Density maps.
* Cluster projections on tissues.
* Uniform Manifold Approximation and Projection visualizations.
* Radar plots summarizing cluster phenotypes.
* Publication-ready figures used throughout the study.

Generated figures are stored in:

```
coding/images/
```

---

## Notes

* Several preprocessing stages involve manual curation. Therefore, exact reproducibility depends on the curated label datasets generated during the original analyses. Exact label modification are available upon reasonable request to Dr. Luis María Escudero laboratory team.
* Border cells are excluded from quantitative analyses to avoid artefacts derived from incomplete neighbour information.
* The workflow was developed and optimized for Madin-Darby Canine Kidney II epithelial monolayers and may require adaptation before being applied to other epithelial systems.

---

## Summary

This pipeline provides an integrated framework for transforming confocal microscopy images into quantitative morpho-topological descriptions of epithelial tissues. By combining three-dimensional segmentation, feature extraction, and unsupervised analyses, it enables the identification of tissue structures and putative cellular subpopulations that can be further investigated using future spatial transcriptomic approaches.
