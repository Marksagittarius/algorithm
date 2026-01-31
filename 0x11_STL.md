# 0x11 STL

## 引言

STL（Standard Template Library）是使用 C++ 求解算法问题的利器，包含了容器、算法库、迭代器、函数对象等基础工具。

本章节将着重介绍 STL 提供的容器类若干。

## vector

```cpp
std::vector<T> vec;
vec.capacity(); 
vec.size();
vec.at(int idx); 

vec.push_back(); 
vec.pop_back(); 
vec.front(); 
vec.back(); 
vec.begin();
vec.end();

vec.insert(pos, elem);    
vec.insert(pos, n, elem);
vec.insert(pos, begin, end); 

vec.erase(pos);
vec.erase(begin, end); 

vec.reverse(pos1, pos2);
```

## stack

```cpp
std::stack<T> stk;

stk.empty();
stk.pop();
stk.push(); 
stk.size();
stk.top();
```

## queue

```cpp
std::queue<T> que;

que.front(); 
que.back();
que.push(T&& obj);
que.pop(); 
que.size();
que.empty();
```

## priority_queue

```cpp
std::priority_queue<T, vector<T>, less<T>> pq;
std::priority_queue<T, vector<T>, greater<T>> pq;

pq.top();
pq.empty();
pq.size();
pq.push();
pq.pop();
```

## set/unordered_set

```cpp
std::set<T, &compare> st;
std:unordered_set<T> st;

st.size();
st.empty();
st.begin();
st.end();
st.insert(elem);

st.erase(pos);
st.erase(elem);

st.clear();
st.find(elem);
st.count(elem);
```

## map/unordered_map

```cpp
std::map<T1, T2> mp;
std::unordered_map<T1, T2> mp;

mp.at(key); 
mp[key]; 

mp.count(key); 
mp.max_size();
mp.size();
mp.begin(); 
mp.end(); 

mp.insert(elem); 
mp.insert(pos, elem); 
mp.insert(begin, end); 
mp.erase(pos); 
mp.erase(begin, end);
mp.erase(key);
```

## string

```cpp
std::string str;

str.size();
str.length();
str.push_back(ch);

str.append(s);
str.erase(pos);
str.erase(begin, end);
str.replace(pos, n, s);
str.replace(begin, end, s);
str.find(s);
str.find(ch, n);

str.substr(begin, end);
```

## deque

```cpp
std::deque<T> dq;

dq.size();
dq.empty();
dq.at(idx);
dq[idx];
dq.front();
dq.back();

dq.push_back(elem);
dq.push_front(elem);
dq.pop_back();
dq.pop_front();

dq.begin();
dq.end();
dq.insert(pos, elem);
dq.erase(pos);
dq.clear();

```

## list

```cpp
std::list<T> lst;

lst.size();
lst.empty();
lst.front();
lst.back();

lst.push_back(elem);
lst.push_front(elem);
lst.pop_back();
lst.pop_front();

lst.begin();
lst.end();
lst.insert(pos, elem);
lst.erase(pos);

lst.remove(elem);
lst.unique();
lst.reverse();
lst.sort();
lst.merge(lst2);

```

## multiset

```cpp
std::multiset<T> mst;

mst.size();
mst.empty();
mst.insert(elem);
mst.find(elem);
mst.count(elem);

mst.lower_bound(elem);
mst.upper_bound(elem);
mst.equal_range(elem);

mst.erase(pos);
mst.erase(elem);
mst.clear();
```

## multimap

```cpp
std::multimap<T1, T2> mmp;

mmp.size();
mmp.empty();
mmp.insert(make_pair(k, v));

mmp.find(key);
mmp.count(key);
mmp.lower_bound(key);
mmp.upper_bound(key);
mmp.equal_range(key);

mmp.erase(pos);
mmp.erase(key);
mmp.clear();
```

## pair

```cpp
std::pair<T1, T2> p;

p.first;
p.second;

std::make_pair(v1, v2);
```
