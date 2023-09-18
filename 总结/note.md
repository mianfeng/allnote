# 旨在收集碰到的一些较好的写法

- [旨在收集碰到的一些较好的写法](#旨在收集碰到的一些较好的写法)
  - [1.大小写转换与移除非字母数字字符](#1大小写转换与移除非字母数字字符)
  - [2.KMP算法的C++实现](#2kmp算法的c实现)
  - [3.lamdba表达式的知识点](#3-lamdba表达式的知识点)
  - [4.智能指针shared\_ptr](#4-智能指针shared_ptr)
  - [5.递归的思想](#5-递归的思想)
  - [6.双指针](#6-双指针)

## 1.大小写转换与移除非字母数字字符

   ```c++
// 将大写字符转换为小写字符
std::transform(s.begin(), s.end(), s.begin(), ::tolower);

// 移除非字母数字字符
s.erase(std::remove_if(s.begin(), s.end(), [](char c) { return !std::isalnum(c); }), s.end());
   ```

## 2.KMP算法的C++实现

   ```C++
class Solution {
public:
    int strStr(string s, string p) {
        int n = s.size(), m = p.size();
        if(m == 0) return 0;
        //设置哨兵
        s.insert(s.begin(),' ');
        p.insert(p.begin(),' ');
        vector<int> next(m + 1);
        //预处理next数组
        for(int i = 2, j = 0; i <= m; i++){
            while(j and p[i] != p[j + 1]) j = next[j];
            if(p[i] == p[j + 1]) j++;
            next[i] = j;
        }
        //匹配过程
        for(int i = 1, j = 0; i <= n; i++){
            while(j and s[i] != p[j + 1]) j = next[j];
            if(s[i] == p[j + 1]) j++;
            if(j == m) return i - m;
        }
        return -1;
    }
};
   ```

## 3. lamdba表达式的知识点

   ![image](https://github.com/mianfeng/allnote/assets/64387330/edd382b9-3c55-46da-b7d0-9deb745604e1)

## 4. 智能指针shared_ptr

   ```C++
#include <iostream>
#include <memory>

class MyClass {
public:
    MyClass(int value) : data(value) {
        std::cout << "MyClass Constructor" << std::endl;
    }

    ~MyClass() {
        std::cout << "MyClass Destructor" << std::endl;
    }

    void printData() {
        std::cout << "Data: " << data << std::endl;
    }

private:
    int data;
};

int main() {
    // 创建 shared_ptr
    std::shared_ptr<MyClass> sharedPtr1 = std::make_shared<MyClass>(42);
    sharedPtr1->printData();

    // 复制 shared_ptr，引用计数增加
    std::shared_ptr<MyClass> sharedPtr2 = sharedPtr1;
    sharedPtr2->printData();

    // 使用 weak_ptr 避免循环引用问题
    std::shared_ptr<MyClass> sharedPtr3 = std::make_shared<MyClass>(99);
    std::weak_ptr<MyClass> weakPtr = sharedPtr3;

    // 通过 weak_ptr 获取 shared_ptr，需要检查是否有效
    if (auto sharedPtr4 = weakPtr.lock()) {
        sharedPtr4->printData();
    } else {
        std::cout << "Weak pointer expired" << std::endl;
    }

    return 0;
}
   ```

   **同时两种初始化方式有以下区别：**
   使用 std::make_shared 的优势在于内存分配和对象构造在一个步骤中完成，通常会比直接初始化 std::shared_ptr 更高效。此外，当多个 std::shared_ptr 共享同一个资源时，引用计数的内存也会分配在同一个内存块中。直接初始化 std::shared_ptr 是通过构造函数来分配内存和构造对象的。这意味着可能会分别执行两次内存分配，一次为对象本身，一次为引用计数。这种情况下，可能会在内存中存在两个不相邻的块，一个存储对象，一个存储引用计数。

## 5. 递归的思想

   >**使用递归求解，一定不要用大脑去模拟递归的过程。大脑能压几个栈？正确的做法是：记住递归函数的定义。比如本题中的递归函数`countAndSay(int n)`含义是当取值为 `n` 时的外观数列。那么，必须先求出取值为 ```n−1```时的外观数列，怎么求？根据递归函数的定义，就是 ```countAndSay(n - 1)```。至于```countAndSay(n - 1)```怎么算的，我们不用管。只要知道这个函数能给我们正确的结果就行。**

   **有例题见力扣38**

​	还是下面的题目，借助另一种方法来理解递归的思想，循环是从后往前，递归是从前往后

```C++
    int cur=0;
    ListNode* removeNthFromEnd(ListNode* head, int n) {
       if(!head) return NULL;
       head->next = removeNthFromEnd(head->next,n);
       cur++;
       if(n==cur) return head->next;
       return head;
    }
```

## 6. 双指针

删除单链表倒数第N个元素，采用快慢指针的方法，这样可以只遍历一次。同时有个细节，新建一个`dummy`指向`head`是为了可以避免讨论头节点被删除的情况

```C++
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummy = new ListNode(0);
        dummy->next = head;
        ListNode* fast = dummy;
        ListNode* slow = dummy;
        for (int i = 0; i < n; i++) {
            fast = fast->next;
        }
        while (fast->next) {
            fast = fast->next;
            slow = slow->next;
        }
        slow->next = slow->next->next;
        return dummy->next;
    }
```
## 7. 右值
左值（Lvalue）：左值是具有持久性的表达式，通常是可以对其取地址的、可以被赋值的、可以被引用的表达式。它们是程序中具名的变量或对象。

右值（Rvalue）：右值是临时的、不具有持久性的表达式，通常是字面常量、临时对象或表达式求值的结果。
```C++
#include <iostream>
#include <vector>

void ProcessVector(std::vector<int>&& rvalueVector) {
    // 使用 std::move 将右值引用转换为右值，以移动资源
    std::vector<int> localVector = std::move(rvalueVector);

    // 在这里，localVector 拥有了 rvalueVector 的资源
    std::cout << "Size of localVector: " << localVector.size() << std::endl;
}

int main() {
    std::vector<int> myVector = {1, 2, 3, 4, 5};

    // 调用 ProcessVector 时，将 myVector 转换为右值引用
    ProcessVector(std::move(myVector));

    // 在这里，myVector 不再拥有资源，因为它已经被移动
    std::cout << "Size of myVector: " << myVector.size() << std::endl;  // 输出 0

    return 0;
}
```
