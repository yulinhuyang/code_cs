##### 1. 两数之和

```c++
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int,int> dict;
        for(int i = 0;i < nums.size();i++){
            int tmp = target - nums[i];
			//map.find(key) != map.end()
			//map.count(key) > 0
            if(dict.count(tmp) > 0){
                return {dict[tmp],i}; //返回vector
            }
            dict[nums[i]] = i;
        }
        return  {};
    }
```

##### 2. 两数相加

```python
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        //nullptr类型
        ListNode *head = nullptr;
        ListNode *tail = nullptr;
        int carry = 0;
        while(l1 || l2){
            int l1_val = l1?l1->val:0;
            int l2_val = l2?l2->val:0;

            int sum = l1_val + l2_val + carry;
            carry = sum/10;
            if(!head){
                //理解new的返回类型
                tail = head = new ListNode(sum%10);
            }else{
                tail->next = new ListNode(sum%10);
                tail = tail->next;
            }
            if(l1){
                l1 = l1->next;
            }
            if(l2){
                l2 = l2->next;
            }
        }
        if(carry > 0){
            tail->next = new ListNode(carry);
        }

        return head;
    }
};

```