Given a cluster index for all images, `clustering`, and image labels, `y`, return a Numpy array of length `k`, such that the `i`-th element of this array indicates the most common label among **labeled images** in the `i`-th cluster. In the case of a tie for the most common label, return the value of the smallest label. If a cluster does not contain any labeled images, assign a label of `-1` to that cluster.

For example, suppose you execute the following.

```python
clustering = np.array([ 8, 5, 5, 8, 6, 7,  4, 1, 8, 7, 5,  8, 8])
y =          np.array([-1, 5, 6, 9, 6, 7, -1, 1, -1, 7, 6, -1, 3])
z = find_max_common_label_in_cluster(clustering, y, 10)
print(z)  # Output will be: [-1  1 -1 -1 -1  6  6  7  3 -1]
```

Consider every element where `clustering == 5`; these are the points assigned to Cluster 5. The corresponding labels, `y[clustering == 5]`, are `[5, 6, 6]`. Thus, the most common label in Cluster 5 is 6. Therefore, `z[5] == 6`.

Next, consider every element where `clustering == 4`. There is just one such point: `y[clustering == 4] == [-1]`, which is an unlabeled point. Therefore, `z[4] == -1`.

Now consider `clustering == 8`. Observe that `y[clustering == 8] == [-1,  9, -1, -1,  3]`. Among the **labeled** points, `3` and `9` occur once each, so they are tied in frequency. In this case, you should return the smallest label, 3.

Lastly, observe that `clustering` contains no instances of 0, 2, 3, or 9. The corresponding outputs in `z` have been set to -1.

### Solution

```Python
def find_max_common_label_in_cluster(clustering, y, k):
    import numpy as np
    z = np.array([-1] * k)
    for i in set(clustering):
        y_indices = np.where(clustering == i)[0]
        y_take = np.take(y, y_indices)
        y_filtered = np.extract(y_take != -1, y_take)
        if y_filtered.size:
            counts = np.bincount(y_filtered)
            z[i] = np.argmax(counts)            
    return z
```
