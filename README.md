# operating-system-project
import threading
import numpy as np

# Define the maximum number of resources of each type
MAX_RESOURCES = np.array([10, 5, 7])

# Define the available resources of each type
available_resources = MAX_RESOURCES.copy()

# Define the allocation matrix, which indicates the resources allocated to each thread
allocation = np.zeros((10, 3))

# Define the maximum matrix, which indicates the maximum resources each thread can request
maximum = np.zeros((10, 3))

# Define a list of threads
threads = []

# Define a mutex lock for accessing shared data
mutex = threading.Lock()

# Define a function that checks if a state is safe
def is_safe_state():
    work = available_resources.copy()
    finish = np.zeros(len(threads))
    while np.any(finish == 0):
        safe = False
        for i in range(len(threads)):
            if finish[i] == 0 and np.all(work >= maximum[i] - allocation[i]):
                work += allocation[i]
                finish[i] = 1
                safe = True
        if not safe:
            return False
    return True

