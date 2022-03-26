---
layout: single
title: "Timing Two Algorithm"
categories: TimeTwoAlgorithm
toc: true
---

For each value of n, ran both algorithms on 50 different lists of length n and record the average
running time over all 50 runs.
The program measures the “running time” in two different ways. The first measurement can simply
be the elapsed time. The second measurement is a count of the number of times the key is compared
to a list element.

```python
'''Sequential Search algorithm'''

def seqSearch(L, key):
    count = 0
    for i in range(0, len(L) - 1):
        count += 1
        if L[i] == key:
            break
    return count

'''Binary search algorithm'''
def binarySearch(L, key):
    L.sort()
    low, high = 1, len(L) - 1
    location = 0
    count = 0
    while low <= high and location == 0:
        mid = (low + high) // 2
        if L[key] == L[mid]:
            count += 1
            location = mid
        elif L[key] < L[mid]:
            count += 1
            high = mid - 1
        else:
            count += 1
            low = mid + 1
    return count

'''Creates as list of random integers with a key from list'''
def createList(n):
    maxInt = (2 * n) // 3
    L = [random.randrange(1, maxInt, 1) for i in range(n)]
    key = random.choice(L)
    return L, key

'''Main function to run both algorithms and gather running time data'''
def runForN():

    # starts with list of size 10
    n = 10

    # four lists to keep track of # of comparisons and elapsed times for each algo
    seqCountList = []
    seqTimeList = []
    binCountList = []
    binTimeList = []

    # four lists to keep track of # of avg comparisons and elapsed times for each algo
    allAvgSeqCounts = []
    allAvgSeqTime = []
    allAvgBinCounts = []
    allAvgBinTime = []

    # while loop allows for increasing size n lists
    while n != 1000000:

        # for loop allows to run each list of size n 50 times
        for i in range(0, 49):
            L, key = createList(n)

            seqStart = time.time()
            seqCount = seqSearch(L, key)
            seqEnd = time.time()
            seqTime = seqEnd - seqStart

            binStart = time.time()
            binCount = binarySearch(L, key)
            binEnd = time.time()
            binTime = binEnd - binStart

            # appends comparisons and elapsed times to lists
            seqCountList.append(seqCount)
            seqTimeList.append(seqTime)
            binCountList.append(binCount)
            binTimeList.append(binTime)

        # takes avg and appends it to the data list
        averageSeqCount = sum(seqCountList) // len(seqCountList)
        allAvgSeqCounts.append(averageSeqCount)

        # takes avg and appends it to the data list
        averageSeqTime = sum(seqTimeList) / len(seqTimeList)
        allAvgSeqTime.append(averageSeqTime)

        # takes avg and appends it to the data lists
        averageBinCount = sum(binCountList) // len(binCountList)
        allAvgBinCounts.append(averageBinCount)

        # takes avg and appends it to the data lists
        averageBinTime = sum(binTimeList) / len(binTimeList)
        allAvgBinTime.append(averageBinTime)

        # resets the counter lists for the next list of size increasing size n
        seqCountList = []
        seqTimeList = []
        binCountList = []
        binTimeList = []

        # increases list size by tenfold
        n = n * 10

    # returns data
    return allAvgSeqCounts, allAvgSeqTime, allAvgBinCounts, allAvgBinTime
    ```
