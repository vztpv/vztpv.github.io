# 1. var = val 
# 2. val
# 3. var = { }
# 4. { }

# Think of mixed 1,2,3,4.
# array is like.. var = { val val val ... } or { val val val ... } ...
# object is like.. var = { var = val var = val ... } or { var = val var = val var = val ... } ...
# mixed? is like.. var = { val var = val ... } or { val var = val ... } ...

# now, 이렇게 구성된(1~4가 혼합된?) 문자열을 병렬 파싱해봅시다. <- parallel parenthesis matching와 큰틀은 같습니다. 
