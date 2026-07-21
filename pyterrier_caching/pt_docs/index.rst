Caching for PyTerrier
=====================================

`pyterrier-caching <https://github.com/seanmacavaney/pyterrier-caching>`__ provides several
PyTerrier components for caching intermediate results.

You can install ``pyterrier-caching`` with pip:

.. code-block:: console
   :caption: Install ``pyterrier-caching``

   $ pip install pyterrier-caching

The right component (:class:`~pyterrier_caching.ScorerCache`, :class:`~pyterrier_caching.IndexerCache`,
:class:`~pyterrier_caching.RetrieverCache`, or :class:`~pyterrier_caching.KeyValueCache`) will depend on
your use case. More information can be
found in the subsequent pages:

.. toctree::
   :maxdepth: 1

   scorer-cache
   indexer-cache
   retriever-cache
   key-value-cache
   extras

For more information about caching in PyTerrier, see our WOWS 2025 paper:

.. cite.dblp:: journals/corr/abs-2504-09984
