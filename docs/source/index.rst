.. role:: text_red
   :class: red

Welcome to SCVdb's documentation!
==================================


Genome-wide association studies in combination with single-cell genomic atlases
can provide insights into the mechanisms of genetic variation.
Fine-mapping refines this to pinpoint causal variants and more precisely decode
disease mechanisms. The advent of single-cell ATAC sequencing, which employs
co-localization and random walk techniques alongside fine-mapping, allows for
the regulation and tagging of cell types with genetic variants, elucidating
their role in cellular contexts. Some algorithms like g-chromVAR and SCAVENGE
have been developed that effectively elucidate the roles and impacts of genetic
variations at the single-cell level. However, they face limitations like less
user-friendly analysis workflows, time-intensive, and reliance on high-performance
computing. These challenges hinder biologists from widely investigating the
single-cell genetic variations, urgently calling for a user-friendly online
analysis tool. Thus, we developed `SCVdb <https://bio.liclab.net/scvdb/>`_, the first online and user-friendly
server for variant-to-function mapping at single-cell resolution.


Check out the :doc:`usage` section for further information, including
how to :ref:`installation` the project.

.. note::

   This project is under active development.

Contents
--------

.. toctree::
    :maxdepth: 3

   available
   structure
   usage
   implement
