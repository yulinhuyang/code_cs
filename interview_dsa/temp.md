
##### 238. 除自身以外数组的乘积

```C++
class Solution {
public:
    vector<int> productExceptSelf(vector<int> &nums) {
        vector<int> answer(nums.size(), 0);
        answer[0] = 1;
        for (int i = 1; i < nums.size(); i++) {
            answer[i] = answer[i - 1] * nums[i - 1];
        }
        //左右遍历合并成一
        int Rmul = 1;
        for (int i = nums.size() - 1; i > -1; i--) {
            answer[i] *= Rmul;
            Rmul *= nums[i];
        }

        return answer;
    }
};

```
##### 239 

```C++
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        int m = nums.size();

        //存索引，方便出的判断
        deque<int> deque;
        vector<int> res;
        //处理前K个,保持升序
        for (int i = 0; i < k; i++) {
            while (!deque.empty() && nums[i] >= nums[deque.back()]) {
                deque.pop_back();
            }
            deque.emplace_back(i);
        }
        res.emplace_back(nums[deque.front()]);

        for (int i = k; i < m; i++) {
            //先出
            if (deque.front() == i - k) {
                deque.pop_front();
            }
            //后入并判断
            while (!deque.empty() &&  nums[i] >= nums[deque.back()]) {
                deque.pop_back();
            }

            deque.emplace_back(i);
            res.emplace_back(nums[deque.front()]);
        }

        return  res;
    }
};

```
