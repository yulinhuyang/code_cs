
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
    
    int test_item_2()
    {
        #define ASPECT_RATIO 1.653 // 不推荐
        const double AspectRatio = 1.653; // 推荐

        {const char* const authorName = "Scott Meyers";} // 推荐
        const std::string autorName("Scott Meyers"); // 更推荐

        return 0;
    }

第一种是定义常量指针(constant pointers)，由于常量定义式通常被放在头文件内(以便被不同的源码含入)，因此有必要将指针(而不只是指针所指之物)声明为const。string对象通常比其前辈char*-base合宜。

第二种是class专属长量。让它成为class的一个成员(member),让它成为一个static成员.



