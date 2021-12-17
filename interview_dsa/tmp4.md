

## 【alkf056】1022sjbc[实验室预约系统设计]学习交流

```C++
class BookingSystem {
public:
    struct Node {
        int labId;
        vector<pair<int, int>> times;
        Node(int iLabId) : labId(iLabId) {}
    };
    vector<Node> mAdd;
    map<int, vector<int>> mBook;
    BookingSystem() {}
    int findLad(int labId)
    {
        for (int i = 0; i < mAdd.size(); ++i) {
            if (mAdd[i].labId == labId) {
                return i;
            }
        }
        return -1;
    }
    bool IsCross(pair<int, int> &p, int startTime, int endTime)
    {
        return !(p.first >= endTime || p.second <= startTime);
    }
    void merge(vector<pair<int, int>> &times)
    {
        sort(times.begin(), times.end(), [](pair<int, int> &p1, pair<int, int> &p2) { return p1.first <= p2.first; });
        for (auto iter = times.begin() + 1; iter != times.end();) {
            if (iter->first == (iter - 1)->second) {
                (iter - 1)->second = iter->second;
                times.erase(iter);
            } else {
                iter++;
            }
        }
    }
    bool AddLab(int labId, int startTime, int endTime)
    {
        int pos = findLad(labId);
        if (pos == -1) {
            Node lab(labId);
            lab.times.emplace_back(startTime, endTime);
            mAdd.push_back(lab);
            return true;
        } else {
            Node &lab = mAdd[pos];
            for (auto &p : lab.times) {
                if (IsCross(p, startTime, endTime)) {
                    return false;
                }
            }
            lab.times.emplace_back(startTime, endTime);
            merge(lab.times);
            return true;
        }
    }
    bool InBooking(int labId, int fromTime, int toTime)
    {
        for (auto p : mBook) {
            if (p.second[0] == labId) {
                pair<int, int> tmp { p.second[1], p.second[2] };
                if (IsCross(tmp, fromTime, toTime)) {
                    return true;
                }
            }
        }
        return false;
    }
    int BookTime(int recordId, int fromTime, int toTime)
    {
        for (auto lab : mAdd) {
            for (auto &p : lab.times) {
                // cout << "lab:" << lab.labId << ' ' << p.first << ' ' << p.second << endl;
                // cout << "book:" << fromTime << ' ' << toTime << endl;
                if (p.first <= fromTime && p.second >= toTime && !InBooking(lab.labId, fromTime, toTime)) {
                    vector<int> book { lab.labId, fromTime, toTime };
                    mBook[recordId] = book;
                    return lab.labId;
                }
            }
        }
        return -1;
    }
 
    bool CancelBooking(int recordId)
    {
        for (auto p : mBook) {
            if (recordId == p.first) {
                mBook.erase(recordId);
                return true;
            }
        }
        return false;
    }
};

```


## 【alkf058】1105sjbc[图片表格内容识别转换]学习交流

```C++
#include <algorithm>
#include <iostream>
#include <map>
#include <regex>
#include <sstream>
#include <string>
#include <vector>

using namespace std;

void SplitString(const string& input, char sperChar, vector<string>& outArray);

struct Key {
    unsigned long m_row = 0;
    unsigned long m_col = 0;
};

bool operator<(Key const& k1, Key const& k2)
{
    if (k1.m_col != k2.m_col) {
        return k1.m_col < k2.m_col;
    }
    return k1.m_row < k2.m_row;
}

using ColKey = unsigned long;

class Solution {

public:
    // 待实现函数，在此函数中填入答题代码;
    void TransformationTable(const vector<string>& tableInfo)
    {
        Parse(tableInfo);
        TextOut();
    }

    void Parse(const vector<string>& tableInfo)
    {
        std::for_each(tableInfo.begin(), tableInfo.end(), [this](auto& info) {
            std::regex reg("line(\\w+)[ \t]+col(\\w+)[ \t]*(\\w*)");
            std::smatch smatchs;
            if (std::regex_match(info, smatchs, reg)) {
                enum NUM { ONE = 1, TWO = 2, THREE = 3 };
                auto row = std::stoul(smatchs[ONE]);
                auto col = std::stoul(smatchs[TWO]);
                auto& value = smatchs[THREE];
                SetValue(row, col, value);
                SetWidth(col, value);
                m_maxLine = m_maxLine < row ? row : m_maxLine;
                m_maxCol = m_maxCol < col ? col : m_maxCol;
            }
        });
    }

    void SetValue(unsigned long row, unsigned long col, std::string const& value)
    {
        m_values[Key { row, col }] = value;
    }

    void SetWidth(unsigned long col, std::string const& value)
    {
        auto& width = m_colWidth[col];
        if (width < value.size()) {
            width = value.size();
        }
    }
    
    void TextOut() const
    {
        PrintLine();
        for (unsigned int row = 1; row <= m_maxLine; ++row) {
            std::string line;
            for (unsigned int col = 1; col <= m_maxCol; ++col) {
                auto it = m_colWidth.find(col);
                auto len = it == m_colWidth.end() ? 0 : it->second;
                line += "|";
                auto nlen = len + 2;
                auto vit = m_values.find(Key { row, col });
                std::string value = vit == m_values.end() ? "" : vit->second;
                auto vlen = value.size();
                line += std::string((nlen - vlen) / 2, ' ');
                line += value;
                line += std::string(nlen - vlen - (nlen - vlen) / 2, ' ');
            }
            line += "|\n";
            std::cout << line;
        }
        PrintLine();
    }
    
    void PrintLine() const
    {
        std::string line;
        for (unsigned int col = 1; col <= m_maxCol; ++col) {
            line += "+";
            auto it = m_colWidth.find(col);
            auto len = it == m_colWidth.end() ? 0 : it->second;
            line += std::string(len + 2, '-');
        }
        line += "+\n";
        std::cout << line;
    }

private:
    std::map<ColKey, std::size_t> m_colWidth;
    std::map<Key, std::string> m_values;
    unsigned long m_maxLine = 0;
    unsigned long m_maxCol = 0;
};

// 以下为考题输入输出框架，此部分代码不建议改动
void SplitString(const string& input, char sperChar, vector<string>& outArray)
{
    stringstream sstr(input);
    string token;
    while (getline(sstr, token, sperChar)) {
        outArray.push_back(token);
    }
}

```


## 【alkf060】1119sjbc[令牌桶限速系统]学习交流

```cpp
class RateLimitSystem {

public:

    explicit RateLimitSystem(int tokenLimit) : m_tokenLimt(tokenLimit), m_token(0), m_time(0) {}

    bool AddRule(int ruleId, int time, int interval, int number)
    {
        if (m_rules.find(ruleId) != m_rules.end()) {
            return false;
        }
        Rule rl { time, interval, number };
        m_rules.emplace(make_pair(ruleId, rl));
        m_token += number;
        m_token = min(m_token, m_tokenLimt);
        return true;
    }
    
    bool RemoveRule(int ruleId, int time)
    {
        auto iter = m_rules.find(ruleId);
        if (iter == m_rules.end()) {
            return false;
        }
        m_token += (time - iter->second.time) / iter->second.interval * iter->second.number;
        m_token = min(m_token, m_tokenLimt);
        m_rules.erase(iter);
        return true;
    }

    bool TransferData(int time, int size)
    {
        QueryToken(time);
        if (m_token >= size) {
            m_token -= size;
            return true;
        }
        return false;
    }

    int QueryToken(int time)
    {
        for (auto iter = m_rules.begin(); iter != m_rules.end(); ++iter) {
            int cs = (time - iter->second.time) / iter->second.interval;
            iter->second.time += cs * iter->second.interval;
            m_token += cs * iter->second.number;
        }

        m_token = min(m_token, m_tokenLimt);
        return m_token;
    }

private:
    int m_tokenLimt;
    int m_token;
    int m_time;
    map<int, Rule> m_rules;
};

```
