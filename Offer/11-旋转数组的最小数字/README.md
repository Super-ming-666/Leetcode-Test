# 11. 旋转数组的最小数字

把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个递增排序的数组的一个旋转，输出旋转数组的最小元素。例如，数组 [3,4,5,1,2] 为 [1,2,3,4,5] 的一个旋转，该数组的最小值为1。  

示例 1：

```
输入：[3,4,5,1,2]
输出：1
```


示例 2：

```
输入：[2,2,2,0,1]
输出：0
```





## 解析

数组输出最小数字，从头到尾遍历一遍，就能找出最小元素，是很直观的想法。但这并不符合出题者的意思。

最好根据旋转数组的特性来寻找本题的解决方案，同时处理几个特殊情况。

解决方案：模仿二分查找，提升查找效率。



## 题解

## C

```c
int minArray(int* numbers, int numbersSize){
    //二分查找
    if(numbersSize <= 0) 
        return -1;
    
    int index1 = 0;
    int index2 = numbersSize - 1;
    int indexMiddle = index1; //若仍为有序数组，返回第一个。
    if(numbersSize == 1)
        return numbers[index1];
    if(numbers[index1] == numbers[index2] && numbers[indexMiddle] == numbers[index1]){
        return MinInOrder(numbers, index1, index2);
    }

    while(numbers[index1] > numbers[index2]){
        if(index2 - index1 == 1){
            indexMiddle =index2;
            break;
        }

        indexMiddle = (index1 + index2) / 2;
        if(numbers[indexMiddle] >= numbers[index1])
            index1 = indexMiddle;
        else if(numbers[indexMiddle] <= numbers[index2])
            index2 = indexMiddle;
    }

    return numbers[indexMiddle];
}

int MinInOrder(int* numbers, int index1, int index2){
    int result = numbers[index1];
    for(int i = index1 + 1; i <= index2; i++){
        if(result > numbers[i]){
            result = numbers[i];
        }
    }
    return result;
}
```



利用两个递增的特性，只要找到不是递增处即可，可以减少很多代码量。【但还是推荐上者】

```java
int minArray(int* numbers, int numbersSize){
    for(int i = 0; i < numbersSize - 1; i++){
        int j = i + 1;
        if(numbers[i] > numbers[j])
            return numbers[j];
    }
    return numbers[0];
}
```



