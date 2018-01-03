# plan

**core maximization**

- [ ] put them in the doc and email the people about the result
- [ ] hardness proof: search related problem of core-number maximization / resilience maximization (1h)

**active learning**

- [ ] report the result 
- [ ] `inference.py` using the `sample_pool.py` with incremental support (1h)
- [ ] measure the score using the sampling based inference method (0.5h)
- [ ] integrate our order-based steiner into the method (1h)


**presentation**

- [ ] basic structure according to the survey paper (2h) 
  - direct method
  - neighborhood
  - convolution-based (why the name)
  - subgraph embedding
  - open challenges (with my thinking and my own problems)
- [ ] more technical details
- [ ] GraphSage: https://arxiv.org/pdf/1706.02216.pdf (2h)

**label embedding xml**

- [ ] visualize the embedding (1h)
- [ ] search papers on metric learning on bipartite graph? (0.5h)
- [ ] problem and method (1h)
  - what's the training objective of learning the projection matrix using the probabilistic approach?
  - how to do the traning in keras using negative sampling?


# Tuesday

- [X] frog: can we use dynamic programming to solve single node version? (1h)
- [ ] search for common NP-hard problems that are reduced from (1h)
- [X] integrate `random_steiner_tree` to sampling pool (1h)
  - speed comparison
- [ ] https://www.researchgate.net/profile/Minh_Phan22/publication/319259804_NeuPL_Attention-based_Semantic_Matching_and_Pair-Linking_for_Entity_Disambiguation/links/59d7afa9458515bbfee8df7b/NeuPL-Attention-based-Semantic-Matching-and-Pair-Linking-for-Entity-Disambiguation.pdf (1h)

## frog

it seems impossible because getting the gain of the edge being considered requires knowing whether other edges are added.

## speed comparison

`cut` and `loop_erased` is very fast. however, constructing the `GraphView` is costly. 

## integration

why `isolate_disconnected_components`?

# Wednesday

- [ ] frog: search papers on how to prove inapproximability (1 h)
- [ ] improve the plot (node color consistent with core number, easy to interpret the change) (1h)
- [ ] make `test_query_selection` pass why `isolate_disconnected_cc`? 
- [ ] experiment: varying sampling method (fixed query methoad) (1h)
- [ ] experiment: varying query method (fix sampling method) (1h)
- [ ] search for other related inference methods (1h)
- [ ] entity linking paper (LSTM, pair-linking, experiment) (1h)