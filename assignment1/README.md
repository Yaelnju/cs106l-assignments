# Assignment 1: EnrollmentNavigator · 作业 1：EnrollmentNavigator

## Setting up C++ · 配置 C++

Go find and follow the setup instructions in [Assignment Setup](../assignment-setup/README.md) to get your C++ compiler and autograder set up for this assignment.

请按照[作业环境配置](../assignment-setup/README.md)中的说明，为本作业配置 C++ 编译器和自动评分器。

## Overview · 概述

It’s that time of the quarter again; time to use EnrollmentNavigator 🤗 Wootwoot.
One thing everyone realizes in their Stanford career at one point is that they
have to eventually graduate — and so enrolling in classes becomes a strategic
endeavor to maximize the XP towards graduation, while also being able to sleep
more than 4 hours a night!

又到了每学期使用 EnrollmentNavigator 的时候了 🤗 好耶！在斯坦福上学，总有一天你会意识到：自己终究是要毕业的。于是，选课就成了一项策略活儿：既要尽可能多地积累通往毕业的经验值（XP），又要保证每晚能睡上 4 个小时以上！

In this hopefully short assignment, we’re going to use example data from the
ExploreCourses API to figure out which CS classes on ExploreCourses are
offered, and which are not! We’ll be taking advantage of streams, while also exercising initialization and references in C++. Lets jump in ʕ•́ᴥ•̀ʔっ

在这份作业中（希望不会花太久），我们将使用 ExploreCourses API 的示例数据，找出 ExploreCourses 上哪些计算机科学（CS）课程开设了，哪些没有开设。我们会用到流（streams），同时练习 C++ 中的初始化和引用。开始吧 ʕ•́ᴥ•̀ʔっ

There are only two files you should need to care about:

你只需要关注两个文件：

* `main.cpp`: All your code goes here 😀!

  `main.cpp`：你的所有代码都写在这里 😀！

* `utils.cpp`: Contains some utility functions. You'll use functions defined in this file, but you don't otherwise need to modify it.

  `utils.cpp`：包含一些工具函数。你会用到其中定义的函数，但不需要修改这个文件。

## Running your code · 运行代码

To run your code, first you'll need to compile it. Open up a terminal (if you are using VSCode, hit <kbd>Ctrl+\`</kbd> or go to **Terminal > New Terminal** at the top). Then make sure that you are in the `assignment1/` directory and run:

运行代码前，需要先编译。打开终端（如果使用 VSCode，可以按 <kbd>Ctrl+\`</kbd>，或点击顶部菜单中的 **Terminal > New Terminal（终端 > 新建终端）**）。确认当前位于 `assignment1/` 目录，然后运行：

```sh
g++ -std=c++20 main.cpp -o main
```

Assuming that your code compiles without any compiler errors, you can now do:

如果编译没有报错，接下来就可以运行：

```sh
./main
```

which will actually run the `main` function in `main.cpp`. This will execute your code and then run an autograder that will check that your code is correct.

这条命令会运行 `main.cpp` 中的 `main` 函数，先执行你的代码，再运行自动评分器，检查代码是否正确。

As you are following the instructions below, we recommend intermittently compiling/testing with the autograder as a way to make sure you're on the right track!

按照下面的说明完成作业时，建议时不时编译一次，并用自动评分器测试，确认自己的方向正确！

> [!NOTE]
> ### Note for Windows · Windows 注意事项
>
> On Windows, you may need to compile your code using
>
> 在 Windows 上，你可能需要使用以下命令编译代码，才能看到输出：
>
> ```sh
> g++ -static-libstdc++ -std=c++20 main.cpp -o main
> ```
>
> in order to see output. Also, the output executable may be called `main.exe`, in which case you'll run your code with:
>
> 此外，生成的可执行文件可能名为 `main.exe`。此时，请使用以下命令运行代码：
>
> ```sh
> ./main.exe
> ```

## Part 0: Read the code and fill in the `Course` struct · 第 0 部分：阅读代码并补全 `Course` 结构体

1. In this assignment, we'll be using the `Course` struct to represent records pulled from ExploreCourses in C++. Take a look at the (incomplete) definition of the `Course` struct in `main.cpp` and fill in the field definitions. Ultimately, we'll be using streams to generate `Course`s --- remember what types streams deal with?

   在本作业中，我们会用 C++ 的 `Course` 结构体表示从 ExploreCourses 获取的记录。查看 `main.cpp` 中尚未完成的 `Course` 结构体定义，并补全字段定义。最终，我们会使用流来生成 `Course` 对象——还记得流处理的是什么类型的数据吗？

2. Take a look at the `main` function in `main.cpp`, and take special notice of how `courses` is passed into `parse_csv`, `write_courses_offered`, and `write_courses_not_offered`. Think about what these functions are doing. Do you need to change anything in the function definition? Spoiler, you do.

   查看 `main.cpp` 中的 `main` 函数，特别留意 `courses` 是如何传入 `parse_csv`、`write_courses_offered` 和 `write_courses_not_offered` 的。想一想这些函数各自要做什么。你需要修改函数定义中的某些内容吗？提前透露一下：需要。

## Part 1: `parse_csv` · 第 1 部分：`parse_csv`

Check out `courses.csv`, it is a CSV file, with three columns: Title, Number of
Units, and Quarter. Implement `parse_csv` so that, for each line in the csv file, it creates a struct `Course` containing the Title, Number of Units, and Quarter for that line.

查看 `courses.csv`：这是一个 CSV 文件，包含三列：Title（课程名称）、Number of Units（学分）和 Quarter（开课学季）。实现 `parse_csv`，为 CSV 文件中的每条课程记录创建一个 `Course` 结构体对象，包含该行的课程名称、学分和开课学季。

A couple of things you need to think about:

你需要考虑以下问题：

1. How are you going to read in `courses.csv`? Muahahaha, perhaps a stream 😏?

   你打算如何读取 `courses.csv`？嘿嘿，也许可以用流 😏？

2. How will you get each line in the file?

   你要如何获取文件中的每一行？

### Hints · 提示

1. Take a look at the `split` function we provide in `utils.cpp`. It may come in handy!

   看看我们在 `utils.cpp` 中提供的 `split` 函数，它可能会派上用场！

   * Feel free to check out the implementation of `split` and ask us any questions about it – you should be able to reason about it since it’s using a `stringstream`.

     欢迎查看 `split` 的实现，有疑问可以随时问我们。它使用了 `stringstream`，你应该能够理解其中的逻辑。

2. Each **line** is a record! *This is important, so we're saying it again :>)*

   每一**行**就是一条记录！*这点很重要，所以再强调一次 :>)*

3. In CSV files (and specifically in `courses.csv`), the first line is usually a row that defines the column names (a column header row). This line doesn't actually correspond to a `Course`, so you'll need to skip it somehow!

   CSV 文件（包括这里的 `courses.csv`）的第一行通常用于定义列名，也就是表头。这一行并不对应一个 `Course` 对象，因此你需要想办法跳过它！

## Part 2: `write_courses_offered` · 第 2 部分：`write_courses_offered`

Ok. Now you have a populated `courses` vector which has all of the records
of the `courses.csv` file neatly stored in a `Course` struct! You find yourself
interested in only the courses that are offered, right? **A course is considered offered if its Quarter field is not the string `“null”`.** In this function, write out to `“student_output/courses_offered.csv”` all the courses that don’t have
`“null”` in the quarter field.

好了，现在 `courses` 向量中已经存入了 `courses.csv` 的全部课程记录，每条记录都由一个 `Course` 结构体对象表示！接下来，你只想关注开设了的课程，对吧？**如果一门课程的 Quarter 字段不等于字符串 `null`，就认为这门课已开设。** 在这个函数中，将所有 quarter 字段不等于 `null` 的课程写入 `student_output/courses_offered.csv`。

> [!IMPORTANT]
> When writing out to the CSV file, please follow this format:
>
> 写入 CSV 文件时，请遵循以下格式：
>
> ```
> <Title>,<Number of Units>,<Quarter>
> ```
>
> Note that there are **no spaces** between the commas! The autograder will not be happy if this format is not followed!
>
> 注意：**不要在逗号分隔符旁额外添加空格**！格式不符合要求，就无法通过自动评分器的检查！
>
> Also, **make sure to write out the column header row** as the first line in the output. This is the same line you had to skip in `courses.csv` for the previous step!
>
> 此外，**务必将表头写在输出文件的第一行**，也就是上一步读取 `courses.csv` 时需要跳过的那一行！

Once `write_courses_offered` has been called, we expect that all of the offered courses (and consequently all the courses you wrote to the output file) will be removed from the `all_courses` vector. **This means that after this
function runs, `all_courses` should ONLY contain courses that are
not offered!**

调用 `write_courses_offered` 后，所有已开设的课程（也就是写入输出文件的那些课程）都应从 `all_courses` 向量中移除。**这意味着，函数运行结束后，`all_courses` 应当只包含未开设的课程！**

One way to do this is to keep track of the courses that are offered perhaps with another vector and delete them from `all_courses`. Just like in Python and many other languages, it is a bad idea to remove elements from a data structure while you are iterating over it, so you'll probably want to do this *after* you have written all offered courses to file.

一种实现方式是使用另一个向量记录已开设的课程，再将它们从 `all_courses` 中删除。与 Python 和许多其他语言一样，一边遍历数据结构一边删除其中的元素容易出问题，因此最好在将所有已开设课程写入文件*之后*，再执行删除操作。

## Part 3: `write_courses_not_offered` · 第 3 部分：`write_courses_not_offered`

So you’re curious about courses that aren’t offered... In the
`write_courses_not_offered` function, write out to
`“student_output/courses_not_offered.csv”` the courses in
`unlisted_courses`. Remember since you deleted the courses that are
offered in the previous step, `unlisted_courses` trivially contains ONLY courses that are not offered – lucky you. So this step should look really similar to Part 2 except shorter and a *tiny* bit simpler.

接下来，看看那些没有开设的课程吧……在 `write_courses_not_offered` 函数中，将 `unlisted_courses` 中的课程写入 `student_output/courses_not_offered.csv`。别忘了，上一步已经删除了已开设的课程，所以 `unlisted_courses` 中自然只剩下未开设的课程——这下省事了！因此，这一步与第 2 部分十分相似，只是代码更短，也会*稍微*简单一点。

## 🚀 Submission Instructions · 提交说明

After compiling and running, if your autograder looks like this:

编译并运行后，如果自动评分器的结果如下图所示：

![An image showing a terminal window where the autograder has run with all tests passing · 自动评分器运行完毕，所有测试均通过](docs/autograder.png)

then you have finished the assignment! Woot woot.

那么你就完成这次作业了！好耶！

To submit the assignment:

提交作业的方法：

1. Send your code from `main.cpp` to `cs106l-aut2627-staff@lists.stanford.edu` from your Stanford email, with the email subject of `CS106L Assignment 1 Submission`.

   使用你的斯坦福邮箱，将 `main.cpp` 中的代码发送至 `cs106l-aut2627-staff@lists.stanford.edu`，邮件主题填写 `CS106L Assignment 1 Submission`。

Your deliverable should be:

需要提交的文件是：

- `main.cpp`
