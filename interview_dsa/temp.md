##### Leetcode 150 逆波兰表达式求值

```C++
class Solution {
public:
    int evalRPN(vector<string> &tokens) {
        stack<int> stk;
        for (auto &token:tokens) {
            if (token == "+" || token == "-" || token == "*" || token == "/") {
                auto b = stk.top();
                stk.pop();
                auto a = stk.top();
                stk.pop();
                if (token == "+") a += b;
                else if (token == "-") a -= b;
                else if (token == "*") a *= b;
                else a /= b;
                stk.push(a);
            } else {
                stk.push(stoi(token));
            }
        }
        return stk.top();
    }
};
```














