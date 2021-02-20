
1  efficitive C++ 

**尽量以const, enum, inline替换#define(Prefer consts, enums, and inlines to #defines)**

宁可以编译器替换预处理器

(1). 对于单纯常量，最好以const对象或enum替换#define；(2). 对于形式函数的宏(macro)，最好改用inline函数替换#define。

prefer:

  class GamePlayer {
  
  private:
  
    static const int NumTurns = 5; // 常量声明式
    
    int scores[NumTurns]; // 使用该常量
    
  };
