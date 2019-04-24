---
title: purrr in mutate
---

<!-- Generated automatically from purrr-mutate.yml. Do not edit by hand -->

# purrr in mutate <small class='program'>[program]</small>
<small>(Builds on: [Vector functions](function-vector.md), [purrr basics](purrr-basics.md))</small>  
<small>(Leads to: [List-columns](list-cols.md), [purrr map with multiple inputs](purrr-parallel.md))</small>

Until now, you've been using vector functions inside `mutate()` to create new
columns. In *Vector functions*, you learned how to convert some scalar functions
into vector functions. Sometimes, however, you'll want to use a scalar function 
to create a new column, and you won't be able to convert that scalar function 
into a vector function. 

In this reading, you'll learn how to create new columns with a scalar function 
by using a map function to apply that scalar function to each element of an 
existing column.

## Readings

  * [purrr inside mutate](https://dcl-prog.stanford.edu/purrr-mutate.html) [prog-11]


