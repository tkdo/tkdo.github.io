##  1. vector容器

### 1.1. 打印
    
```c++
void printVector(vector<int> &v)
{
for(vector<int>::iterator it = v.begin(); it != v.end(); it++)
{
    cout<< *it << ", ";
}
cout <<endl;
}
```

### 1.2. 插入删除
* push_back(ele);//尾部插入ele
* pop_back();//删除最后一个元素
* insert(const_iterator pos, ele);//迭代器指向pos插入元素ele
* insert(const_iterator pos, int count, ele);//迭代器指向位置pos插入count个元素ele
* erase(const_iterator pos);//删除迭代器指向的元素
* erase(const_iterator start, const_iterator end);//删除迭代器 start和end之间的元素
* clear();//删除容器中所有的元素
#### 1.2.1. 案例
```c++
vector<int> v1;
  //尾插
  v1.push_back(10);
  v1.push_back(20);
  v1.push_back(30);
  v1.push_back(40);
  v1.push_back(50);
  v1.push_back(60);
  //遍历
  printVector(v1);
  //尾删
  v1.pop_back();
  printVector(v1);

  v1.insert(v1.begin(), 100);
  printVector(v1);
  v1.insert(v1.begin(), 2, 1000);
  printVector(v1);

  v1.erase(v1.begin());
  printVector(v1);
  //清空
  v1.erase(v1.begin(), v1.end());
  printVector(v1);
```
### 1.3. 遍历访问
#### 1.3.1 案例
```c++
  vector<int> v1;
  for(int i=0; i<10; i++)
  {
    v1.push_back(i);
  }

  for (int i=0; i<v1.size(); i++){
    cout << v1[i] << ", ";
  }
  cout << endl;

  for (int i=0; i <v1.size();  i++){
    cout <<  v1.at(i) << ", ";
  }
  cout << endl;

  cout  << "第一个元素：" << v1.front() << endl;
  cout  << "最后一个元素：" << v1.back() << endl;
```
### 1.4. swap 交换 
#### 1.4.1 案例
```c++
  vector<int> v1;
  for(int i=0; i<10; i++)
  {
    v1.push_back(i);
  }
  printVector(v1);

  vector<int> v2;
  for(int i=10; i>0; i--){
    v2.push_back(i);
  }
  printVector(v2);

  cout << "交换后：" << endl;
  v1.swap(v2);

  printVector(v1);
  printVector(v2);
```
#### 1.4.1 巧用swap释放内存
```c++
  vector<int> v;
  for (int i=0; i<100000; i++)
  {
    v.push_back(i);
  }
  cout  << "v容量为：" << v.capacity() << endl;
  cout  << "v大小为：" << v.size() << endl;
  v.resize(3);
  cout  << "v容量为：" << v.capacity() << endl;
  cout  << "v大小为：" << v.size() << endl;
  vector<int>(v).swap(v);
  cout  << "v容量为：" << v.capacity() << endl;
  cout  << "v大小为：" << v.size() << endl;
  //vector<int>(v) 匿名对象，按照v目前所用的元素个数初始化匿名对象，
  //swap容器交换
  //匿名对象执行完成，内存回收
```

## 2. Deque 容器
### 2.1. 打印
  ```c++
  void printDeque(const deque<int> &d)
  {
  for(deque<int>::const_iterator it = d.begin(); it != d.end(); it++)
  {
      //*it = 100; 为了不让用户修改
      //const, const_iterator 一起使用否则报错
      cout<< *it << ", ";
  }
  cout <<endl;
  }
  ```
### 2.2. 构造函数
#### 2.2.1. 构造函数
 * deque<T>  deqT; //默认构造形式
 * deque(beg, end); //构造函数[beg, end)区间中的元素拷贝给本身
 * deque(n, elem); //构造函数将n个elem拷贝给本身
 * deque(const deque, &deq); //拷贝构造函数
#### 2.2.2. 代码样例
  ```c++
    deque<int> d1;
    for(int i=0; i<10; i++)
    {
      d1.push_back(i);
    }
    printDeque(d1);
    deque<int>d2(d1.begin(), d1.end());
    printDeque(d2);
    deque<int>d3(10, 100);
    printDeque(d3);
    deque<int>d4(d3);
    printDeque(d4);
  ```
#### 2.3. 赋值
  ```c++
    printDeque(d1);
    //operator=赋值
    deque<int>d2;
    d2 = d1;
    printDeque(d2);
    // assign 赋值
    deque<int>d3;
    d3.assign(d1.begin(), d1.end());
    printDeque(d3);
    deque<int>d4;
    d4.assign(10, 100);
    printDeque(d4);
  ```
