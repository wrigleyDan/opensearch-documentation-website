---
layout: default
title: Judgments for Search
nav_order: 65
parent: Search relevance
has_children: false
has_toc: false
---

# Judgments

Clicks Over Expected Clicks (COEC) is one specific click model that is implemented in the following way in the Search Relevance Workbench.
The data used to derive relevance labels is based on past user behavior following the User Behavior Insights schema specification. The two types of interactions are impressions and clicks that happen after a user query. Technically, this means that all events in the index ubi_events are used to model implicit judgments that either have impression or click as values indexed in the field action_name.
What Clicks Over Expected Clicks models is an expected value that assumes that every query-document pair has an expected click-through rate (CTR) for a given rank based on all passed impressions and clicks. It is implemented as the sum of all clicks over the sum of all impressions for all events in ubi_events that were observed at a given rank.
Be aware that depending on the implementation details of the tracking this may mean that for one query multiple clicks can be stored in the ubi_events index. This means that in theory an average click-through rate greater than 1 (=100%) is possible. In practice this is not likely to happen as search sessions usually do not have multiple clicks.

The average click-through rate across all events per rank is the expected CTR for a given query–doc pair at a certain position. COEC divides the observed CTR by the expected rank-aggregated CTR value. This means that those query-document pairs that have a better (higher) click-through rate than the average CTR at the rank of this query-document pair were observed and the judgment will be greater than 1. If it’s less than the average then the judgment value will drop below 1.

For query-document observations that occur on different positions we assume all observations (impressions and clicks) all happened at the lowest (=best) position. This means that we bias the final judgment towards lower values as we assume higher click-through rates for lower ranks.
