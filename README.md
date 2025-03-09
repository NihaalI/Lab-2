import time
import random

# Association List Functions (Dictionary-Like Operations)
def alist_insert(alist, key, value):
    for i, (k, v) in enumerate(alist):
        if k == key:
            alist[i] = (key, value)  # Update existing key
            return alist
    alist.append((key, value))  # Insert new key-value pair
    return alist

def alist_lookup(alist, key):
    for k, v in alist:
        if k == key:
            return v
    raise KeyError(key)  # Raise an error if key is not found

def alist_remove(alist, key):
    for i, (k, v) in enumerate(alist):
        if k == key:
            del alist[i]  # Delete key-value pair
            return alist
    raise KeyError(key)  # Raise an error if key is not found

def alist_get_keys(alist):
    return [k for k, v in alist]  # Return list of all keys

def alist_get_values(alist):
    return [v for k, v in alist]  # Return list of all values

# Function to Compare Runtime Performance
def compare_runtime(num_inserts, num_lookups):
    student_ids = [random.randint(10000000, 99999999) for _ in range(num_inserts)]
    grades = [random.randint(0, 100) for _ in range(num_inserts)]
    lookup_ids = random.sample(student_ids, min(num_lookups, num_inserts))

    # Timing Dictionary Insertions
    python_dict = {}
    t0 = time.time()
    for i in range(num_inserts):
        python_dict[student_ids[i]] = grades[i]
    t1 = time.time()
    dict_insert_time = t1 - t0

    # Timing Association List Insertions
    assoc_list = []
    t0 = time.time()
    for i in range(num_inserts):
        assoc_list = alist_insert(assoc_list, student_ids[i], grades[i])
    t1 = time.time()
    alist_insert_time = t1 - t0

    # Timing Dictionary Lookups
    t0 = time.time()
    for sid in lookup_ids:
        _ = python_dict[sid]
    t1 = time.time()
    dict_lookup_time = t1 - t0

    # Timing Association List Lookups
    t0 = time.time()
    for sid in lookup_ids:
        _ = alist_lookup(assoc_list, sid)
    t1 = time.time()
    alist_lookup_time = t1 - t0

    return (dict_insert_time, alist_insert_time, dict_lookup_time, alist_lookup_time)

# Performance Testing
test_cases = [(10000, 5000), (20000, 10000), (40000, 20000), (80000, 40000)]

for num_inserts, num_lookups in test_cases:
    results = compare_runtime(num_inserts, num_lookups)
    print(f"Insertions: {num_inserts}, Lookups: {num_lookups}")
    print(f"Dictionary - Insert Time: {results[0]:.6f}s, Lookup Time: {results[2]:.6f}s")
    print(f"Assoc List - Insert Time: {results[1]:.6f}s, Lookup Time: {results[3]:.6f}s")
    print("-" * 50)

# Answering Questions
print("\n### Analysis ###")
print("1. Dictionary insertions and lookups remain fast even as data size increases.")
print("2. Association list becomes much slower, especially for lookups due to O(n) complexity.")
print("3. Disadvantages of association list:")
print("   - Slow lookup time (linear search).")
print("   - Inefficient insertions for large datasets.")
print("   - No built-in hashing like Python dictionaries.")
print("4. Using lists with student IDs as indices is impractical because:")
print("   - IDs are not sequential (e.g., 10000000 to 99999999).")
print("   - A direct list lookup would require an enormous list, wasting memory.")
print("   - Lookup would be O(n) instead of O(1) in a dictionary.")
