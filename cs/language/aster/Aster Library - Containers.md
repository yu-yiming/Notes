# Aster Library - Containers

This file is a proposal for the **Aster** container library. The proposed types and algorithms are listed as follows:

- `std::vector`: A dynamic array just like **C++** `std::vector`.
- `std::list`: A more functional linked list than the built-in lists.
- `std::bytes`: A byte-oriented string just like **C++** `std::basic_string`.
- `std::string`: A character-oriented string that support **UTF-8**.
- `std::set`: A sorted collection of data just like **C++** `std::set`.
- `std::map`: A sorted collection of key-value pairs just like **C++** `std::map`.
- `std::hashset`:
- `std::hashmap`:

Next we will talk about these containers.

## Vectors



```cpp
// vector.ah
module prelude;
namespace std {

export type protected:
	const vector;
export object:
	const make_vector;
	static default_capacity = 8;
        
}
```

```cpp
// vector.ast
module prelude;
namespace std {
    
const vector = T : type -> (m_data : [T], size : int, capacity : int);
const make_vector : [T] -> vector T;
static default_capacity = 8;

const make_vector [] = (new default_capacity T, 0, default_capacity);
const make_vector vec = make_vector_aug vec (new (sizeof vec) T, 0, default_capacity)
    where make_vector_aug [] res = res,
   	      make_vector_aug [x, rest...]  
    
}
```

