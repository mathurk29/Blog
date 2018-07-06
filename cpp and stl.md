# C++ and STL
	
The simple idea is to pass __data type as a parameter__ so that we don’t need to write same code for different data types.

Templates are expended at compiler time. This is like macros. The difference is, compiler does type checking before template expansion.

__Keywords - typename, template__

__Function Templates__ We write a generic function that can be used for different data types. Examples of function templates are sort(), max(), min(), printArray()

```cpp
template <typename T>
T myMax(T x, T y)
{
   return (x > y)? x: y;
}
```

__Class Templates:__



```cpp
template <typename T>
class Array {
private:
    T *ptr;
    int size;
public:
    Array(T arr[], int s);
    void print();
};
 
template <typename T>
Array<T>::Array(T arr[], int s) {
    ptr = new T[s];
    size = s;
    for(int i = 0; i < size; i++)
        ptr[i] = arr[i];
}
 
template <typename T>
void Array<T>::print() {
    for (int i = 0; i < size; i++)
        cout<<" "<<*(ptr + i);
    cout<<endl;
}
```

__More than one arg and default arg__

```cpp
#include<iostream>
using namespace std;
 
template<class T, class U = char>
class A  {
    T x;
    U y;
Public:
    A() {    cout<<"Constructor Called"<<endl;   }
};
 
int main()  {
   A<char, char> a;
   A<int, double> b;
   return 0;
}
```
Each instance of a template contains its own __static variable__

__Function overloading is used when multiple functions do similar operations, templates are used when multiple functions do identical operations.__

## Tempelate Specialization

What if we want a different code for a particular data type?

Use __templete<>__ keyword.

```cpp
// A generic sort function 
template <class T>
void sort(T arr[], int size)
{
    // code to implement Quick Sort
}
 
// Template Specialization: A function 
// specialized for char data type
template <>
void sort<char>(char arr[], int size)
{
    // code to implement counting sort
}
```

## Vectors

Dynamic ararays
resize automatically
contiguous

```cpp
vector<int> myvector{1, 2, 3, 4, 5, 6, 7, 8, 9};
```

__Iterators__
1. begin() – Returns an iterator pointing to the first element in the vector
2. end() – Returns an iterator pointing to the theoretical element that follows last element in the vector
3. rbegin() – Returns a reverse iterator pointing to the last element in the vector (reverse beginning). It moves from last to first element
4. rend() – Returns a reverse iterator pointing to the theoretical element preceding the first element in the vector (considered as reverse end)

__Capacity__
1. size() – Returns the number of elements in the vector
2. max_size() – Returns the maximum number of elements that the vector can hold
3. capacity() – Returns the size of the storage space currently allocated to the vector expressed as number of elements
4. resize(size_type g) – Resizes the container so that it contains ‘g’ elements
5. empty() – Returns whether the container is empty

__Accessing the elements__
1. reference operator [g] – Returns a reference to the element at position ‘g’ in the vector
2. at(g) – Returns a reference to the element at position ‘g’ in the vector
3. front() – Returns a reference to the first element in the vector
4. back() – Returns a reference to the last element in the vector



Vector.clear() - remove all elements thus reducing size to 0.
Vector.erase(poition) || vector.erase(start, end) - remove at the mentioned position(s)

## List

Lists are sequence containers that allow non-contiguous memory allocation. 

front() – Returns the value of the first element in the list
back() – Returns the value of the last element in the list
push_front(g) – Adds a new element ‘g’ at the beginning of the list
push_back(g) – Adds a new element ‘g’ at the end of the list
pop_front() – Removes the first element of the list, and reduces size of the list by 1
pop_back() – Removes the last element of the list, and reduces size of the list by 1
begin() – Returns an iterator pointing to the first element of the list
end() – Returns an iterator pointing to the theoretical last element which follows the last element
empty() – Returns whether the list is empty(1) or not(0)
insert() – Inserts new elements in the list before the element at a specified position
erase() – Removes a single element or a range of elements from the list
assign() – Assigns new elements to list by replacing current elements and resizes the list
remove() – Removes all the elements from the list, which are equal to given element
reverse() – Reverses the list
size() – Returns the number of elements in the list
sort() – Sorts the list in increasing order

## Deque

They are similar to vectors, but are more efficient in case of insertion and deletion of elements at the end, and also the beginning. Unlike vectors, contiguous storage allocation may not be guaranteed.

__The functions for deque are same as vector, with an addition of push and pop operations for both front and back.__

