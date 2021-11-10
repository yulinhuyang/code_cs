##### 215. 数组中的第K个最大元素


```C++
class Solution {
public:
    int partition(vector<int> &nums, int low, int high) {
        //gen random index
        int index = (rand()%(high - low + 1))+1;
        swap(nums[low],nums[index]);

        int pivot = nums[low];
        int i = low;
        int j = high;
        while (i < j) {
            //i在大于基准数的地方停下，j在小于基准数的地方停下，如果i先走，最后停下跟基准数交换时，总是大于基准数的
            //j先走
            while (i < j && nums[j] >= pivot) {
                j--;
            }
            while (i < j && nums[i] <= pivot) {
                i++;
            }
            swap(nums[i], nums[j]);
        }
        swap(nums[low], nums[i]);
        return i;
    }

    void quickSort(vector<int> &nums, int start, int end) {
        if (start >= end) {
            return;
        }
        int index = partition(nums, start, end);
        quickSort(nums, start, index - 1);
        quickSort(nums, index + 1, end);
    }

    int findKthLargest(vector<int> &nums, int k) {

        quickSort(nums, 0, nums.size() - 1);
        return nums[nums.size() - k - 1];
    }
};
```
