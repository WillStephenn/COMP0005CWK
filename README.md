# Experimental Analysis of Self-Balancing Search Trees

This project is an experimental framework, developed for the COMP0005 Algorithms group coursework, to analyse and compare the performance of three theoretically equivalent O(log n) search data structures: **Left-Leaning Red-Black (LLRB) Trees**, **Scapegoat Trees**, and **B-Trees**.

The framework is implemented in Python within a Jupyter Notebook and uses Matplotlib for data visualisation. It's designed to test these data structures under various conditions to reveal how their theoretical performance translates to real-world scenarios.

---

## Experimental Framework Design

The framework is built on four core components to ensure a robust and repeatable analysis:

* **Initialisation**: Allows for the parameterised instantiation of each tree. This includes setting the **alpha value** for Scapegoat Trees and the **branching factor** for B-Trees, enabling detailed comparisons of jejich internal mechanisms.

* **Data Generation**: To rigorously test the balancing mechanisms, four distinct types of sample data were generated:
    * **Random Samples**: To evaluate average-case performance.
    * **Sorted Samples**: To create a worst-case scenario for insertion, challenging the rebalancing of LLRB Trees and the node-splitting logic of B-Trees.
    * **Balanced Samples**: To provide a near-optimal insertion sequence as a baseline.
    * **Unbalanced Samples**: To specifically target and test the rebalancing thresholds.

* **Data Collection & Processing**: The framework captures performance metrics for both **insert** and **search** operations across 10,000 data points. To reduce noise from system-level fluctuations, the data is processed using a windowed moving average. Additionally, it includes a metric for theoretical **I/O operations** to compare performance in disk-based scenarios.

* **Visualisation**: A flexible plotting module built with Matplotlib allows for the rapid generation of comparison charts, including individual data points and moving averages to show underlying trends.

---

## Summary of Key Findings & Analysis

### LLRB Tree Performance

The LLRB Tree's insertion performance closely followed the expected **O(log n)** logarithmic curve. However, search performance was observed to be nearly **constant time**, rather than logarithmic. This suggests that for the tested data size, the search time is dominated by constant factors, likely due to the high efficiency of cache and branch prediction in modern CPUs, which makes the cost of traversing the shallow tree negligible.

![LLRB Inserts Graph](Plots/mAvg/LLRBBST%20Tree%2C%20All%20Sample%20Variants%2C%20Inserts.png)
![LLRBBST Tree Searches Graph](Plots/mAvg/LLRBBST%20Tree%2C%20All%20Sample%20Variants%2C%20Searches.png)

### Scapegoat Tree Parameter Tuning

Experiments on the Scapegoat Tree's **alpha** parameter revealed that higher values (0.8-0.9) led to better performance for both insertions and searches. This is because a higher alpha tolerates more imbalance, which reduces the frequency of costly subtree rebuilding operations. This finding suggests that for many workloads, less frequent but larger rebalancing events are more efficient than constant, smaller adjustments.

![Scapegoat Tree Alpha Comparison Searches Graph](Plots/mAvg/Scapegoat%20Tree%20Alpha%20Comparison%2C%20Random%20Samples%2C%20Searches.png)
![Scapegoat Tree Alpha Comparison Inserts Graph](Plots/mAvg/Scapegoat%20Tree%20Alpha%20Comparison%2C%20Random%20Samples%2C%20Inserts.png)

### B-Tree Branching Factor and I/O Performance

For in-memory operations, B-Trees with smaller branching factors performed better, as the linear search within each node is faster with fewer keys. While B-Tree searches were slower than the binary trees, their key strength was revealed in the theoretical I/O analysis. Due to their shallow height, B-Trees required an order of magnitude fewer disk I/O operations, making them vastly superior for datasets larger than available memory.

![BTree Branching Factor Comparison Inserts Graph](Plots/mAvg/BTree%20Branching%20Factor%20Comparison%2C%20Random%20Samples%2C%20Inserts.png)
![BTree Branching Factor Comparison Searches Graph](Plots/mAvg/BTree%20Branching%20Factor%20Comparison%2C%20Random%20Samples%2C%20Searches.png)
---

## Limitations & Future Work (Incorporating Feedback)

While the experimental framework provided valuable insights, the project had several limitations that offer clear directions for future work, based on the feedback received:

* **Scale of Experiments**: The tests were conducted on datasets of up to 10,000 elements. To properly investigate the **asymptotic behavior** of these algorithms, the dataset size should be increased by at least two orders of magnitude (e.g., to 1 million elements). This would reduce the impact of constant factors and provide a clearer view of the logarithmic growth.

* **Data Variety**: The analysis was limited to strings of a fixed length (5). Future experiments should analyze the impact of **varying string lengths** and datasets with **significant substring overlap**. This would test the algorithms' performance on more "dense" and realistic string data.

* **Statistical Rigor**: The current analysis relies on a moving average to smooth out variance. A more robust methodology would involve repeating each experiment multiple times with different random seeds. This would allow for the calculation of **means and standard deviations**, which could be visualized using boxplots to provide a clearer understanding of performance consistency and make more informed algorithmic choices.

* **Deeper Performance Analysis**: The observation that search times were nearly constant highlights the influence of "hidden constants" in Big O notation. Future work could involve **fitting regression lines** to the performance curves to empirically estimate these constants and build a more precise performance model.

---

## How to Run the Experiments

The entire framework, including the data structure implementations and plotting code, is contained within the `grp-29-0005code.ipynb` Jupyter Notebook.

1.  **Ensure you have a Python environment** with Jupyter, Matplotlib, and other standard libraries installed.
2.  **Launch Jupyter Notebook** and open the `grp-29-0005code.ipynb` file.
3.  **Run the Tests** click 'Run All' 

## Contributors

* William Stephen
* Nemo Shu
* Mariha Subhan
