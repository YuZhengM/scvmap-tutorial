2.5.2 Analyzing variant-to-function mapping with genes
=======================================================

 | Link: https://bio.liclab.net/scvmap/analysis

Users submit genes of interest and set thresholds related to differential genes in scATAC-seq samples, as well as thresholds for traits or diseases enriched in genes through MAGMA.

.. image:: ../../img/analysis/analysis_gene.png

The returned result consists of five parts, each corresponding to its own abbreviation. The last four are included on the first analysis results page.

Click the abbreviation button to assign it to the corresponding position.

On the left is the scATAC-seq sample information contained in the strategy genes among the differential genes.

On the right is the trait or disease information contained in the strategy genes in the enriched genes.

.. note::

    All traits or disease data related to data genes may not necessarily have an enrichment relationship with the selected single-cell sample on the left. The **MetaData** option allows users to check if an enriched trait table exists.

.. image:: ../../img/analysis/analysis_gene_result_sample_trait.png

When clicking on different samples above, the following four panels also change accordingly, which is the same as the content of "Enriched cells", "Differential genes", "Genes with enriched trait", and "Gene hub network" on the detailed information page.

Help document for users to view detail pages: `https://scvmap.readthedocs.io/en/latest/usage/detail.html <https://scvmap.readthedocs.io/en/latest/usage/detail.html>`_

.. image:: ../../img/analysis/analysis_gene_result.png
