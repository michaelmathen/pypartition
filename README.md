# pypartition
Cuttings and partition tree code for generating small samples for halfspaces.
This is code from the paper "Practical Low Dimensional Half Space Range Space Sampling". It can be used to generate partition trees like the ones from Chan's "Optimal Partition Trees". The main file is partitions.py. The Chan partitioning methods return an actual tree object which can be queried. See the below example that visualizes a partitioning over a random set of points:
```
    import matplotlib.pyplot as plt
    import matplotlib
    pts = [(random.random(), random.random()) for i in range(100000)]
    tree = chan_partitions2(pts, b=28, min_cell_size=100)
    f, ax = plt.subplots()
    s_pts = random.sample(pts, 10000)
    x, y = zip(*pts)
    ax.scatter(x, y, marker='.')
    tree.visualize_arrangement(ax, min(x), max(x), min(y), max(y))
    plt.show()
```
The Matuesek methods return a sample instead where each point is paired with a weight. For instance the below line of code will 
generate a tree with a branch factor of 16 until each node contains approximately 100 points. It will then return a sample of one point from each partition with a corresponding weight of the points in that cell.
```
sample, weights = partitions(pts, 16, min_cell_size=100)
```
Cutting code is in polytree.py. The below code for instance will generate and visualize a polygon cutting with each polygon having at most 6 sides and each polygon crossed by at most 1000/5 lines. Lines can have variable weight, but in this example each line has weight 1.
```
pts = [(random.random(), random.random()) for i in range(1000)]
import test_set
import matplotlib.pyplot as plt
import matplotlib
k = 6
lines = test_set.test_set_lines(pts, 25)
tree = PolyTree2(pts, lines, k = k)
tree.cutting_r(5, {l:1 for l in lines})
f, ax = plt.subplots()
tree.visualize_arrangement(ax, -1, 2, -1, 2)
ax.set_xlim(0, 1)
ax.set_ylim(0, 1)
plt.show()
 ```
