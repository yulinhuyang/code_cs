##### 152 乘积最大子数组

```C++

class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int n = nums.size();
        vector<int> dpMax(n,0);
        vector<int> dpMin(n,0);
        dpMax[0] = nums[0];
        dpMin[0] = nums[0];
        int ans = nums[0];
        for(int i = 1;i < n;i++){
            dpMax[i] = max(nums[i],max(dpMax[i-1]*nums[i],dpMin[i-1]*nums[i]));
            dpMin[i] = min(nums[i],min(dpMax[i-1]*nums[i],dpMin[i-1]*nums[i]));
            ans = max(ans,dpMax[i]);
        }
        return  ans;
    }
};

```

#### 169 多数元素

```C++
class Solution {
public:
    int majorityElement(vector<int>& nums) {

        //投票法
        int count = 1;
        int conda = nums[0];
        for(int i = 1;i < nums.size();i++){
            if(count == 0){
                conda = nums[i];
            }
            if(conda == nums[i]){
                count++;
            } else{
                count--;
            }
        }
        return conda;
    }
};

```
#### 198 打家劫舍

```C++
class Solution {
public:
    int rob(vector<int>& nums) {

        //dp array -->简化
        int dp_i = 0;
        int dp_i_1 = 0;
        int dp_i_2 = 0;
        for(int i = 0;i < nums.size();i++){
            dp_i = max(dp_i_1,dp_i_2 + nums[i]);
            dp_i_2 = dp_i_1;
            dp_i_1 = dp_i;
        }
        return  dp_i;
    }
};

```



