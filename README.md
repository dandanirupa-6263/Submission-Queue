# Submission-Queue
A web-based platform for managing and tracking assignment and project submissions efficiently.
#include <stdio.h>
#include <stdbool.h>

int binary_search(int arr[], int l, int r, int target) {
    while (l <= r) {
        int mid = l + (r - l) / 2;
        if (arr[mid] == target) return mid;
        if (arr[mid] < target) l = mid + 1;
        else r = mid - 1;
    }
    return -1;
}

int main() {
    int N;
    if (scanf("%d", &N) != 1) return 0;
    int times[N];
    for (int i = 0; i < N; i++) scanf("%d", &times[i]);

    bool active[N];
    for (int i = 0; i < N; i++) active[i] = false;

    int currentSize = 0, maxSize = 0;
    for (int i = 0; i < N; i++) {
        // add this submission
        active[i] = true;
        currentSize++;

        // remove any submission exactly 5000 seconds before if still active
        int target = times[i] - 5000;
        int idx = binary_search(times, 0, i - 1, target);
        if (idx != -1 && active[idx]) {
            active[idx] = false;
            currentSize--;
        }

        if (currentSize > maxSize) maxSize = currentSize;
    }

    printf("%d\n", maxSize);
    return 0;
}
