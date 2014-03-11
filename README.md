anonymous_callbacks_in_tests
============================

I needed a quick and dirty library for writing (almost) anonymous callbacks in C. This is it.

When writing callback support in [accel](https://github.com/shalecraig/accel), I realized that I'd
be writing a large number of tests which use callbacks.

In form with [GoogleTest](https://code.google.com/p/googletest/), I needed a macro to generate test
functions with specific namespaces.

This came from it.


Here's an **example**:

```
/* mytest.c */

#include <test_callback_util.h>

TEST_CALLBACK(int, FooTest, CallbackCalled, callback1, int foo, int bar)
    EXPECT_EQ(0, bar);
    return bar + foo;
}

TEST(FooTest, CallbackCalled) {
    int result = foo(&TEST_CALLBACK_NAME(FooTest, CallbackCalled, callback1));
    ASSERT_EQ(0, result);
    ASSERT_EQ(1, TEST_CALLBACK_COUNTER(FooTest, CallbackCalled, callback1));
}
```
