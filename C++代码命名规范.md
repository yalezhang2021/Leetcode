# C++代码命名规范

一般而言，公认的最好的是google C++命名规范。



1. ### 通用命名规则

   ```c++
   int num_errors;
   int price_count_reader;
   ```

   

2. ### 文件命名 

   全部要小写，用下划线连接

   ```
   my_useful_class.cc
   manager.h
   ```



3. ### 类命名

   每个单词首字母都大写，不要包含下划线

   ```C++
   class UrlTable { ...
   class Student { ...
   sturct UrlTableProperties { ...
   // 类型定义
   typedef hash_map<UrlTableProperties *, string> PropertiesMap;
   
   // using 别名
   using PropertiesMap = hash_map<UrlTableProperties *, string>;
   
   // 枚举
   enum UrlTableErrors { ...
   ```

   

4. ### 变量命名

   **普通变量命名** 
   举例:

   ```
   string table_name;  // 好 - 用下划线.
   ```

   

    **类数据成员** -- 和普通变量名类似，在最后面加下划线。成员变量后加 _

   ```
   class TableInfo {
     ...
    private:
     string table_name_;  // 好 - 后加下划线.
     string tablename_;   // 好.
     static Pool<TableInfo>* pool_;  // 好.
   };
   ```

   **常见的公司中变量名** 都是第一个单词小写加第二个单词首字母大写的方式

   ```
   int rightBorder;
   int maxNum;
   ```

   

5. ### 结构体变量

   不管是静态的还是非静态的，结构体数据成员都可以和普通变量一样，不用像类那样接下划线。

   ```C++
   struct UrlTableProperties {
   	string name;
   	int num_entries;
   	static Pool<UrlTableProperties>* pool;
   };
   ```

   

6. ### 常量命名

   声明为`constexpr` 或`const`的变量，或在程序运行期间其值始终保持不变的，**命名时以 “k" 开头，大小写混合**：

   ```C++
   const int kDaysInAWeek = 7;
   ```



7. ### 函数命名

   常规函数使用大小写混合, 取值和设值函数则要求与变量名匹配: 

   `MyExcitingFunction()`, 

   `MyExcitingMethod()`, 

   `my_exciting_member_variable()`, 

   `set_my_exciting_member_variable()`.

   一般来说, 函数名的每个单词首字母大写 (即 “驼峰变量名” 或 “帕斯卡变量名”), 没有下划线. 对于首字母缩写的单词, 更倾向于将它们视作一个单词进行首字母大写 (例如, 写作 `StartRpc()` 而非 `StartRPC()`).

   ```
   AddTableEntry()
   DeleteUrl()
   OpenFileOrDie()
   ```

   **取值和设值函数的命名与变量一致.** 一般来说它们的名称与实际的成员变量对应, 但并不强制要求. 例如 

   `int count()` 

   `void set_count(int count)`.



8. ### 命名空间命名

   命名空间以小写字母命名. 最高级命名空间的名字取决于项目名称. 要注意避免嵌套命名空间的名字之间和常见的顶级命名空间的名字之间发生冲突.

   顶级命名空间的名称应当是项目名或者是该命名空间中的代码所属的团队的名字. 命名空间中的代码, 应当存放于和命名空间的名字匹配的文件夹或其子文件夹中.

   注意 [不使用缩写作为名称](https://zh-google-styleguide.readthedocs.io/en/latest/google-cpp-styleguide/naming/#general-naming-rules) 的规则同样适用于命名空间. 命名空间中的代码极少需要涉及命名空间的名称, 因此没有必要在命名空间中使用缩写.

   要避免嵌套的命名空间与常见的顶级命名空间发生名称冲突. 由于名称查找规则的存在, 命名空间之间的冲突完全有可能导致编译失败. 尤其是, 不要创建嵌套的 `std` 命名空间. 建议使用更独特的项目标识符 (`websearch::index`, `websearch::index_util`) 而非常见的极易发生冲突的名称 (比如 `websearch::util`).

   对于 `internal` 命名空间, 要当心加入到同一 `internal` 命名空间的代码之间发生冲突 (由于内部维护人员通常来自同一团队, 因此常有可能导致冲突). 在这种情况下, 请使用文件名以使得内部名称独一无二 (例如对于 `frobber.h`, 使用 `websearch::index::frobber_internal`).



9. ### 枚举命名

   枚举的命名应当和 [常量](https://zh-google-styleguide.readthedocs.io/en/latest/google-cpp-styleguide/naming/#constant-names) 或 [宏](https://zh-google-styleguide.readthedocs.io/en/latest/google-cpp-styleguide/naming/#macro-names) 一致: `kEnumName` 或是 `ENUM_NAME`.

   **说明**

   单独的枚举值应该优先采用 [常量](https://zh-google-styleguide.readthedocs.io/en/latest/google-cpp-styleguide/naming/#constant-names) 的命名方式. 但 [宏](https://zh-google-styleguide.readthedocs.io/en/latest/google-cpp-styleguide/naming/#macro-names) 方式的命名也可以接受. 枚举名 `UrlTableErrors` (以及 `AlternateUrlTableErrors`) 是类型, 所以要用大小写混合的方式.

   ```
   enum UrlTableErrors {
       kOK = 0,
       kErrorOutOfMemory,
       kErrorMalformedInput,
   };
   enum AlternateUrlTableErrors {
       OK = 0,
       OUT_OF_MEMORY = 1,
       MALFORMED_INPUT = 2,
   };
   ```

   2009 年 1 月之前, 我们一直建议采用 [宏](https://zh-google-styleguide.readthedocs.io/en/latest/google-cpp-styleguide/naming/#macro-names) 的方式命名枚举值. 由于枚举值和宏之间的命名冲突, 直接导致了很多问题. 由此, 这里改为优先选择常量风格的命名方式. **新代码应该尽可能优先使用常量风格**. 但是老代码没必要切换到常量风格, 除非宏风格确实会产生编译期问题.

   

10. ### 宏命名

    你并不打算 [使用宏](https://zh-google-styleguide.readthedocs.io/en/latest/google-cpp-styleguide/others/#preprocessor-macros), 对吧? 如果你一定要用, 像这样命名: `MY_MACRO_THAT_SCARES_SMALL_CHILDREN`.

    **说明**

    参考 [预处理宏](https://zh-google-styleguide.readthedocs.io/en/latest/google-cpp-styleguide/others/#preprocessor-macros); 通常 *不应该* 使用宏. 如果不得不用, 其命名像枚举命名一样全部大写, 使用下划线:

    ```
    #define ROUND(x) ...
    #define PI_ROUNDED 3.0
    ```