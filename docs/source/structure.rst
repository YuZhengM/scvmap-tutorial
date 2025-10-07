1. scVMAP process
==============================================

 | scVMAP: https://bio.liclab.net/scvmap/
 | scVMAP reproducibility:: https://github.com/YuZhengM/scvmap_reproducibility
 | scVMAP tutorial: https://scvmap.readthedocs.io/en/latest/
 | scVMAP front-end: https://github.com/YuZhengM/scvmap_web
 | scVMAP back-end: https://github.com/YuZhengM/scvmap
 | scVMAP API: https://bio.liclab.net/scvmap_service/swagger-ui/index.html

`scVMAP <https://bio.liclab.net/scvmap/>`_ gathers and provides scATAC-seq datasets with
established cell type labels (from scATAC-Ref, GreenleafLab and PlaqView) and cell type
labels annotated by singleR (from scBlood), along with detailed fine-mapping data (from
CAUSALdb2, FinnGen, UKBB and BBJ). scVMAP, by employing the SCAVENGE and g-chromVAR methods,
assigns trait-relevant scores at single-cell resolution, uncovering biologically
relevant cell types.

The core original intention and purpose of `scVMAP <https://bio.liclab.net/scvmap/>`_ is to provide a resource of trait-relevant significant cell types, offering researchers interested in this objective a convenient and fast retrieval interface.
It uses widely recognized methods to provide reference for key variants, TFs, and genes involved in relevant potential enrichment mechanisms, offering researchers a direction for reference, reducing their time, and making a modest contribution to the scientific research community.

Throughout the process, data undergoes standardization, unified processing, and comprehensive analysis. The main contents are as follows:

+ Single-cell samples:
  * Differential gene activity analysis of cell types
  * Differential TF activity analysis of cell types
  * High-scoring gene pathway enrichment analysis of cell types

+ Traits or diseases:
  * LD-based MAGMA analysis (Trait-relevant genes)
  * HOMER motif enrichment analysis (Trait-relevant TFs)
  * MAGMA-based trait-relevant gene pathway enrichment analysis

+ Integrated analysis:
  * ATAC-based Cicero analysis (Trait-relevant genes)
  * ATAC-based GimmeMotifs analysis (Trait-relevant genes)
  * Gene hub regulatory network analysis
  * TF hub regulatory network analysis
  * Integrated regulatory network analysis

The `scVMAP <https://bio.liclab.net/scvmap/>`_ database primarily provides the following functions: ``Data-browse``, ``Search``, ``Analysis``, ``Statistics``, ``Download``, ``Contact``, and ``Online analysis``.
These functionalities streamline the process of studying single-cell genetic regulation, enabling users to easily explore and interpret biological mechanisms in specific contexts.

In conclusion, (scVMAP, https://bio.liclab.net/scvmap/) is a user-centric database that facilitates the exploration and interpretation of single-cell genetic data through structured displays and dynamic visualizations. It offers intuitive workflows, customizable parameters and comprehensive data accessibility.

.. image:: ./img/overview.png


.. tip::

    This section provides a comprehensive explanation of the entire pipeline for the scVMAP platform, encompassing the stages of data acquisition, curation, analysis, and the ultimate platform realization.


1.1 Data collection and curation
--------------------------------

The scVMAP platform collects scATAC seq data and fine mapping result data.

1.1.1 Collection of scATAC-seq data
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The collected scATAC-seq data mainly comes from ``scATAC-Ref``, ``scBlood``, ``GreenleafLab``, and ``PlaqView`` sources.

A total of ``183`` single-cell sample data were collected, with detailed information as follows:

 | Download url: `sample_info_with_age_sex_drug.txt <https://bio.liclab.net/scvmap_static/download/overview/sample_info_with_age_sex_drug.txt>`_

.. note::

    When preprocessing scATAC-seq data through `SnapATAC2 <https://scverse.org/SnapATAC2/>`_, only the ``min_tsse`` parameter varies for different samples. (Excluding input files and reference genome information.)

1.1.1.1 scATAC-Ref
""""""""""""""""""""""""""

 | Source url: `https://bio.liclab.net/scATAC-Ref/ <https://bio.liclab.net/scATAC-Ref/>`_

We downloaded human data from scATAC-Ref and retained a total of 28 scATAC-seq datasets.
We integrated all GSM samples from GSE195460 in scATAC-Ref to create the dataset "sample_id_30 (GSE195460_diabetic_kidney)".

Detailed processing steps:

1. Download the RDS files.
#. Standardize the filenames to the format: ``<sample_label>_ATAC.rds``.
#. Generate the metadata files: ``barcodes.tsv``, ``matrix.mtx``, and ``peaks.bed``.
#. Generate the ``<sample_label>_fragments.tsv.gz`` file.
#. Perform preprocessing using the SnapATAC2 software with the following specific parameters:

.. code-block:: python

    min_num_fragments = 200,
    bin_size = 500,
    min_tsse = <min_tsse>,
    need_features = 500000,
    min_cells = 5,

    data = snap.pp.import_data(
        fragment_file="<sample_label>_fragments.tsv.gz",
        chrom_sizes=<genome_anno>,
        file=<h5ad_file>,
        min_num_fragments=min_num_fragments,
        sorted_by_barcode=False
    )

    # the standard procedures to add tile matrices, select features, and identify doublets
    snap.metrics.tsse(data, <genome_anno>)

    snap.pp.filter_cells(data, min_tsse=min_tsse)
    snap.pp.add_tile_matrix(data, bin_size=bin_size)
    snap.pp.select_features(data, n_features=need_features)

    snap.pp.scrublet(data, features=features)
    snap.pp.filter_doublets(data)


Please see `scVMAP-reproducibility-SnapATAC2 <https://github.com/YuZhengM/scvmap_reproducibility/tree/main/scATAC/SnapATAC2>`_ for the detailed workflow code.

.. note::

    The cell type labels were annotated based on the original publications of each scATAC-seq sample and are highly reliable.


For all single-cell samples except "sample_id_20" (Alzheimer’s Disease (AD)), we utilized the uniform manifold approximation and projection (UMAP) coordinates from their original collection sources for clustering. Due to the poor clustering performance of the original UMAP coordinates for "sample_id_20", we processed the binarized sparse counts matrix using SnapATAC2, converted it to a term frequency-inverse document frequency (TF-IDF) matrix, and subsequently extracted representative low-dimensional features through latent semantic indexing (LSI). Finally, we performed UMAP on this low-dimensional feature matrix to obtain the coordinates for "sample_id_20".


1.1.1.2 scBlood
""""""""""""""""""""""""""

 | Source url: `https://bio.liclab.net/scBlood/ <https://bio.liclab.net/scBlood/>`_

We downloaded human data from scBlood and retained a total of 152 scATAC-seq datasets.

The processing pipeline is identical to that of scATAC-Ref.

.. note::

    The cell type labels were annotated with SingleR. Their reliability should be treated as indicative.

1.1.1.3 GreenleafLab
""""""""""""""""""""""""""

 | Source url: `https://github.com/GreenleafLab/MPAL-Single-Cell-2019 <https://github.com/GreenleafLab/MPAL-Single-Cell-2019>`_

We downloaded a scATAC-seq dataset for PBMC.

The processing pipeline is identical to that of scATAC-Ref.

1.1.1.4 PlaqView
""""""""""""""""""""""""""

 | Source url: `https://www.plaqview.com/ <https://www.plaqview.com/>`_

We downloaded a scATAC-seq dataset for coronary artery disease (CAD).

The processing pipeline is identical to that of scATAC-Ref.

1.1.1.5 Summary
""""""""""""""""""""""""""

Here are the specific parameter settings for ``min_tsse``.

================= ===============
Sample ID         min_tsse
================= ===============
sample_id_1-30    8
sample_id_31-183  5
================= ===============

The scATAC-seq data is obtained through `download <https://bio.liclab.net/scvmap/download>`_ page. Once read, the ``adata.obs['tsse']`` information can be accessed.

Cell type annotations were directly assigned from their original articles, whereas the scATAC-seq samples obtained from scBlood were annotated using the SingleR software.

Besides cell type annotation, we also performed annotation for age, sex, and drug resistance, involving 24, 19, and 2 samples, respectively.
It can be viewed via the `browser <https://bio.liclab.net/scvmap/data_browse>`_ page.

1.1.2 Collection of trait data
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

 | FINEMAP fine-mapping result download url (15805): `trait_info.xlsx <https://bio.liclab.net/scvmap_static/download/overview/trait_info.xlsx>`_
 | SuSiE fine-mapping result download url (79): `trait_info_susie.xlsx <https://bio.liclab.net/scvmap_static/download/overview/trait_info_susie.xlsx>`_

1.1.2.1 FINEMAP
""""""""""""""""""""""""""

The collected FINEMAP fine-mapping result data comes from ``CAUSALdb2``, ``UKBB``, ``FinnGen``, and ``BJJ`` sources.

============ ============ ========================== ============================== ========= ============== ============ ============= ==============================================================================================================================  ================
Source ID    Source name  Source author, year        Source description             PMID      Source genome  Trait count  Filter count Source link                                                                                                                    Source version
============ ============ ========================== ============================== ========= ============== ============ ============= ==============================================================================================================================  ================
source_id_1  CAUSALdb     Jianhua Wang et al., 2024  CAUSALdb fine-mapping results  31691819  hg19           15038        14417         `http://www.mulinlab.org/causaldb/index.html <http://www.mulinlab.org/causaldb/index.html>`_                                   Release 2.1
source_id_2  UKBB         Wang, Q.S. et al., 2021    UKBB fine-mapping results      -         hg19           94           94            `https://www.finucanelab.org/data <https://www.finucanelab.org/data>`_                                                         Release 1.1
source_id_3  FinnGen      Kurki, M.I. et al., 2023   FinnGen fine-mapping results   36653562  hg38           1234         1215          `https://www.finngen.fi/en/access_results <https://www.finngen.fi/en/access_results>`_                                         R11
source_id_4  BBJ          Kanai, M. et al., 2021     BBJ fine-mapping results       -         hg19           79           79            `https://humandbs.dbcls.jp/en/hum0197-v18#hum0197.v5.gwas.v1 <https://humandbs.dbcls.jp/en/hum0197-v18#hum0197.v5.gwas.v1>`_   -
============ ============ ========================== ============================== ========= ============== ============ ============= ==============================================================================================================================  ================

For each trait, we retained the variants with a causal variant ``probability value (PP) > 0.001`` calculated by FINEMAP. As a result, we retained ``15,805`` traits from the initial ``16,445`` traits and used them for subsequent analysis.

1.1.2.2 SuSiE
""""""""""""""""""""""""""

The collected 79 SuSiE fine-mapping results (PP > 0.001) come from ``BJJ`` source.

1.1.2.3 Summary
""""""""""""""""""""""""""

The trait data is obtained through `download <https://bio.liclab.net/scvmap/download>`_ page.

To harmonize genomic coordinates between variants and scATAC-seq data, we performed LiftOver to convert variant positions to match the reference genome version used in single-cell analysis.
Next, we manually categorized them into a broad array of classifications, including diseases, indicators, drugs, chemical compounds, health care, treatments, therapies and symptoms.
For the disease category, we annotated diseases according to ICD-10, encompassing all ``22`` major disease categories, and further subclassifying them into more than ``250`` specific subcategories to provide an intuitive and convenient reference.

Please see `scVMAP-reproducibility-Trait <https://github.com/YuZhengM/scvmap_reproducibility/tree/main/variant>`_ for the detailed workflow code.

It can be viewed via the `browser <https://bio.liclab.net/scvmap/data_browse>`_ page.
