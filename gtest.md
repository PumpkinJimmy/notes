## GTest框架
## QuickStart
```cpp
#include "List.h"
#include "gtest/gtest.h"
using namespace std;
using ds::List;
// 设置测试夹具SetUp/TearDown
class ListTest: public testing::Test{
protected:
    void SetUp() override {
        l1.assign({3, 1, 2, 4, 5});
    }
    List<int> l1;
};
// 相比TEST宏，TEST_F可以设置SetUp/TearDown
// TEST_F(TestSuiteName, TestName)
TEST_F(ListTest, MergeSort){
    l1.merge_sort();
    auto p = l1.begin();
    EXPECT_EQ(p->record, 1);
    p = p->next;
    EXPECT_EQ(p->record, 2);
    p = p->next;
    EXPECT_EQ(p->record, 3);
    p = p->next;
    EXPECT_EQ(p->record, 4);
    p = p->next;
    EXPECT_EQ(p->record, 5);
    ASSERT_EQ(p->next, nullptr);
    // ASSERT_*系列宏测试失败会中断test；EXPECT_*系列不会
}
int main(int argc, char** argv)
{
    ::testing::InitGoogleTest(&argc, argv);
    return RUN_ALL_TESTS();
}
```