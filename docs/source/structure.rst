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

    1. Single-cell samples:
    1.1 Differential gene activity analysis of cell types
    1.2 Differential TF activity analysis of cell types
    1.3 High-scoring gene pathway enrichment analysis of cell types

    2. Traits or diseases:
    2.2 LD-based MAGMA analysis (Trait-relevant genes)
    2.3 HOMER motif enrichment analysis (Trait-relevant TFs)
    2.4 MAGMA-based trait-relevant gene pathway enrichment analysis

    3. Integrated analysis:
    3.1 ATAC-based Cicero analysis (Trait-relevant genes)
    3.2 ATAC-based GimmeMotifs analysis (Trait-relevant genes)
    3.4 Gene hub regulatory network analysis
    3.5 TF hub regulatory network analysis
    3.6 Integrated regulatory network analysis

The `scVMAP <https://bio.liclab.net/scvmap/>`_ database primarily provides the following functions: ``Data-browse``, ``Search``, ``Analysis``, ``Statistics``, ``Download``, ``Contact``, and ``Online analysis``.
These functionalities streamline the process of studying single-cell genetic regulation, enabling users to easily explore and interpret biological mechanisms in specific contexts.

In conclusion, (scVMAP, https://bio.liclab.net/scvmap/) is a user-centric database that facilitates the exploration and interpretation of single-cell genetic data through structured displays and dynamic visualizations. It offers intuitive workflows, customizable parameters and comprehensive data accessibility.

.. image:: ./img/overview.png

