---
# 18 nov 2023
layout: page
title: Sudoku Solver
description: Program designed to solve Sudoku puzzles using the AC-3 constraint satisfaction algorithm
img: assets/img/sudoku.jpg
importance: 2
category: school
related_publications: true
---

Welcome to the Sudoku Solver project! Ever stared at a Sudoku grid and felt your neurons overheating? This program tackles that for you using the magical-sounding AC-3 {% cite mackworth1977consistency %} constraint satisfaction algorithm. Choose from three different Sudoku setups—if you find yourself stumped, just hit "solve" and let logic (and a little code) do the heavy lifting. Bragging rights for human vs. machine are optional. To see the magic under the hood, checkout the code in my [repo](https://github.com/AndrejPer/hidato-choco).

<iframe
src="{{ '/assets/html/sudoku.html' | relative_url}}" 
width="100%" 
height="570px" 
frameborder="0">
</iframe>

### Sudoku revisited

If javascript is not your favourite programming language, here is a Sudoku solver implemented in Python usinf [toulbar2](https://github.com/toulbar2/toulbar2/tree/master) solver.

```python
import pytoulbar2 as tb2

N = 9
top = N**4 + 1

Problem = tb2.CFN(top)

# adding variables and the domain
for i in range(N):
    for j in range(N):
        Problem.AddVariable('cell_' + str(i) + str(j), range(1, 10))

# Different values in row
for i in range (N**2):
    row = i // N + 1
    # for each cell in the same row
    for j in range(i + 1, N * row):
        # now we make a diagonal matrix preventing assignment 
        # of the same value to cells in the same row
        ListConstraintsRow = []
        # for each value in domain of Y_i
        for a in range(N):
            # for each value in domain of Y_j
            for b in range(N):
                if a != b:
                    ListConstraintsRow.append(0)
                else:
                    # penalize assigning the same value
                    ListConstraintsRow.append(top)
        Problem.AddFunction([i, j], ListConstraintsRow)

# Different values in columns
for i in range(N ** 2):
    # for each cell in the same column
    for j in range(i + N, N ** 2, N):
        ListConstraintsCol = []
        # for each value in domain of Y_i
        for a in range(N):
            # for each value in domain of Y_j
            for b in range(N):
                if a != b:
                    ListConstraintsCol.append(0)
                else:
                    # penalize assigning the same value
                    ListConstraintsCol.append(top)
        Problem.AddFunction([i, j], ListConstraintsCol)

# Different values in square
for i in range(0, N**2, 3): # TODO change 3's into sqrt(N)
    row = i // N
    col = i % N
    # print("i, j", row, col)

    boxI = (row // 3)
    boxJ = (col // 3)
    # print("BOX I J ", boxI, boxJ)
    indices_I = list(range(boxI * 3, (boxI + 1) * 3))
    # print("is", indices_I)
    indices_J = list(range(boxJ * 3, (boxJ + 1) * 3))
    # print("js", indices_J)

    # extract indices of each box
    box_indices = []
    for idx_I in indices_I:
        for idx_J in indices_J:
            box_indices.append(idx_I * N + idx_J)

    # impose pair-wise constraints
    n = len(box_indices)
    # for each pair
    for k in range(n - 1): # for each index in the box (accessed as box_indices[k])
        for l in range(k + 1, n):
            # now make the cost matrix
            ListConstraintsBox = []
            # for each value, penalize identical assignment
            for a in range(N):
                for b in range(N):
                    if a != b: 
                        ListConstraintsBox.append(0)
                    else: 
                        ListConstraintsBox.append(top)
            Problem.AddFunction([box_indices[k], box_indices[l]], ListConstraintsBox)

game = list("050720000060000090008000000400030800000005060900000000300000002000006300000900000")
# Add hints as unary constraints
assert len(game) == N**2
for i in range(N**2):
    cell = int(game[i])
    if cell > 0:
        # impose constraint for the value
        ListConstraintsHint = []
        for a in range(N):
            if a != cell - 1:
                ListConstraintsHint.append(top)
            else:
                ListConstraintsHint.append(0)
        Problem.AddFunction([i], ListConstraintsHint)


# Solving
Problem.CFN.timer(300)
print("Number of constraints:", Problem.GetNbConstrs())
res = Problem.Solve()
print(res)
if res:
	for i in range(N):
		print(res[0][i * N:(i + 1) * N])
	# and its cost
	print("Cost:", int(res[1]))
```