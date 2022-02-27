# 문제 정의
Given a well-formed sequence of parentheses stored in an array,
determine the index of the mate of each parentheses stored in the array.

# Sequential 접근
```cpp
std::vector<int> solve(const char* str, int64_t n) {
  std::vector<int> vec(n, 0);
  std::vector<Pos> _stack; _stack.reserve(n / 2);

  for (int64_t i = 0; i < n; ++i) {
    if (str[i] == '(') {
      _stack.push_back(i);
    }
    else {
      if (!_stack.empty()) {
        int pos = _stack.back(); _stack.pop_back();
        vec[i] = pos;
        vec[pos] = i;
      }
      // else { not valid }
    }
  }

	return vec;
}


```

# Parallel 접근
```cpp
void solve_parallel_part1(Pos* vec, Stack* _stack, const char* str, int64_t start, int64_t n) 
{

	int a = clock();

	for (int64_t i = 0; i < n; ++i) {
		if (str[i] == '(') {
			push_back(_stack, (start + i + 1));
		}
		else {
			if (!empty(_stack)) {
				//std::cout << "not empty\n";
				Pos x = back(_stack); //_stack.pop_back();
				if (x > 0) {
					(vec)[start + i] = x - 1 ;
					(vec)[x - 1] = start + i;

					//std::cout << "here\n";
					pop_back(_stack);
				}
				else {
					push_front(_stack, - (start + i + 1));
				}
			}
			else {
				push_back(_stack, -(start + i + 1));
			}
		}
	}
	
	int b = clock();
	std::cout << b - a << "ms\n";
}

```
```C++
void solve_parallel_part2(Pos* vec, Stack* _stack[THR_NUM]) {
	for (int i = 1; i < THR_NUM; ++i) {
		int idx = -1;
		for (int64_t j = 0; j < size((_stack)[i]); ++j) {
			if ((_stack)[i]->_arr[j] > 0) {
				break;
			}
			idx++;
		}
		if (idx > -1) {
			for (int64_t j = idx; j >= 0; --j) {
				Pos before = back(_stack[0]); pop_back((_stack)[0]);
				Pos now = (_stack)[i]->_arr[j];

				vec[before - 1] = -now - 1;
				vec[-now - 1] = before - 1;

				std::cout << -now - 1 << " " << before - 1 << "\n";
			}
			for (int64_t j = idx + 1; j < size((_stack)[i]); ++j) {
				push_back(_stack[0], (_stack)[i]->_arr[j]);
			}
		}
		else {
			for (int64_t j = 0; j < size((_stack)[i]); ++j) {
				push_back(_stack[0], (_stack)[i]->_arr[j]);
			}
		}
	}
}
```
