# 算法红色第四版\_排序

## 碎片

加上表情包对自己的心情有着正面影响😄，也不会让自己记录的时候那么枯燥。

#### 4/24



## 排序

排序就是将一组对象按照某种逻辑顺序重新排列的过程。

#### 游戏模板

就是面向对象的思想，给排序算法创建一个基类，然后所有其他的排序算法进行继承并且编写属于自己的接口。

### &#x20;选择排序

特点1：运行时间和输入无关--有序数组和随机乱序数组排序的的时间一样长。

特点2：数据移动是最少的--用了N次交换

```
void sort (int *a){
    int len=sizeof(a)
    for(int i=0;i<len;i++){
        //int min=a[i];//思路错了！交换次数增加
        int min=i;
        for(int j=1;j<len<j++){
            if(min[j]<min[i]){//这边我觉得应该加上=<
       /* 这样子写的话交换次数就会是N*N，那么就不符合特点2.
                int temp=min[i];
                min[i]=min[j];
                min[j]=temp;
        */        
                //i=j;//×
                min=j;
            }
        }
        int temp=a[i];//这样交换次数就为N次；
        a[i]=a[min];
        a[min]=temp;
    }
}
```

### 插入排序

当你打斗地主时摸牌的样子哈哈哈哈哈哈。我们插牌很轻松，计算机就不轻松了，你摸一张大王的时候计算机要把所有比大王小的牌全部都右移一次然后腾出最前面的位置，大王就能插入到计算机了，当然了你摸一张3的时候计算机就轻松了。之前（大二下刚开学参加蓝桥杯的时候现在大三下了😥）看插入排序比较潦草。

对于有序数组插入排序能够立即发现每个元素已经在合适的位置上了，因此插入对接近有序的数组十分的高效，也很适合小规模的数组。而选择排序对有序数组就不是很有效。

```
//按照自己的想法来实现排序即使错了，也能够矫正自己对该排序的理解所以自己实现一遍很重要!
//觉知此事要躬行
void sort (int *a){
    int len=sizeof(a);
//    for(int i=0;i++;i<len){//应该从后面到前面！！
    for(int i=1;i<len;i++){
        if(a[i]<a[i-1]){//如果这样的话会不能正确排序操作数
            int j=i-1;
            int x = a[i];
            while(j>-1&&x<a[j]){
                
            }
      }
    }
    
}
```

### 希尔排序(Shell's Sort)

希尔排序是基于插入排序的快速排序算法。由于插入排序对乱序数组效率比较低所以产生了希尔排序。

大规模乱序数组使用插入排序只会交换**相邻**的元素，一点一点的从一端移动到另一端。希尔排序能够加快速度，能交换**不相邻**的元素对数组的局部进行排序。

希尔排序的思想是将打序数组分成若干了小序数组进行排序，使得大序列数组逐渐变的有序，最后使用插入排序对大序列进行排序。

透彻理解希尔排序的性能至今仍然是一项挑战,不过数组越大希尔排序效果越明显。

```
// Shell sort
void shellSort(int array[], int n) {
  // Rearrange elements at each n/2, n/4, n/8, ... intervals
  for (int interval = n / 2; interval > 0; interval /= 2) {
    for (int i = interval; i < n; i += 1) {
      int temp = array[i];
      int j;
      for (j = i; j >= interval && array[j - interval] > temp; j -= interval) {
        array[j] = array[j - interval];
      }
      array[j] = temp;
    }
  }
}
```

### 归并排序(MERGE-SORT)

merge：to combine or make two or more things combine to form a single thing

归并排序：简单的递归排序，将两个有序的数组归并成一个更大的有序数组。

分而治之思想：一个问题被分成多个子问题，每个子问题都是单独解决的，最后，将子问题组合起来形成最终解决方案。

![](<../.gitbook/assets/image (2).png>)![](<../.gitbook/assets/image (1).png>)

先划分在合并，合并的过程就是排序的过程。

#### 分而治之

**Divide**

如果 q 是 p 和 r 的中点，那么我们可以拆分子数组A\[p..r]分成两个数组一个数组M\[p..q]和数组L\[q+1, r]。

**conquer**

我们分别去排序数组M和数组L，如果没有达到我们的基准条件，那么分别devide数组M和数组L,并继续对他们进行排序，直到达到我们的基准条件。

**Combine**

当我们得到两个已经排序的子数组，我们通过创建一个新数组将两个已经排序的子数组组合成一个更大的有序数组。



```
void merge(int arr[],int temparr,int left,int mid,int right)
{
    //标记左半部分第一个未排序元素
    int l_pos = left;
    //标记右半部分第一个未排序元素
    int r_pos =mid+1;
    //临时数组元素
    int pos =left;
    //合并--合并就是排序的过程
    while(l_pos<=mid&&r_pos<=right){
        if(arr[l_pos]<arr[arr[r_pos])
            temparr[pos++]=arr[l_pos++];
        else
            temarr[pos++]=arr[r_pos++];      
    }
    // 合并左半区剩余的元素
    while(l_pos<=mid){
        temparr[pos++]=arr[l_pos++];
    }
    while(r_pos<=right){
        temparr[pos++]=arr[r_pos++];
    }    
     // 把临时数组中合并后的元素复制回原来的数组
    while (left <= right)
    {
        arr[left] = tempArr[left];
        left++;
    }   
}

void mergeSort(int arrr[],int tempArr[],int left,int right){
    if(left<right){
        int mid= (left+right)/2;
        //对左半部分进行归并排序
        mergeSort(arr,tempArr,left,mid);
        //对右半部分进行归并排序
        mergeSort(arr,tempArr,mid+1,right);
        //合并已经排序的部分
        merge(arr,tempArr,left,mid,right);
    }
}
void msort(int arr[], int n)
{
    // 分配一个辅助数组
    int tempArr[n];
    // 调用实际的归并排序
    mergeSort(arr, tempArr, 0, n - 1);
}
```

### 快速排序

```
int partition(int arr[],int low,int high){
    int pivot = arr[high];  //选择一个数字作为比较值，
    int i =(low -1);
    //数组所有的值更pivot值做比较
    for(int j = low; j<high; j++){
        if(arr[j]<=pivot){
            i++;
            swap(&arr[i],&arr[j]);
        }
    }
    swap(&arr[i + 1], &arr[high]);
    return(i+1);
}

quickSort(int arr[],leftmostIndex,rightIndex){
    if (low < high) {
    int pi = partition(array, low, high);
    
    quickSort(array, low, pi - 1);
    
    quickSort(array, pi + 1, high);
  }
}
    
```

### 优先队列

很多应用程序都需要处理有序的元素，但不一定要求它们全部有序，或者是不一定要一次将它们排序。

优先级队列最重要的操作就是删除最大元素和插入元素，所以一定要选择正确的数据结构来实现优先队列。

不管是用有序数组还是无序数组插入和删除操作都是1和N。

但是用堆这个数据结构删除和插入操作时间都是logN，所以用堆这个数据结构能够满足要求。

了解二叉堆是什么，如果父节点是k的画能够通过父节点知道左右子节点，也可以利用子节点确定父节点。这样就能保证最大堆：父节点大于子节点。最小堆反之。

#### 堆排序

了解完全二叉树是什么

堆：需要满足是一个完全二叉树，并且所有父节点都要大于子节点

乱序的完全二叉树要满足所有父节点都要大于子节点就是**heapif过程**

可以用数组表示一个完全二叉树

```
void swap(int *a,int *b){
    int temp=*a;
    *a=*b;
    *b=temp;
}
void heapify(arr[],int n,int i){
    int max=i;
    int left=2i+1;
    int right=2i+2;
    if(left<n&&arr[left]>arr[max])
        max=left;
    if(right<n&&arr[right]>arr[max])
        max=right;  
    if(max != i){
        swap(&arr[i],&arr[max]);
        heapify(arr, n, largest);
    }      
}

void heapSort(int arr[],int n){
    //无序的数组创建成最大堆
    for(int i=n/2-1;i>=0;i--)
        heapify(arr,n,i);
  
//堆排序
    for(int i=n-1;i>=0;i--){
        swap(&arr[0],&arr[i]);
        heapify(arr,i,0);
    }      
}

```

