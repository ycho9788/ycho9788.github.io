---
layout: single
title: "Quicksort Algorithm Experiment"
categories: Quicksort
toc: true
---

1. Deterministic Quicksort
Implemented the Deterministic Quicksort algorithm. A “driver” program that reads a sequence of
numbers form a file, loads them into a list, calls Quicksort to sort the array, and keeps track of the time
Quicksort takes to sort the array.



For each n = 1000, 2000, 3000, . . . , 10000, determined the elapsed time required by Quicksort to sort
the sequence n, n −1, n −2, . . . , 1. Let T (n) be the running time for a sequence of length n. This code shows two
plots: (i) T (n) as a function of n and (ii) T (n)/n2as a function of n.


For each n = 1000, 2000, 3000, . . . , 10000, determined the elapsed time required by Quicksort to sort
and “almost sorted” sequence of n numbers. Here is what’s meant by “almost sorted”. Start with the
sequence 1, 2, 3, . . . , n. this code randomly picks 10 pairs of elements and exchange them. Show the same plots
as in part (a).



For each n = 1000, 2000, 3000, . . . , 10000, determined the elapsed time required by Quicksort to sort a
random permutation of 1, 2, 3, . . . , n. Because of the randomness involved, to get accurate results,
it should repeat the experiment for each n multiple time and use the average results. 
This code repeats the experiment 10 times and use the average time over these 10 runs. Let T (n) be the running
time for a sequence of length n and shows two plots: (i) T (n) as a function of n and (ii) T (n)/(n log2n)
as a function of n.



2. Median-of-Three Quicksort
Replaced Deterministic Quicksort with Median-of-Three Quicksort and re-run the experiments from 1st Deterministic Quicksort.


3. Randomized Quicksort
Replaced Deterministic Quicksort with Randomized Quicksort and re-run the experiments from 1st Deterministic Quicksort.

```python
import random
from timeit import default_timer as timer
import numpy as np
import sys


def partitionDeterministic(low, high, array):
    # Initializing pivot's index to start
    pivot_index = low
    pivot = array[pivot_index]

    # This loop runs till start pointer crosses
    # end pointer, and when it does we swap the
    # pivot with element on end pointer
    while low < high:

        # Increment the start pointer till it finds an
        # element greater than  pivot
        while low < len(array) and array[low] <= pivot:
            low += 1

        # Decrement the end pointer till it finds an
        # element less than pivot
        while array[high] > pivot:
            high -= 1

        # If start and end have not crossed each other,
        # swap the numbers on start and end
        if low < high:
            array[low], array[high] = array[high], array[low]

    # Swap pivot element with element on end pointer.
    # This puts pivot on its correct sorted place.
    array[high], array[pivot_index] = array[pivot_index], array[high]

    # Returning end pointer to divide the array into 2
    return high


# The main function that implements Deterministic QuickSort
def quick_sort(low, high, array):
    if low < high:
        # p is partitioning index, array[p]
        # is at right place
        p = partitionDeterministic(low, high, array)

        # Sort elements before partition
        # and after partition
        quick_sort(low, p - 1, array)
        quick_sort(p + 1, high, array)

# partition for median of three quicksort
def partition(array, lowest, highest):
    pivot = median_of_three(array, lowest, highest)
    x = lowest - 1
    y = highest + 1
    while True:
        x += 1
        while array[x] < pivot:
            x += 1
        y -= 1
        while array[y] > pivot:
            y -= 1
        if x >= y:
            return y
        array[x], array[y] = array[y], array[y]


def median_of_three(array, lowest, highest):
    middle = (lowest + highest - 1) // 2
    if (array[lowest] - array[middle]) * (array[highest] - array[lowest]) >= 0:
        return array[lowest]
    elif (array[middle] - array[lowest]) * (array[highest] - array[middle]) >= 0:
        return array[middle]
    else:
        return array[highest]


# quicksort for median of three
def quickSort(array, lowest, highest):
    if highest is None:
        highest = len(array) - 1

    if lowest < highest:
        p = partition(array, lowest, highest)
        quickSort(array, lowest, p)
        quickSort(array, p + 1, highest)


# randomized quicksort
def quickSortRandom(array, low, high):
    if low < high:
        pivotPoint = partitionRandom(array, low, high)
        quickSortRandom(array, low, pivotPoint - 1)
        quickSortRandom(array, pivotPoint + 1, high)


# partition for randomized quicksort
def partitionRandom(array, low, high):
    r = random.randrange(low, high)

    array[low], array[r] = array[r], array[low]

    pivotItem = array[low]

    j = low
    for i in range(low + 1, high + 1):
        if array[i] < pivotItem:
            j = j + 1
            array[i], array[j] = array[j], array[i]

    pivotPoint = j
    array[low], array[pivotPoint] = array[pivotPoint], array[low]
    return pivotPoint


if __name__ == "__main__":
    sys.setrecursionlimit(10 ** 8)
    # experiments 1
    time = []
    time1 = []
    time6 = []
    for i in range(1, 11):
        n = 1000 * i
        array1 = []
        for j in range(n, 0, -1):
            array1.append(j)

        start = timer()
        quickSortRandom(array1, 0, len(array1) - 1)
        end = timer()
        time.append((end - start) * 1000000)  # microseconds

        start = timer()
        quickSort(array1, 0, len(array1) - 1)
        end = timer()
        time1.append((end - start) * 1000000)

        start = timer()
        quick_sort(0, len(array1) - 1, array1)
        end = timer()
        time6.append((end - start) * 1000000)

    # experiments 2
    time2 = []
    time4 = []
    time7 = []
    i = 0
    j = 0
    for i in range(1, 11):
        n = 1000 * i
        array1 = []
        for j in range(1, n + 1):
            array1.append(j)

        # swap ten pairs
        for k in range(0, 11):
            r1 = random.randrange(0, n + 1)
            r2 = random.randrange(0, n + 1)

            array1[r1], array1[r2] = array1[r2], array1[r1]

        start = timer()
        quickSortRandom(array1, 0, len(array1) - 1)
        end = timer()
        time2.append((end - start) * 1000000)  # microseconds

        start = timer()
        quickSort(array1, 0, len(array1) - 1)
        end = timer()
        time4.append((end - start) * 1000000)

        start = timer()
        quick_sort(0, len(array1) - 1, array1)
        end = timer()
        time7.append((end - start) * 1000000)

    # experiments 3
    time3 = []
    time5 = []
    time8 = []
    i = 0
    j = 0
    for i in range(1, 11):
        sumTime = 0
        sumTime2 = 0
        sumTime3 = 0
        n = 1000 * i
        for j in range(0, 11):
            array1 = []

            for j in range(1, n + 1):
                array1.append(j)

            array2 = np.random.permutation(array1)
            start = timer()
            quickSortRandom(array2, 0, len(array2) - 1)
            end = timer()
            sumTime = sumTime + ((end - start) * 1000000)

            start = timer()
            quickSort(array1, 0, len(array1) - 1)
            end = timer()
            sumTime2 = sumTime2 + ((end - start) * 1000000)

            start = timer()
            quick_sort(0, len(array1) - 1, array1)
            end = timer()
            sumTime3 = sumTime3 + ((end - start) * 1000000)

        time3.append(sumTime / 10)
        time5.append(sumTime2 / 10)
        time8.append(sumTime3 / 10)

    print("Random quicksort experiments:")

    print("Experiment 1: ", time)
    print("Experiment 2: ", time2)
    print("Experiment 3: ", time3)

    print("Median of three experiments:")
    print("Experiment 1: ", time1)
    print("Experiment 2: ", time4)
    print("Experiment 3: ", time5)

    print("Deterministic quicksort experiments:")

    print("Experiment 1: ", time6)
    print("Experiment 2: ", time7)
    print("Experiment 3: ", time8)
```

[Summary]
Interpreted the plots from the experiments. The main questions is that if the results confirm theoretical predictions or not.

 [Quicksort_Experiment_summary.pdf](C:\Users\user\Desktop\github 자료 올리기\Quicksort_Experiment_summary.pdf) 

The plots all confirmed theoretical predictions. The plots that showed T(n)/n^2 as a function of n ended up looking similarly to an inverse of the plots with T(n) as a function of n. The plots with T(n)/nlogn were much different than the T(n) plots as they represent a logarithmic function. For the Deterministic Quicksort, the function would only run up to n = 3000 for Experiment 1 and n = 8000 for Experiment 2. As opposed to n =10000 for every other experiment. All data is in  microseconds.
