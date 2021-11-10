#####  冒泡的三种方法

```C++

//冒泡排序1

void bubbleSort(vector<int> &nums) {
	for (int i = 0; i < nums.size(); i++) {
		// -i是因为每轮上冒一个已有序
		//有序区的长度和排序的轮数是相等的
		for (int j = 0; j < nums.size() - i -1; j++) {
			//j的相邻原始比较，大的向上冒泡
			if (nums[j] > nums[j + 1]) {
				swap(nums[j], nums[j + 1]);
			}
		}
	}
}

//冒泡排序2

void bubbleSort2(vector<int> &nums) {

	for (int i = 0; i < nums.size(); i++) {
		bool isSorted = true;
		for (int j = 0; j < nums.size() - i - 1; j++) {
			//j的相邻原始比较向上冒泡
			if (nums[j] > nums[j + 1]) {
				swap(nums[j], nums[j + 1]);
				isSorted = false;
			}
		}
		if (isSorted) {
			break;
		}
	}
}

//冒泡排序3
void bubbleSort(vector<int> &nums) {

	int sortBorder = nums.size() - 1;
	int lastExchangeIndex = 0;
	for (int i = 0; i < nums.size(); i++) {
		bool isSorted = true;
		for (int j = 0; j < sortBorder; j++) {
			//j的相邻原始比较向上冒泡
			if (nums[j] > nums[j + 1]) {
				swap(nums[j], nums[j + 1]);
				isSorted = false;
				//设置有序边界,扩大有序区长度
				lastExchangeIndex = j;
			}
		}
		if (isSorted) {
			break;
		}
		sortBorder = lastExchangeIndex;
	}
}

```
