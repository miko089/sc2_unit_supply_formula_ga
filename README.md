# Starcraft II Zerg Unit Supply Formula - Genetic Algorithm

## Project Overview
This project uses a Genetic Algorithm (GA) to find a mathematical formula that approximates the supply cost of Zerg units in Starcraft II based on their internal Unit IDs. The goal is to replace a hardcoded lookup table with a compact formula.

## Problem Definition
- **Input**: Unit ID (integer `x`).
- **Output**: Supply cost (float/integer).
- **Objective**: Minimize the difference between the formula's output and the actual supply cost for all Zerg units.

## Methodology

### Chromosome Representation
Solutions are represented as strings forming mathematical expressions.
- **Alphabet**: `0123456789+-*/|&^%x`
- **Variable**: `x` represents the Unit ID.
- **Example**: `x/2+1`

### Fitness Function
The fitness function quantifies the accuracy of an expression.
$$ Fitness = 5000 - \sum_{units} (Predicted - Actual)^2 \times 25 - Penalties $$
Penalties are applied for:
- Invalid syntax (eval failure)
- Missing variable `x`. (tried to penalize formulas that do not depend on x, but it led to same formulas as x//4 on gen300 and I wanted to make it use x more creatively)
- Lack of operators
- Excessive length (slight penalty to encourage shorter formulas)

### Genetic Operators
- **Selection**: Weighted random selection (Roulette Wheel).
- **Crossover**: Single-point crossover at operator positions to maintain valid syntax probability.
- **Mutation**: Random insertion, deletion, or substitution of characters.

## Experiments
The notebook `main.ipynb` contains experiments running the GA with different configurations:
- Varying population sizes (e.g., 50, 100).
- Varying mutation rates (e.g., 0.3, 0.5, 0.7).

## How to Run
1. Open `main.ipynb` in VS Code or Jupyter.
2. Run all cells.
3. The "Experiments" section will execute the GA with multiple configurations and display a summary table.
4. The final cell will verify the best found formula against the dataset.

## Results
The algorithm typically finds approximations like `x//4` (best yet) or similar simple arithmetic relations, though exact matches for all units are difficult due to the non-linear nature of the ID-to-Supply mapping. Sadly, all formulas found so far have some discrepancies with the actual supply costs, but I haven't managed to find a perfect one. There were funny formulas like `0x3` (hex number) that returned constant supply cost regardless of the unit ID, but they are not useful too.