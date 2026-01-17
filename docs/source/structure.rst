1. scVMAP process
==============================================

 | scVMAP: https://bio.liclab.net/scvmap/
 | scVMAP reproducibility: https://github.com/YuZhengM/scvmap_reproducibility
 | scVMAP tutorial: https://scvmap.readthedocs.io/en/latest/
 | scVMAP front-end: https://github.com/YuZhengM/scvmap_web
 | scVMAP back-end: https://github.com/YuZhengM/scvmap
 | scVMAP API: https://bio.liclab.net/scvmap_service/scvmap.html

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

The scVMAP platform collects scATAC-seq data and fine-mapping result data.

1.1.1 Collection of scATAC-seq data
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The collected scATAC-seq data mainly comes from ``scATAC-Ref``, ``scBlood``, ``GreenleafLab``, and ``PlaqView`` sources.

A total of ``183`` single-cell sample data were collected, with detailed information as follows:

 | Download url: `sample_info_with_age_sex_drug.txt <https://bio.liclab.net/scvmap_static/download/overview/sample_info_with_age_sex_drug.txt>`_

.. note::

    When preprocessing scATAC-seq data through `SnapATAC2 <https://scverse.org/SnapATAC2/>`_, only the ``min_tsse`` parameter varies for different samples (Excluding input files and reference genome information).

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

The scATAC-seq data is obtained through `download <https://bio.liclab.net/scvmap/download>`_ page.

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

============ ============ ========================== ============================== ========= ============== ============ ============= ============================================================ ================
Source ID    Source name  Source author, year        Source description             PMID      Source genome  Trait count  Filter count  Source link                                                  Source version
============ ============ ========================== ============================== ========= ============== ============ ============= ============================================================ ================
source_id_1  CAUSALdb     Jianhua Wang et al., 2024  CAUSALdb fine-mapping results  31691819  hg19           15038        14417         http://www.mulinlab.org/causaldb/index.html                  Release 2.1
source_id_2  UKBB         Wang, Q.S. et al., 2021    UKBB fine-mapping results      —         hg19           94           94            https://www.finucanelab.org/data                             Release 1.1
source_id_3  FinnGen      Kurki, M.I. et al., 2023   FinnGen fine-mapping results   36653562  hg38           1234         1215          https://www.finngen.fi/en/access_results                     R11
source_id_4  BBJ          Kanai, M. et al., 2021     BBJ fine-mapping results       —         hg19           79           79            https://humandbs.dbcls.jp/en/hum0197-v18#hum0197.v5.gwas.v1  —
============ ============ ========================== ============================== ========= ============== ============ ============= ============================================================ ================

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

.. tip::

    Currently, SuSiE fine-mapping results can only be viewed through the "Data-browse" interface. Other interfaces, such as "Search" and "Analysis", do not retrieve SuSiE data and are limited to ``FINEMAP`` results. Support for ``SuSiE`` data in these interfaces will be added in a future update. In addition, we will gradually expand the data of SuSiE method and other fine mapping methods.

1.2 Variant-function-mapping at single cell resolution
------------------------------------------------------

Our preferred method for calculating trait relevance scores (TRSs) is `SCAVENGE <https://doi.org/10.1038/s41587-022-01341-y>`_.
Since the `SCAVENGE <https://doi.org/10.1038/s41587-022-01341-y>`_ method utilizes the `g-chromVAR <https://doi.org/10.1038/s41588-019-0362-6>`_ approach, scVMAP supports both `g-chromVAR <https://doi.org/10.1038/s41588-019-0362-6>`_ and `SCAVENGE <https://doi.org/10.1038/s41587-022-01341-y>`_ for computing TRSs.

Specific process code: `scVMAP-R <https://github.com/YuZhengM/scvmap_reproducibility/tree/main/R>`_

1.2.1 g-chromVAR
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Building upon the original `g-chromVAR <https://doi.org/10.1038/s41588-019-0362-6>`_ codebase, we have addressed the issues caused by ``NA/INF`` values and refined the corresponding implementation.

1.2.2 SCAVENGE
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The values of some parameter settings.

========================================================= ===============
Hyper parameter                                           Value
========================================================= ===============
Dimensionality of latent semantic indexing (LSI)          30
Number of neighbors in mutual k-nearest neighbors (M-kNN) 30
Restart value for random walk                             0.05
========================================================= ===============

1.2.3 Code for using scVMAP
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

We have consolidated the SCAVENGE workflow into a single command. You can view the code here: `scVMAP-exec_R_code <https://github.com/YuZhengM/scvmap_reproducibility/tree/main/R/exec_R_code>`_

The specific implementation process is as follows:

1.2.3.1 Install R packages
"""""""""""""""""""""""""""""""""""""

Refer to file `scVMAP-install_R_packages <https://github.com/YuZhengM/scvmap_reproducibility/blob/main/R/R_code/install.R>`_ for installation.

1.2.3.2 Download files
"""""""""""""""""""""""""""""""""""""

The following two files need to be downloaded through link `scVMAP-exec_R_code <https://github.com/YuZhengM/scvmap_reproducibility/tree/main/R/exec_R_code>`_:
 - ``integration.R``: A file for g-chromVAR and SCAVENGE algorithm code.
 - ``run.R``: An R script for executing the g-chromVAR and SCAVENGE algorithms via ``Rscript`` in the Linux terminal.


.. tip::

    The `online analysis function <https://bio.liclab.net/scvmap/on_line>`_ of scVMAP uses this script command.

1.2.3.3 Create formatted directories
"""""""""""""""""""""""""""""""""""""

Create a root path: ``/project/scVMAP``.

Other path details:

 - ``/project/scVMAP/code``: Path for the ``integration.R`` and ``run.R`` files.
 - ``/project/scVMAP/result``: Path for saving intermediate files and result files.
 - ``/project/scVMAP/scATAC``: This path stores the input scATAC-seq data in RDS format. File example: `scATAC-seq-example <https://bio.liclab.net/scvmap_static/download/example/GSE139369_ELM_sim_0.6_ATAC.rds>`_.
 - ``/project/scVMAP/variant``: This path stores the input phenotype data in BED or TXT format. File example: `Trait-example <https://bio.liclab.net/scvmap_static/download/variant/hg19/BBJ_Mono_55.bed>`_.

1.2.3.4 Execute command
"""""""""""""""""""""""""""""""""""""

The command is as follows: ::

    /bin/Rscript $basePath/code/run.R $basePath $identifier $scFile $variantFile $genome $layer

For example: ::

    /bin/Rscript /project/scVMAP/code/run.R /project/scVMAP 7627190552 26286db074_GSE139369_ELM_sim_0.7_ATAC.rds 4880d7db5c_BBJ_RBC_64.bed hg19 counts

Parameter description:

.. py:function:: core_process($basePath, $identifier, $scFile, $variantFile, $genome, $layer);

  Command-line Parameter Description

  :param string $basePath: The root path for processing.
  :param string $identifier: A unique ID used as the filename to save the results.
  :param string $scFile: The filename of the scATAC-seq data.
  :param string $variantFile: The filename of the trait file.
  :param string $genome: The reference genome (hg19 or hg38).
  :param string $layer: The name of the layer for the counts matrix in the RDS file, e.g., "counts".
  :rtype: None


1.3 Differential activity genes and TFs of cell types in single-cell samples
----------------------------------------------------------------------------

We used SnapATAC2 to calculate gene activity data for single cells. Then, we calculated differentially active genes across cell types using SCANPY.

Here is the SnapATAC2 code content:

.. code-block:: python
    # Create the cell by gene activity matrix
    # The `adata` represents the result preprocessed by SnapATAC2.
    selected_list = np.array(list(adata.var["selected"]))
    selected_adata = adata[:, selected_list]
    gene_matrix = snap.pp.make_gene_matrix(selected_adata, genome_anno)

    # normalize
    sc.pp.filter_genes(gene_matrix, min_cells=min_cells)
    sc.pp.normalize_total(gene_matrix)
    sc.pp.log1p(gene_matrix)
    sc.external.pp.magic(gene_matrix, solver="approximate")


Here is the SCANPY code content:

.. code-block:: python

    def get_difference_genes(
        adata: AnnData,
        cluster: str,
        method: _Method = "wilcoxon",
        cell_anno: Optional[DataFrame] = None,
        diff_genes_file: Optional[str] = None
    ) -> AnnData:

        import scanpy as sc

        # add cell annotation information
        adata.obs = add_cluster_info(adata.obs, cell_anno, cluster)

        if "log1p" not in adata.uns_keys():
            ul.log(__name__).info("The `log1p` not detected in `adata.uns_keys`, `log1p` operation needs to be performed.")
            raise ValueError("The `log1p` not detected in `adata.uns_keys`, `log1p` operation needs to be performed.")

        if "base" not in adata.uns["log1p"].keys():
            adata.uns["log1p"].update({"base": None})

        # gene
        ul.log(__name__).info("Rank genes for characterizing groups.")
        with warnings.catch_warnings():
            warnings.simplefilter("ignore")
            sc.tl.rank_genes_groups(adata=adata, groupby=cluster, method=method, use_raw=False)

        # get difference genes for each `cluster`
        diff_genes = adata.uns['rank_genes_groups']['names']

        # gene names
        gene_list: list = list(adata.var.index)
        gene_list.sort()
        gene_dict: dict = dict(zip(gene_list, range(len(gene_list))))

        # obs
        cluster_info: DataFrame = adata.obs.copy().groupby([cluster], as_index=False).size()
        cluster_info.index = cluster_info[cluster].astype(str)

        cluster_list: list = cluster_info.index.tolist()
        cluster_list.sort()

        _shape_ = (adata.shape[1], cluster_info.shape[0])
        diff_genes_score_matrix: matrix_data = np.zeros(_shape_)
        diff_genes_p_value_matrix: matrix_data = np.zeros(_shape_)
        diff_genes_adjusted_p_value_matrix: matrix_data = np.zeros(_shape_)
        diff_genes_log2_fold_change_matrix: matrix_data = np.zeros(_shape_)
        del _shape_

        # cluster
        for _cluster_ in cluster_list:
            ul.log(__name__).info(f"Obtaining differentially expressed genes for `cluster` ({_cluster_}).")
            # obtain cluster difference gene data
            _cluster_data_: DataFrame = sc.get.rank_genes_groups_df(adata, group=_cluster_)
            _cluster_index_: int = cluster_list.index(_cluster_)

            # Add data value
            for _gene_name_, _score_, _p_value_, _adjusted_p_value_, _log2_fold_change_ in zip(
                _cluster_data_["names"],
                _cluster_data_["scores"],
                _cluster_data_["pvals"],
                _cluster_data_["pvals_adj"],
                _cluster_data_["logfoldchanges"]
            ):
                _gene_index_: int = gene_dict[_gene_name_]
                diff_genes_score_matrix[_gene_index_, _cluster_index_] = 0 if np.isnan(_score_) else _score_
                diff_genes_p_value_matrix[_gene_index_, _cluster_index_] = 1 if np.isnan(_p_value_) else _p_value_
                diff_genes_adjusted_p_value_matrix[_gene_index_, _cluster_index_] = 1 if np.isnan(_adjusted_p_value_) else _adjusted_p_value_
                diff_genes_log2_fold_change_matrix[_gene_index_, _cluster_index_] = 0 if np.isnan(_log2_fold_change_) else _log2_fold_change_

            del _cluster_data_, _cluster_index_

        set_inf_value(diff_genes_score_matrix)
        set_inf_value(diff_genes_p_value_matrix)
        set_inf_value(diff_genes_adjusted_p_value_matrix)
        set_inf_value(diff_genes_log2_fold_change_matrix)

        diff_genes_p_value_matrix[diff_genes_p_value_matrix == 0] = np.min(diff_genes_p_value_matrix[diff_genes_p_value_matrix != 0])
        diff_genes_adjusted_p_value_matrix[diff_genes_adjusted_p_value_matrix == 0] = np.min(diff_genes_adjusted_p_value_matrix[diff_genes_adjusted_p_value_matrix != 0])

        # create
        diff_genes_adata: AnnData = AnnData(diff_genes_score_matrix, obs=adata.var, var=cluster_info)
        diff_genes_adata.layers["p_value"] = diff_genes_p_value_matrix
        diff_genes_adata.layers["adjusted_p_value"] = diff_genes_adjusted_p_value_matrix
        diff_genes_adata.layers["log2_fold_change"] = diff_genes_log2_fold_change_matrix

        # Add diff_genes
        diff_genes_adata.uns["diff_genes"] = diff_genes

        if diff_genes_file is not None:
            save_h5ad(diff_genes_adata, diff_genes_file)

        return diff_genes_adata


Please see `scVMAP-reproducibility-SnapATAC2 <https://github.com/YuZhengM/scvmap_reproducibility/tree/main/scATAC/SnapATAC2>`_ for the detailed workflow code.


