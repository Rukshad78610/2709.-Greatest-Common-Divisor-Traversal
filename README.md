import java.util.*;

public class Solution {
    public boolean canTraverseAllPairs(int[] nums) {
        int numElements = nums.length;

        if (numElements == 1) {
            return true;
        }

        int[] disjointSet = new int[numElements];
        int[] setSize = new int[numElements];
        Map<Integer, Integer> factorFirstOccurrence = new HashMap<>();

        for (int i = 0; i < numElements; i++) {
            disjointSet[i] = i;
            setSize[i] = 1;
        }

        for (int i = 0; i < numElements; i++) {
            int num = nums[i];
            int divisor = 2;

            while (divisor * divisor <= num) {
                if (num % divisor == 0) {
                    if (factorFirstOccurrence.containsKey(divisor)) {
                        unionSets(disjointSet, setSize, i, factorFirstOccurrence.get(divisor));
                    } else {
                        factorFirstOccurrence.put(divisor, i);
                    }

                    while (num % divisor == 0) {
                        num /= divisor;
                    }
                }
                divisor++;
            }

            if (num > 1) {
                if (factorFirstOccurrence.containsKey(num)) {
                    unionSets(disjointSet, setSize, i, factorFirstOccurrence.get(num));
                } else {
                    factorFirstOccurrence.put(num, i);
                }
            }
        }

        return setSize[findSetLeader(disjointSet, 0)] == numElements;
    }

    private void unionSets(int[] disjointSet, int[] setSize, int x, int y) {
        int xLeader = findSetLeader(disjointSet, x);
        int yLeader = findSetLeader(disjointSet, y);

        if (xLeader == yLeader) {
            return;
        }

        if (setSize[xLeader] < setSize[yLeader]) {
            disjointSet[xLeader] = yLeader;
            setSize[yLeader] += setSize[xLeader];
        } else {
            disjointSet[yLeader] = xLeader;
            setSize[xLeader] += setSize[yLeader];
        }
    }

    private int findSetLeader(int[] disjointSet, int x) {
        if (disjointSet[x] != x) {
            disjointSet[x] = findSetLeader(disjointSet, disjointSet[x]);
        }
        return disjointSet[x];
    }
}
