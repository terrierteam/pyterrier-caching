Caching General Key/Value Outputs
====================================

:class:`~pyterrier_caching.KeyValueCache` is a general-purpose cache for
:class:`~pyterrier.Transformer` outputs. It stores values based on one or more key columns,
and reuses cached values whenever those keys are encountered again.

**Example use case:** I have an expensive transformer that produces features from ``query`` and
``docno``, and I want to reuse those feature values across multiple experiments.

You use a ``KeyValueCache`` in place of the transformer in a pipeline. It holds a reference to
the transformer so that it can compute values that are missing from the cache.

.. warning::

   **Important Caveats**:

   - The cache key is based only on the columns listed in ``key``.
     If other columns affect outputs, they must be included in ``key`` too.
   - For a new cache, ``key`` and ``value`` must be provided when constructing the cache.
   - If a key is missing and no backing transformer is provided, a ``LookupError`` is raised.
   - A ``KeyValueCache`` represents the combination of a transformer and data distribution.
     Reusing the same cache with a different transformer can produce invalid results.

Example 1: Cache feature values by ``query`` and ``docno``
-----------------------------------------------------------

.. code-block:: python
   :caption: Cache a feature value with :class:`~pyterrier_caching.KeyValueCache`

   import pyterrier as pt
   from pyterrier_caching import KeyValueCache

   feature_transformer = pt.apply.generic(lambda df: df.assign(feature=1.0))
   cached_features = KeyValueCache(
       'path/to/cache',
       feature_transformer,
       key=['query', 'docno'],
       value=['feature'],
   )

   # First call computes and stores feature values
   cached_features(topics_and_results_df)

   # Second call reuses cached values for seen (query, docno) pairs
   cached_features(topics_and_results_df)

Example 2: Cache multiple outputs per key
------------------------------------------

.. code-block:: python
   :caption: Cache multiple feature columns with :class:`~pyterrier_caching.KeyValueCache`

   import pyterrier as pt
   from pyterrier_caching import KeyValueCache

   feature_builder = pt.apply.generic(lambda df: df.assign(
       qlen=df['query'].str.len(),
       has_question_word=df['query'].str.contains(r'\b(which|how|why)\b', case=False).astype(int),
   ))

   cached_features = KeyValueCache(
       'path/to/feature-cache',
       feature_builder,
       key=['query', 'docno'],
       value=['qlen', 'has_question_word'],
   )

   # Works as a normal transformer, but only computes misses
   features_df = cached_features(topics_and_results_df)


API Documentation
--------------------------

.. autoclass:: pyterrier_caching.KeyValueCache
   :members:

.. autoclass:: pyterrier_caching.Sqlite3KeyValueCache
   :members:
