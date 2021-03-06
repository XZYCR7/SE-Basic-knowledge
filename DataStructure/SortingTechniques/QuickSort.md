# 概述
快速排序是一种高效的排序算法，它基于将数据阵列划分为更小的数组。一个大型数组被分成两个数组，其中一个数组的值小于指定的值，比如pivot，根据该数组创建分区，另一个数组保存的值大于数据透视值。

快速排序对数组进行分区，然后递归调用两次以对两个结果子数组进行排序。该算法对于大尺寸数据集非常有效，因为其平均和最差情况复杂度为0（n 2），其中n是项目数。

# 快速排序中的分区
以下动画表示解释了如何在数组中查找透视值。

![](./images/quick_sort_partition_animation.gif)

数据透视值将列表分为两部分。并且递归地，我们找到每个子列表的轴，直到所有列表只包含一个元素。

# 快速排序枢轴算法
基于我们对快速排序中的分区的理解，我们现在将尝试为其编写算法，如下所示。

```
Step 1 − Choose the highest index value has pivot
Step 2 − Take two variables to point left and right of the list excluding pivot
Step 3 − left points to the low index
Step 4 − right points to the high
Step 5 − while value at left is less than pivot move right
Step 6 − while value at right is greater than pivot move left
Step 7 − if both step 5 and step 6 does not match swap left and right
Step 8 − if left ≥ right, the point where they met is new pivot
```

# 伪代码
```
function partitionFunc(left, right, pivot)
   leftPointer = left
   rightPointer = right - 1

   while True do
      while A[++leftPointer] < pivot do
         //do-nothing            
      end while
		
      while rightPointer > 0 && A[--rightPointer] > pivot do
         //do-nothing         
      end while
		
      if leftPointer >= rightPointer
         break
      else                
         swap leftPointer,rightPointer
      end if
		
   end while 
	
   swap leftPointer,right
   return leftPointer
	
end function

```

# 快速排序算法
递归地使用pivot算法，我们最终得到了更小的可能分区。然后处理每个分区以进行快速排序。我们为quicksort定义递归算法如下 -

```
Step 1 − Make the right-most index value pivot
Step 2 − partition the array using pivot value
Step 3 − quicksort left partition recursively
Step 4 − quicksort right partition recursively

```

# 快速排序伪代码
```
procedure quickSort(left, right)

   if right-left <= 0
      return
   else     
      pivot = A[right]
      partition = partitionFunc(left, right, pivot)
      quickSort(left,partition-1)
      quickSort(partition+1,right)    
   end if		
   
end procedure

```

# C代码如下
```
#include <stdio.h>
#include <stdbool.h>

#define MAX 7

int intArray[MAX] = {4,6,3,2,1,9,7};

void printline(int count) {
   int i;
	
   for(i = 0;i < count-1;i++) {
      printf("=");
   }
	
   printf("=\n");
}

void display() {
   int i;
   printf("[");
	
   // navigate through all items 
   for(i = 0;i < MAX;i++) {
      printf("%d ",intArray[i]);
   }
	
   printf("]\n");
}

void swap(int num1, int num2) {
   int temp = intArray[num1];
   intArray[num1] = intArray[num2];
   intArray[num2] = temp;
}

int partition(int left, int right, int pivot) {
   int leftPointer = left -1;
   int rightPointer = right;

   while(true) {
      while(intArray[++leftPointer] < pivot) {
         //do nothing
      }
		
      while(rightPointer > 0 && intArray[--rightPointer] > pivot) {
         //do nothing
      }

      if(leftPointer >= rightPointer) {
         break;
      } else {
         printf(" item swapped :%d,%d\n", intArray[leftPointer],intArray[rightPointer]);
         swap(leftPointer,rightPointer);
      }
   }
	
   printf(" pivot swapped :%d,%d\n", intArray[leftPointer],intArray[right]);
   swap(leftPointer,right);
   printf("Updated Array: "); 
   display();
   return leftPointer;
}

void quickSort(int left, int right) {
   if(right-left <= 0) {
      return;   
   } else {
      int pivot = intArray[right];
      int partitionPoint = partition(left, right, pivot);
      quickSort(left,partitionPoint-1);
      quickSort(partitionPoint+1,right);
   }        
}

int main() {
   printf("Input Array: ");
   display();
   printline(50);
   quickSort(0,MAX-1);
   printf("Output Array: ");
   display();
   printline(50);
}

```

# 输出
```
Input Array: [4 6 3 2 1 9 7 ]
==================================================
 pivot swapped :9,7
Updated Array: [4 6 3 2 1 7 9 ]
 pivot swapped :4,1
Updated Array: [1 6 3 2 4 7 9 ]
 item swapped :6,2
 pivot swapped :6,4
Updated Array: [1 2 3 4 6 7 9 ]
 pivot swapped :3,3
Updated Array: [1 2 3 4 6 7 9 ]
Output Array: [1 2 3 4 6 7 9 ]
==================================================

```

# 总结
java代码如下
```

public class main {
    public int[] sort(int[] sourceArray){
        // 对arr进行拷贝
        int[] arr = Arrays.copyOf(sourceArray, sourceArray.length);
        return quickSort(arr, 0, arr.length - 1);
    }
    // 进行排序
    private int[] quickSort(int[] arr, int left, int right){
        if(left < right){
            // 对整个数组进行排序,先选取基准数，比基准小的放在前面，比基准大的放在后面
            // 在分完区以后，确保基准数位于中间位置
            int partitionIndex = partition(arr, left, right);
            // 左边排序
            quickSort(arr, left, partitionIndex - 1);
            // 右边再次进行排序
            quickSort(arr, partitionIndex + 1, right);
        }
        return arr;
    }
    // 分区操作
    private int partition(int[] arr, int left, int right){
        // 设定基准值,其中初始化基准值为left的第一个值
        // index的值为遍历的值
        int pivot = left;
        int index = pivot + 1;
        // 从当前的初始值进行遍历操作
        for(int i = index; i <= right; i++){
            if(arr[i] < arr[pivot]){
                swap(arr, i, index);
                index++;
            }
        }
        // 基准数移动到中间
        swap(arr, pivot, index - 1);
        return index - 1;
    }
    // 进行交换
    private void swap(int[] arr, int i, int j){
        int tmp = arr[i];
        arr[i] = arr[j];
        arr[j] = tmp;
    }
}

```

总结，使用递归操作，对整个数组进行排序，需要先选取基准数，然后同理递归左边，递归右边排序。
最重要的是基准数的选择，需要设定两个指针，一个基准值，一个用于移动的index的值。
然后对于任何比基准值小的数，放入index指向的值，并把index进行加加
最后把基准值交换到最后一个index的值。
返回基准值所在的下标等待再次进行递归。

最重要的是选取基准数，此为快排灵魂。