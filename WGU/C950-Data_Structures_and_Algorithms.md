# C950 - Data Structures and Algorithms II

## Notes

All chapters before 6.5 I already did before creating these notes. Hence, for time restriction problems, I won't be going over them again and taking notes.

### Chapter 6

#### Hash Table Collisions

When inserting values straight into a hash table which is called open addressing.
You get to a point where the location you want to insert your value to already been taken.
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
For example, resizing to a prime number would be effective for quadratic probing,
but not much for linear probing(at least I think so).

Hash Table resizing can be done when a certain load factor threshold has been
crossed. Or when using chaining(linked list for each bucket), you can resize the
hash table when a certain linked list size has been reached.

#### Hash Functions

A **perfect hash function** is a hash function that maps items with no collisions.
This would mean that insert, search and remove are all O(1).

**Modulo Hash** uses the remainder from the devision of the key with the table size like so:
```python
def mod_hash(key: int | str):
    return key % table_size
```

##### Pros

- Easy to implement.

##### Cons

- Very sensitive to not just the table size, but also the key which is influenced by the type of data you are storing.

**Mid-Square Hash** squares the key and takes X numbers in the middle like so:
```C
int mid_square_hash(int key, int middle_bits_size, int table_size):
    int squared_key = key * key;
    int total_bits_to_remove = sizeof(int) - middle_bits_size;
    int bits_to_remove = total_bits_to_remove / 2;
    int extracted_bits = squared_key >> bits_to_remove; // Removes left bits of the squared key by shifting right.

    /*
    Removes the right bits by masking the bits that need to be transfered.
    This seems like a very limited implementation since its assuming an int is always 32 bits.
    This is untrue for C's and probably C++'s int implementation because int's size is depended on the architecture the code is compiled to.
    */
    extracted_bits &=  0xFFFFFFFF >> total_bits_to_remove; 

    // Mabye a better/more generic implementation will be like so?
    // extracted_bits = extracted_bits << total_bits_to_remove;
    // extracted_bits = extracted_bits >> total_bits_to_remove;
    
    return extracted_bits % table_size // Makes sure to return a key in the table range.
```
Its good to note that the X numbers that are taken from the middle needs to be
bigger than `log10(table_size)`. X bits that are taken from the middle need to
be bigger than `log2(table_size)`.

##### Pros

- Distributes the keys a lot evenly.

##### Cons

- Harder to implement.

**Mulplicative string hash** adds each character ASCII/Unicode value of a string
key to the current hash value(with an initial value set by the programmer)
multiplied by a given multiplier.
```python
def mul_str_hash(key: str, table_size: int, initial_value: int, multiplier: int):
    hash_value = initial_value
    for character in key:
        hash_value = (hash_value * multiplier) + ord(character) # Get the ASCII/Unicode value
        
    return hash_value % table_size
```

A **direct hash funcion** uses the Key itself as the hash value. The table will
only be able to house keys in the range of `[0, table_size - 1]`. A search
algorithm is for a directly hash table will access the bucket of the key value
and if there is a value in the bucket, it returns the value, and if the bucket
is empty, it returns null. This means that there are no collisions and collision
handling!!!

There are 2 requirements for direct hashing to be effective:
 
- The table size must be the value of the largest key in the table plus the
smallest possible key or 0. For example:
```python
def table_size(largest_key: int, smallest_key: int):
    largest_key + min(smallest_key, 0)
```

#### Cryptography and Password Hashing
**Ceasar Hash** shifts the ASCII value of a string left or right by a chosen ammount.
##### Pros

- Easy to implement.
- Fast to encrypt and decrypt

##### Cons

- Not secure because it is computationally inexpensive to decrypt an
encrypted message without knowing the shift ammount that was used to encrypt it.

Hashing functions can be used to verify the integrity of transmitted data. This
is called a checksum and can be done with many hashing functions like MD5.

###### Password Hashing
Usually you store the hashes of your users passwords insthead of the actual
passwords. When a user wants to log in, you hash the password they enter and
compare it to the hashed password you stored in the database. This makes it
harder for hackers that might break into your database to get the actual
passwords as the password won't be able to be reconstructed from the password
hash that was stored.

