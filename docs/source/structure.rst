1. Platform construction
==============================================

 | scVMAP: https://bio.liclab.net/scvmap/
 | scVMAP tutorial: https://scvmap-tutorial.readthedocs.io/en/latest/index.html
 | scVMAP front-end: https://github.com/YuZhengM/scvmap_web
 | scVMAP back-end: https://github.com/YuZhengM/scvmap
 | scVMAP API: https://bio.liclab.net/scvmap_service/swagger-ui/index.html

`scVMAP <https://bio.liclab.net/scvmap/>`_ gathers and provides scATAC-seq datasets with
established cell type labels (from scATAC-Ref, GreenleafLab and PlaqView) and cell type
labels annotated by singleR (from scBlood), along with detailed fine-mapping data (from
CAUSALdb2, FinnGen, UKBB and BBJ). scVMAP, by employing the SCAVENGE and g-chromVAR methods,
assigns trait- or disease-related scores at single-cell resolution, uncovering biologically
relevant cell types. Throughout this process, data undergoes standardization, unified
processing and comprehensive analysis, including differential gene expression analysis,
transcription factor activity profiling, pathway enrichment analysis, MAGMA gene enrichment
analysis, HOMER motif association analysis and so on, to ensure data consistency across
various sources. The database is structured into four modules, offering diverse search
methods, analysis tools, and a user-friendly browser interface for data exploration and
full downloads. These functionalities streamline the process of investigating single-cell
genetic regulation, enabling users to effortlessly explore and interpret biological
mechanisms within specific contexts.

In conclusion, (scVMAP, https://bio.liclab.net/scvmap/) is a user-centric database that facilitates the exploration and
interpretation of single-cell genetic data through structured displays and dynamic
visualizations. It offers intuitive workflows, customizable parameters and comprehensive data accessibility.

.. image:: ./img/overview.png

