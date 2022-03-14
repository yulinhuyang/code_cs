##### 127 单词接龙

双向BFS理解每次各自扩展一层的含义
```C++
class Solution {
public:
    int ladderLength(string beginWord, string endWord, vector<string> &wordList) {
        unordered_set<string> Sets;
        for (auto &word:wordList) {
            Sets.emplace(word);
        }
        if(!Sets.count(endWord))
            return 0;
        queue<string> qBegin;
        qBegin.emplace(beginWord);
        unordered_map<string, int> distBegin;
        distBegin[beginWord] = 0;

        queue<string> qEnd;
        qEnd.emplace(endWord);
        unordered_map<string, int> distEnd;
        distEnd[endWord] = 0;

        while (qBegin.size() && qEnd.size()) {
            int len1 = qBegin.size();
            for (int k = 0; k < len1; k++) {
                auto word = qBegin.front();
                qBegin.pop();
                if (distEnd.count(word)) {
                    return distBegin[word] + distEnd[word] + 1;
                }
                for (int i = 0; i < word.size(); i++) {
                    auto tmp = word;
                    for (int j = 'a'; j <= 'z'; j++) {
                        tmp[i] = j;
                        if (Sets.count(tmp) && !distBegin.count(tmp)) {
                            distBegin[tmp] = distBegin[word] + 1;
                            qBegin.emplace(tmp);
                        }
                    }
                }
            }

            int len2 = qEnd.size();
            for (int k = 0; k < len2; k++) {
                auto wordEnd = qEnd.front();
                qEnd.pop();
                if (distBegin.count(wordEnd)) {
                    return distBegin[wordEnd] + distEnd[wordEnd] + 1;
                }
                for (int i = 0; i < wordEnd.size(); i++) {
                    auto tmp = wordEnd;
                    for (int j = 'a'; j <= 'z'; j++) {
                        tmp[i] = j;
                        if (Sets.count(tmp) && !distEnd.count(tmp)) {
                            distEnd[tmp] = distEnd[wordEnd] + 1;
                            qEnd.emplace(tmp);
                        }
                    }
                }
            }

        }
        return 0;
    }
};
```
