2. Analysis process and plat structure
======================================



The current version of SCVdb records ``183`` scATAC-seq data and ``15,805``
fine-mapping result data. The scATAC-seq samples were organized and
manually classified by scATAC-Ref, scBlood and others. By using a
unified system environment and software for unified process preprocessing,
causal variant data is collected from sources ``CAUSALdb``, ``FinnGen``, ``UKBB``,
and ``BBJ`` and processed and screened according to unified principles. SCVdb
provides a user-friendly interface for analyzing, querying, browsing, and
downloading pages, as well as their related annotation information. For
more detailed statistical data, please refer to the "`Statistics <https://bio.liclab.net/scvdb/statistics>`_" page.

The analysis results mainly include:

1. Enrichment heatmap of different cell types in traits or diseases in a single-cell sample.
#. Scatter plot and box plot of cells of interest for a certain trait or disease in the sample.
#. The differential gene results and the bubble plot of gene enrichment in a single-cell sample.
#. Results of gene enrichment through MAGMA for causal variant effects.
#. Regulating a network from a trait to a single-cell sample through genes as a hub.
#. Differential transcription factor (TF) data in a single-cell sample.
#. Identifying TFs related to a trait or disease through HOMER.
#. Regulating a network from a trait to a single-cell sample through TFs as a hub.

.. image::img/structure/structure.png
