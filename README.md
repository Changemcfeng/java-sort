# java-sort
 The classic sorting algorithms
 
/*
	 * 冒泡排序 
	 * 从数组第一个值开始与其后一个值比较，当较大值在后面，两个值交换，再将较大值与后一个值比较，直至数组最后一个值，第一轮比较完毕。
	 * 此时最大值为数组最后一个，再用同样的方法比较，直到数组的倒数第二个值，第二轮结束。 依次循环比较，直至数组剩下最后一个值。
	 */

	public static void bubbleSort(int[] array) {
		if (!ArrayUtils.isEmpty(array)) {
			for (int i = 1; i < array.length - 1; i++) {
				for (int j = 0; j < array.length - i; j++) {
					if (array[j] > array[j + 1]) {
						int temp = array[j];
						array[j] = array[j + 1];
						array[j + 1] = temp;
					}
				}
			}
		}
	}

	/*
	 * 选择排序 
	 * 从数组找到值最小的索引（下标值） 最小值与数组第一个互换值，从数组第二个值开始，找到值最小的索引。
	 * 最小值与数组第二个互换值，依次循环到最后一个值。
	 */

	public static void selectionSort(int[] array) {
		if (!ArrayUtils.isEmpty(array)) {
			for (int i = 0; i < array.length - 1; i++) {
				int min = i;
				for (int j = i + 1; j <= array.length - 1; j++) {
					if (array[min] > array[j]) {
						min = j;
					}
				}
				int temp = array[min];
				array[min] = array[i];
				array[i] = temp;

			}
		}
	}

	/*
	 * 插入排序 
	 * 从数组第二个位值（current）开始与前一个值比较，直至找到比current大的值，这期间每比较一次就将比较值往后移动一位。
	 * 最小值与数组第二个互换值，依次循环到最后一个值。 使用工具类的耗时太长（建议自己单独写个方法）；
	 */

	public static void insertionSort(int[] array) {
		if (!ArrayUtils.isEmpty(array)) {
			for (int i = 1; i < array.length; i++) {
				int current = array[i];
				int j = i - 1;
				while (j >= 0 && current < array[j]) {
					array[j + 1] = array[j];
					j--;
				}
				array[j + 1] = current;
			}
		}
	}

	/*
	 * 希尔排序 
	 * 插入排序的升级版（分组插入排序） 比较相距一定间隔（步长）的元素大小先排序，然后每次比较所用的距离随着算法的进行而减小。
	 * 每趟排序，根据对应的增量（步长），将待排序列分割成若干长度为m 的子序列，分别对各子数组进行直接插入排序。仅增量因子为1
	 * 时，整个序列作为一个数组来处理，数组长度即为整个序列的长度。
	 */
	public static void shellSort(int[] array) {
		if (!ArrayUtils.isEmpty(array)) {
			int step = array.length / 2;
			while (step > 0) {
				for (int i = 0; i < array.length; i++) {
					int current = array[i];
					int j = i - step;
					while (j >= 0 && current < array[j]) {
						array[j + step] = array[j];
						j -= step;
					}
					array[j + step] = current;
				}
				step /= 2;
			}
		}
	}

	/*
	 * 归并排序 
	 * 需要三个指针，头尾各一个，以及分割这个数组的一个指针（mid） 另外需要一个临时数组长度为（hight-low+1）
	 * 通过指针比较，将小的存于临时数组，再将未循环的数依次放进临时数组中 将临时数组复制给参数数组作为一个数组来处理，数组长度即为整个序列的长度。
	 */

	public static void mergeSort(int[] array) {
		if (!ArrayUtils.isEmpty(array)) {
			mergeSortArrays(array, 0, array.length - 1);
		}
	}

	/*
	 * 先拆分成二叉树，再从最下面一步步合并上来
	 */
	private static void mergeSortArrays(int[] array, int low, int hight) {
		int mid = (low + hight) / 2;
		if (low < hight) {
			mergeSortArrays(array, low, mid);
			mergeSortArrays(array, mid + 1, hight);
			mergeArrays(array, low, mid, hight);
		}
	}

	/*
	 * 比较两数组，小的值先放进临时数组，未循环的数组的值也放进临时数组。最后再将临时数组给到其原来的数组。（未循环的数组也是有序）
	 */
	private static void mergeArrays(int[] array, int low, int mid, int hight) {
		int[] temp = new int[hight - low + 1];
		int i = low, j = mid + 1, index = 0;

		while (i <= mid && j <= hight) {
			if (array[i] <= array[j]) {
				temp[index] = array[i];
				i++;
				index++;
			} else {
				temp[index] = array[j];
				j++;
				index++;
			}
		}
		while (i <= mid) {
			temp[index] = array[i];
			i++;
			index++;
		}
		while (j <= hight) {
			temp[index] = array[j];
			j++;
			index++;
		}
		for (int k = 0; k < temp.length; k++) {
			array[low + k] = temp[k];
		}

	}

	/*
	 * 快速排序 
	 * 和归并是反着来的，先排序在拆分。
	 */
	public static void quickSort(int[] array) {
		if (!ArrayUtils.isEmpty(array)) {
			quickSortArrays(array, 0, array.length - 1);
		}
	}

	private static void quickSortArrays(int[] array, int low, int hight) {
		// 判断数组是否查询完毕
		if (low >= hight) {
			return;
		}
		// 临时数字储存关键字（对比字段）
		int temp = array[low];
		// 与关键字对比，先从后往前扫描，遇到比关键字小的，就将这次比较的值填充到指针开始的地方
		// 与关键字对比，从前往后扫描，遇到比关键字大的，就将这次比较的值填充到指针结束的地方
		int i = low, j = hight;
		while (i < j) {
			while (i < j && temp <= array[j]) {
				j--;
			}
			if (i < j) {
				array[i] = array[j];
				i++;
			}
			while (i < j && temp >= array[i]) {
				i++;
			}
			if (i < j) {
				array[j] = array[i];
				j--;
			}

		}
		// 填坑操作最后一步就是把最初那个临时的数字放进最后一个坑
		// 最后弹出来的j(i)就是最后一个坑（i=j）,因为指针每次都是往上一个坑移动，移不动的时候（加一或者减一）就是最后一个坑
		array[j] = temp;
		// 分而治之（将关键字左边的数组循环快排）
		quickSortArrays(array, low, j - 1);
		// 分而治之（将关键字右边的数组循环快排）
		quickSortArrays(array, i + 1, hight);
	}
	
	/* 堆排序
	 * 先将数组排序成大顶堆
	 * 将数组末尾与数组开始（array[0]）交换（既数组最大值在末尾，数组长度减1）
	 * 重新调整数组成大顶堆
	 * 数组末尾与数组开始交换（既数组最大值在末尾，数组长度减1）
	 * 重复此操作直至数组长度为1
	 */
	
	public static void heapSort(int[] array) {
		if (ArrayUtils.isEmpty(array)) {return ;}
		// 大顶堆
		// 从下到上，从右至左，从第一个非叶子节点开始，数组长度减1再除以2作为父节点。
		// 先将左节点（2*父节点+1）与右节点（左节点+1）比较，再将大的一个子节点与父节点比较，也就是选取最大的作为父节点。
		// 再以第二个非叶子节点（前一个节点）作为父节点，与孩子节点比较选取最大的作为父节点。
		// 接着节点往前移（重复与孩子节点比较选取最大的作为父节点，直至节点移完）
		for (int i = (array.length-1)/2; i >=0; i--) {
			heapSortArrays(array,i,array.length);
		}
		//重构大顶堆
		//数组末尾与数组开始（array[0]）交换（既数组最大值在末尾，数组长度减1）
		//将较大的子节点的值给予父节点。将较大的值的索引作为父节点，与其较大的子节点比较，大于父节点，将就较大的值给予父节点，直至该父节点没有子节点。
		for (int i = array.length-1; i >0; i--) {
			int temp=array[i];
			array[i]=array[0];
			array[0]=temp;
			heapSortArrays(array, 0, i);	
		}
		
		
	}
	private static void heapSortArrays(int[] array,int parent,int len) {
		int temp=array[parent];
		int ichild=2*parent+1;
		while(ichild<len) {
			int rchild=ichild+1;
			if (rchild<len && array[rchild]>array[ichild]) {
				ichild++;
			}
			if (array[ichild]<=temp) {
				break;
			}
			array[parent]=array[ichild];
			parent=ichild;
			ichild=2*ichild+1;	
		}
		array[parent]=temp;
	}
	
	/* 计数排序
	 * 生成一个记录数组(长度为原来数组最大值减最小值加1)
	 * 循环原来的数组，在记录数组对应的位置加1
	 * 通过记录数组，将原来数组重新填充
	 */
	
	public static void CountSort(int[] array) {
		//获取最大值(默认数组第一个)，最小值(默认数组第一个)
		if (ArrayUtils.isEmpty(array)) {return;}
		int max=array[0],min=array[0];
		for (int i = 1; i < array.length; i++) {
			if (max<array[i]) {
				max=array[i];
			}
			if (min>array[i]) {
				min=array[i];
			}
		}
		//记录数组（数组的计数）
		int[] temp=new int[max-min+1];
		for (int i = 0; i < array.length; i++) {
			temp[array[i]-min]++;	
		}
		//根据记录数组修改原来的数组
		int index=0;
		for (int i = 0; i < temp.length; i++) {
			while (temp[i]>0) {
				array[index]=i+min;
				temp[i]--;
				index++;
			}
		}
	}
	
	/*
	 * 桶排序
	 */
	
	public static void bucketSort(int[] array) {
		if (ArrayUtils.isEmpty(array)) {return;}
		int max = array[0];
		int min = array[0];
		for(int i = 0; i < array.length; i++){
	       max = Math.max(max, array[i]);
	       min = Math.min(min, array[i]);
		}
		//桶数
		int number=(max-min)/array.length+1;
		//创建n个桶
		ArrayList<ArrayList<Integer>> bucket=new ArrayList<ArrayList<Integer>>();
		for (int i = 0; i < number; i++) {
			bucket.add(new ArrayList<Integer>());
		}
		//每个桶容量为数组长度，往桶里添加数据
		for (int i = 0; i < array.length; i++) {
			bucket.get((array[i] - min) / (array.length)).add(array[i]);
		}
		//对每个桶进行排序
	    for(int i = 0; i < bucket.size(); i++){
	        Collections.sort(bucket.get(i));
	    }
	    //将桶里的数据倒出来。
	    int sub=0;
	    for(int i = 0; i < bucket.size(); i++){
	    	int index=0;
	    	while(index<bucket.get(i).size()) {
	    		array[sub]=bucket.get(i).get(index);
	    		index++;
	    		sub++;
	    	}
	    }
	}
	/*
	 * 基数排序
	 */
	public static void redixSort(int[] array) {
		if (ArrayUtils.isEmpty(array)) {return;}
		//无法排序负数和0
		for (int i = 0; i < array.length; i++) {
			if (array[i]<=0) {
				return;
			}
		}
		//数组最大值
		int max = array[0];
		//数组最小值
		int min = array[0];
		for(int i = 0; i < array.length; i++){
	       max = Math.max(max, array[i]);
	       min = Math.min(min, array[i]);
		}
		ArrayList<ArrayList<Integer>> bucket=new ArrayList<ArrayList<Integer>>();
		//创建0-9序号的桶
		for (int i = 0; i < 10; i++) {
			bucket.add(new ArrayList<Integer>());
		}
		int count=0;
		while (max>0 && (max=max/10)>=0) {
			
			if (count>0) {
				//往桶里倒数据
				for (int i = 0; i < array.length; i++) {
					bucket.get(array[i]/(int)Math.pow(10, count)%10).add(array[i]);
				}
			}
			else { 
				///往桶里倒数据
				for (int i = 0; i < array.length; i++) {
					bucket.get(array[i]%10).add(array[i]);
				}
			}
			int sub=0;
			//将桶里的数据倒出来
		    for(int i = 0; i < bucket.size(); i++){
		    	int index=0;
		    	while(index<bucket.get(i).size()) {
		    		array[sub]=bucket.get(i).get(index);
		    		index++;
		    		sub++;
		    	}
		    }
		     //将桶里的数据清除
		    for(int i = 0; i < bucket.size(); i++){
		    	bucket.get(i).clear();
		    }
		    //个位为0,十位为1，百位为2...
		    count++;
		}
		
		
	}
