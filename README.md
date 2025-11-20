# CI2025_lab3

## Overview

This repository contains my solution for **Lab 3** of the course *Computational Intelligence (CI2025/26)*. It consists in finding the **Shortest Path** between two cities, for different instances of the distance matrix.

---

## Problem definition

Given a directed weighted graph represented as an N × N matrix, where:
* matrix[u][v] = weight(u→v)
* infinite weights (inf) represent missing edges
* weights may be positive or negative depending on the chosen configuration
the goal is to compute the shortest path between multiple source–target pairs and verify correctness against the NetworkX implementation.

Graphs are generated with configurable:
* size (number of nodes)
* density (probability of edge creation)
* noise level (scales the initial distribution)
* presence or absence of negative weights

---

## Algorithms implemented

### 1. Dijkstra's algorithm

Custom implementation using a min-heap (heapq) to compute distances from a single source in graphs with non-negative edge weights.
The algorithm returns:
* minimum cost to the target
* the reconstructed path
* (inf, None) if no path exists
I ended up not using it, because A* was much faster, but I still decided to leave it in the notebook.

### 2. A* search algorithm

General A* implementation with two available heuristics:
* Zero heuristic, which reduces A* to Dijkstra
* Euclidean heuristic, computed from the coordinates assigned to each node when generating the problem
A* is used only when all weights are non-negative, otherwise Bellman-Ford is used.

### 3. Bellman-Ford algorithm

Custom implementation supporting:
* shortest-path computation on graphs with negative weights
* detection of negative cycles
* classic relaxation procedure (N−1 rounds)
Returns:
* distance array
* predecessor array (in case anyone wants to recreate the path (in my case, never used))
* boolean flag indicating the presence of negative cycles

---

## Evaluation methodology:

### 1. evaluate_problem function

This function:
1. generates a random graph instance with the chosen parameters
2. builds a corresponding NetworkX graph
3. selects random (source, target) pairs
4. runs the appropriate algorithm (A* or Bellman-Ford)
5. compares the computed distance with:
  * nx.shortest_path_length() (for non-negative graphs)
  * nx.bellman_ford_path_length() (for negative-weight graphs)
6. collects statistics:
  * correct distances (ok)
  * wrong distances (bad)
  * detected negative cycles (neg_count)
  * execution time

The function can be tested with different parameters to observe its behavior on different problems.

### 2. evaluate_all_problems function

This function iterates over all combinations of:
* sizes: [10,20,50,100,200,500,1000]
* densities: [0.2, 0.5, 0.8, 1.0]
* noise levels: [0.0, 0.1, 0.5, 0.8]
* negative values: [False, True]

In the end, it prints the total number of correct distances, wrong distances and detected negative cycles.

---

## Experimental results:

The algorithm seems to work pretty well, always matching the NetworkX's results, except for problems with negative values.
In that case, the algorithm seems to always find negative cycles, making it impossible to compute the shortest path.


---

**Author:** [Riccardo Dattena (helped by ChatGPT for a couple of lines)]  
**Course:** Computational Intelligence (CI2025/26)  
**Lab 3 — Shortest Path**
