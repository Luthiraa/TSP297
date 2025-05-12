# A.I.M. - Artificially Intelligent Maps

AIM is a fully-fledged mapping application built with C++ (backend), GTK (C++ library for UI elements), CSS, and the OpenStreetMap library. The goal of this map was to create a software that centialzies and consolidates information into one place to elimiate the need for users to seek inforamtion from external source, and this was done through an LLM powered agent [repo: `https://github.com/Luthiraa/297Agent`].

## Collaborators

- [@k-chhajer](https://github.com/k-chhajer)
- [@gracehao15](https://github.com/gracehao15)

## Overview
This project implements a **delivery routing algorithm** for the ECE297 course milestone using a combination of:
- **Regret Insertion** for initial path construction  
- **Multithreaded Dijkstra** for distance matrix computation  
- **Local Search Improvements** (2-opt, 3-opt, 4-opt)  
- **Shake-based perturbations** to escape local minima  

The goal is to compute the most efficient route for a courier to perform a set of pickup and dropoff tasks starting and ending at any depot.

---

## Algorithm Pipeline

### 1. Input Preprocessing
- Nodes are classified as:
  - `depotType`: Starting/ending points
  - `pickupType` and `dropoffType`: Delivery pairs
- Nodes are indexed: `0..depots`, `depots..depots+pickups`, `dropoffs`

### 2. Distance Matrix Generation
- Uses **multi-threaded Dijkstra** to compute travel times between all nodes
- Turn penalties included
- Results stored in a `dist[i][j]` matrix for fast lookup

### 3. Initial Path Construction
- Applies **regret insertion heuristic**:
  - Greedily insert pickup/dropoff pairs with the highest regret (difference between best and second-best insertion)
  - Chooses best depot pair for start and end based on overall cost

### 4. Route Refinement
- Run **2-opt**, **3-opt**, and **4-opt** moves while ensuring pickup-before-dropoff constraints
- Use **shake2** perturbation to escape local optima by swapping delivery pairs
- Optimize with **multithreaded 3-opt and 4-opt**

### 5. Final Path Construction
- Remove consecutive duplicate intersections
- Construct valid subpaths using `findPathBetweenIntersections()`
- Return result as a list of `CourierSubPath` objects

---

## Pipeline Illustration

![pipeline illustration](/diagram2.png)
---

## Visual Output

![alt text](/qor.gif)

### Optimization Techniques
- **Shake2**: Swaps two delivery requests (pickup & dropoff)
- **Greedy regret insertion**: Builds the initial route intelligently
- **Multithreading**: Improves performance in 3-opt/4-opt search
- **Valid order enforcement**: Maintains delivery legality


