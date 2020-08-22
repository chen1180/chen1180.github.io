---
layout: post
title: C++ Iterator
description: C++ notes
tags: [c++]
categories: C++
---
# C++ Notes

In c++, An iterator is any object that, pointing to some element in a range of elements (such as an array or a container), has the ability to iterate through the elements of that range using a set of operators (with at least the increment (++) and dereference (*) operators).  

For example,
```javascript
// C++ code to demonstrate the working of 
// iterator, begin() and end() 
#include<iostream> 
#include<iterator> // for iterators 
#include<vector> // for vectors 
using namespace std; 
int main() 
{ 
    vector<int> ar = { 1, 2, 3, 4, 5 }; 
      
    // Declaring iterator to a vector 
    vector<int>::iterator ptr; 
      
    // Displaying vector elements using begin() and end() 
    cout << "The vector elements are : "; 
    for (ptr = ar.begin(); ptr < ar.end(); ptr++) 
        cout << *ptr << " "; 
      
    return 0;     
} 
```
### Notification

{: .box-note}
**Note:** This is a notification box.

ome inline Latex: $$a^2 + b^2 = c^2$$  
$$\int e^{-kx} \, dx = -\frac{1}{k} e^{-kx}$$
### Warning

{: .box-warning}
**Warning:** This is a warning box.

### Error

{: .box-error}
**Error:** This is an error box.
