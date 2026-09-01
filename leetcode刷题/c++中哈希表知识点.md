# C++哈希表核心精简知识点
## 1. 哈希容器区分
只有带`unordered_`才是哈希表，底层哈希桶；`set/map`是红黑树，不是哈希表。
- unordered_set：存单一值，自动去重
- unordered_map：存key-value键值对，key唯一
- unordered_multiset/multimap：允许key重复

## 2. 核心特性
1. 增删查平均O(1)，冲突严重退化O(n)
2. 遍历无序，顺序不固定
3. 负载因子默认0.75，超阈值自动rehash扩容

## 3. unordered_set 常用API
unordered_set<int> st(nums.begin(), nums.end()); // 批量构造自动去重

st.insert(x); // 重复元素插入无效


st.count(x); // 存在返回1，不存在0


st.find(x); // 找到返回迭代器，找不到st.end()

st.erase(x); st.clear();

// 遍历：范围for / 迭代器，不支持下标st[i]

## 4. unordered_map 常用API

unordered_map<int, int> mp;

mp[key] = val; // key不存在会自动插入

mp.insert({k, v});

mp.count(k); mp.find(k);

it->first // key，it->second // value

mp.erase(k);
## 5. 关键函数说明

- count (x)：快速判断元素是否存在，set/map 只返回 0 或 1
- find (x)：返回迭代器，适合需要获取元素的场景

 哈希表补充