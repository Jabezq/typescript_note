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

3. 处理报错

    当运行 tsc-node 报错时，需要有 tsconfig.json 配置文件，命令：tsc -init

### TypeScript 常用类型

1. 数组类型
    ```typescript
    let number: number[] = [1, 3, 5]

    // Q：如果数组中既有 number 类型，又有 string 类型呢？
    // A：利用 (|) 联合类型
    let arr: (number | string)[] = [1, 'a', 3, 'b']
    ```

2. 类型别名

    优点：可以为任意类型指定别名

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

4. 对象类型

    4.1 基本写法

    ```typescript
    let person: {name: string; age: number; sayHi(word: string): void} = {
      name: 'jack',
      age: 19,
      sayHi(word) {}
    }
    ```

    4.2 可选属性

    ```typescript
    // 例如 axios 的 method 属性默认是 GET 请求，所以发送 GET 请求时，method 属性非必要填写
    function myAxios(config: {url: string; method?: string}) {
      console.log(config)
    }
    ```

5. 接口类型

    只能为对象指定类型

    5.1 基本语法

    ```typescript
    // 当一个对象类型被多次使用时，通过接口（interface）来复用
    interface IPerson {
      name: string
      age: number
      sayHi(): void
    }

    let person: IPerson = {
      name: 'jack',
      age: 19,
      sayHi() {}
    }
    ```

    5.2 接口继承
    ```typescript
    interface Point2D {x: number; y: number}
    interface Point3D {x: number; y: number; z: number}

    // Point3D 和 Point2D 有共用的属性或者方法，使用继承达到复用的目的
    interface Point2D {x: number; y: number}
    interface Point3D extends Point2D {z: number}
    ```

6. 元组类型

    元组类型是另一种类型的数组，它确切地知道包含多少个元素，以及特定索引对应的类型

    ```typescript
    // 场景：坐标轴是由两个数值组成的，如果使用数组则显得不够严谨，而元组类型能够指定含有多少个元素和每个元素的类型
    let position: [number, number] = [39.5427, 116.2317]
    ```

7. 类型断言

    ```typescript
    // 更明确地指定某类型时候，就可以使用类型断言
    // 例如下面获取 a 链接标签元素使用了类型断言
    const aLink = getElementById('link') as HTMLAnchorElement

    // 在属性列表最后能够查看此元素是 HTMLElement 的哪个子类型
    console.dir($0) // $0 代表选择的元素
    ```

8. 字面量类型

    数组类型的严谨方式是元组类型，而字面量类型也可以弥补其他类型不够严谨的缺陷
    使用模式：字面量类型配合联合类型一起使用
    使用场景：用来表示一组明确的可选值列表

    ```typescript
    // 场景：游戏控制中分 上、下、左、右四个方向其中一个
    // 参数 direction 只能是四个中其中一个，相对于 string 类型更加严谨
    function changeDirection(direction: 'up' | 'down' | 'left' |'right') {
      console.log(direction)
    }
    ```

9. 枚举类型

    枚举的功能类似于 字面量类型 + 联合类型，用来表示一组明确的可选值

    注意：枚举成员是有值的，默认为从0开始自增的数值，也可以给枚举中的成员初始化值。枚举与其他类型不同，会被编译成 js 代码，而其他类型在被编译成 js 代码时，会去除类型的名字。
    ![](https://img-blog.csdnimg.cn/9e7a5140daa94447a851a14f2e961c55.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBASmFiZXpx,size_20,color_FFFFFF,t_70,g_se,x_16)
    
    数字枚举：枚举成员的值为数字

    字符串枚举：枚举成员的值为字符串

    ```typescript
    // 定义一组命名常量，描述一个值且是这些命名常量中的其中一个
    enum Direction {Up, Down, Left, Right}

    function changeDirection(direction: Direction) {
      console.log(direction)
    }

    // 如何访问枚举成员？
    // 类似 js 中的对象，通过 (.)
    changeDirection(Direction.Up)
    ```

10. typeof 操作符

    在 **类型上下文** (冒号后面，既加 类型注解 的位置)中引用变量或属性的类型

    使用场景：根据已有变量的值，获取该值的类型来简化类型书写

    ```typescript
    let p = {x: 1, y: 2}

    function formatPoint(point: {x: number; y: number}) {}

    // 利用 typeof 操作符简化如下
    function formatPointe(point: typeof p) {}
    ```
### TypeScript 高级类型

1. 类成员的可见性修饰符

    ```typescript
    // 1. public: 表示公有的

    // 2. protected: 仅对其声明所在类和子类中可见，无论对当前类还是子类的实例化对象都不可见
    class Animal {
      protected move() { console.log('Moving along!') }
    }

    const animal = new Animal() // 无法访问

    class Dog extends Animal {
      bark() {
        console.log('汪!')
        this.move() // 可以调用
      }
    }

    const dog = new Dog() // 无法访问

    // 3. private: 只在当前类中可见，对实例对象以及子类也是不可见的
    ```

2. 泛型

    泛型就是为了让函数更强大，让它适用于多种数据类型，例如一个函数的功能是输入什么就输出什么，包括了数值、字符串等类型，但为了不使用 any 类型来注释，于是出现了泛型。

    2.1 创建泛型
    
    ```typescript
    function id<Type>(value: Type): Type {return value}

    // 解释：此处的 Type 叫做 类型变量，专门 捕获、处理 类型，因为 Type 是类型，所以将其作为函数参数和返回值的类型，就实现了输入什么类型就输出什么类型的目的

    // 如何调用？
    // 下行代码代表以 number 类型调用泛型函数，也可以利用 TypeScript 的类型推断机制不指定类型
    id<number>(10)
    
    // 简化
    id(10)
    ```