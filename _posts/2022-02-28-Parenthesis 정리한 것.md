# Node

  (()()) # valid parentheses.
  
  -> (  (), () )
  
     A B C
     
  Node A has Node B and Node C. (Node A is parent of Node B, and Node C)
  
  (Node B and Node C are children of A)
  

# Virtual Node

  ()()) # not valid parentheses.
  
  ->  #(  () () )
  
      A` B C
      
  A` <- virtual Node
  
  Think ()()) as #( ()()) 
  
# (My own) Parallel Matching?
  1. Divide list of parentheses into several blocks 
  2. Step1 ( parallel ) 
  3. Step2 ( sequential )
  
# Step 1.
```cpp
for (int64_t i = 0; i < n; ++i) { // n : size of block.
  if (str[i] == '(') {
    push_back(_stack, (start + i + 1)); // start : divieded block`s start index. // start + i + 1 ( > 0 )
  }
  else {
    if (!empty(_stack)) {
      Pos x = back(_stack);
      if (x > 0) { // is node. (not virtual node)
        (vec)[start + i] = x - 1 ; // write
        (vec)[x - 1] = start + i;  // write

        pop_back(_stack);
      }
      else { // virtual node
        // virtual node insert. think case )   )
        //                                    here 
        push_front(_stack, - (start + i + 1)); // - ( start+ i + 1) < 0
      }
    }
    else {
      // virtual node insert think case ()  )
      //                                   here
      push_back(_stack, -(start + i + 1));
    }
  }
}
```

# Step 2.
```cpp
for (int i = 1; i < THR_NUM; ++i) { // i == 0, _stack[i] has no virtual-node. only real-node. THR_NUM <- used thread count.
  int idx = -1;
  for (int64_t j = 0; j < size(_stack[i]); ++j) {
    if (_stack[i]->_arr[j] > 0) { // is real node?
      break;
    }
    idx++;
  }
  if (idx > -1) { // i-th has virtual node?
    for (int64_t j = idx; j >= 0; --j) {
      Pos before = back(_stack[0]); pop_back(_stack[0]);
      Pos now = _stack[i]->_arr[j];

      // write.
      vec[before - 1] = -now - 1; // before is real-node, now is virtual-node 
      vec[-now - 1] = before - 1; 
    }
    for (int64_t j = idx + 1; j < size(_stack[i]); ++j) {
      push_back(_stack[0], _stack[i]->_arr[j]);
    }
  }
  else {
    for (int64_t j = 0; j < size(_stack[i]); ++j) {
      push_back(_stack[0], _stack[i]->_arr[j]);
    }
  }
}
```
