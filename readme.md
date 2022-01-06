# TypeScript

### TypeScript 初体验

1. 安装编译 typescript 的工具包
   ```typescript
   npm i -g typescript // tsc -v 查看是否安装成功
   ```
2. 如何运行 ts 代码？

    先 **编译** 再 **执行**，此过程太繁琐，安装 ts-node 包一步到位。
    ```typescript
    npm i -g ts-node // 安装
    ts-node 文件名字 // 编译、执行一步到位
    ```
### TypeScript 常用类型

1. 数组类型
    ```typescript
    let number: number[] = [1, 3, 5]

    // Q：如果数组中既有 number 类型，又有 string 类型呢？
    // A：利用 (|) 联合类型
    let arr: (number | string)[] = [1, 'a', 3, 'b']
    ```

2. 类型别名

    ```typescript
    // 当 某个类型 在多处地方被使用，则需要被重复声明，这样显然是不合理的，这就需要给此类型起一个别名
    // 语法： type 类型别名 = 类型
    type CusTome = (number | string)[]
    let arr1: CusTome = [1, 'a', 3, 'c']
    let arr2: CusTome = [2, 'b', 4, 'd']
    ```

3. 函数类型
    
    函数的类型实际上指的是：**参数** 和 **返回值** 的类型
    
    3.1 有返回值

    ```typescript
    // 1. 参数、返回值单独指定
    function add(num1: number, num2: number): number {
      return num1 + num2
    }

    // 2. 参数、返回值同时指定（只适用于函数作为表达式）
    const add: (num1: number, num2: number) => number = (num1, num2) => {
      return num1 + num2
    }
    // 拆解
    const add = (num1, num2) => {
      return num1 + num2
    }

    : (num1: number, num2: number) => number
    ```
    3.2 无返回值

    ```typescript
    function greet(name: string): void {
      console.log('Hello', name)
    }
    ```
    3.3 可选参数
    ```typescript
    // 注意：可选参数只能出现在参数列表的最后
    function mySlice(start?: number, end?: number): void {
      console.log('起始索引:', start, '结束索引:', end)
    }
    ```