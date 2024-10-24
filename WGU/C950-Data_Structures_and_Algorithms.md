# C950 - Data Structures and Algorithms II

## Notes

### Chapter 6

#### Hash Table Collisions
When inserting values into a hash table, you get to a point where the location you want to insert your value to already been taken.
This is called a collision, and there are many ways to deal with them.
1. **Linear Probing** where you go to the next location if the current localtion your are trying to insert to is already taken using the formula 
```python
current_location + 1
```
##### Pros:
- Easy to implement.
- Very good hashmap utilization ammount (about 70%).
- Better for caching. Don't know why. **Note:** Mabye dig further.
##### Cons:
- Performance doesn't scale well because it ends up creating data clusters like so:
```python
[#,#,#,#,#,#,#, , , , ,#,#,#,#,#,#, , , , , , , ]
```
which causes a lot of probing iterations to be made.

2. **Quadratic Probing** where you go to the next location if the current localtion your are trying to insert to is already taken using the formula 
```python
current_location + C1*i + C2*i^2
```
C1 and C2 are constants that are picked by the programmer.
I still don't know what C1 and C2 are good to pick and why.
**Note:** Mabye dig into this

##### Pros:
- Performance scales much better than linear probing due to data being created sparsly like so:
```python
[#,#,#, , ,#,#, , ,#,#,#, , , ,#,#, , ,#, , ,#,#]
````
- Don't know. **Note:** Mabye look into more pros.
##### Cons:
- Worse for caching than linear probing. Don't know why. **Note:** Mabye dig further.
- Harder to implement than linear probing but not by much.
- Need to pick specific hash table sizes(ex. prime numbers) to perform optimally.

3. **Double Hashing** does the same thing as the other two above, but it uses 2 hash functions to do this
```python
(hash1(current_location) + i * hash2(current_location)) % table_size
```
##### Pros:
- IDK. Note: Mabye find out
##### Cons:
- IDK. Note: Mabye find out

#### Hash Table Resizing

Hash resizing stratagy largley depends on the picked probing strategy.
For example, resizing to a prime number would be effective for quadratic probing, but not much for linear probing(at least I think so).

