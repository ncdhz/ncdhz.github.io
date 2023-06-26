# TypeScript 中有意思的规范

1. 类型
    + 联合类型

        ```ts
        let stringAndNumber: string | number;
        function stringAndNumber(param: string | number):string | number {}
        ```

2. 接口
    + 可选类型

        ```ts
        interface Person {
            age?: number;
        }
        ```

    + 任意属性

        ```ts
        interface Person {
            [propName: string]: string;
        }
        // 注意：任意属性必须是可选类型的父类
        // 错误用法
        interface Person {
            age?: number;
            [propName: string]: string;
        }
        // 正确用法
        interface Person {
            age?: number;
            [propName: string]: string | number;
        }
        ```

    + 只读属性（除了初始化以为不许第二次写入）

        ```ts
        interface Person {
            readonly id: number;
        }
        ```

3. 数组类型

    ```ts
    let numbers: number[] = [1, 1, 2, 3, 5];
    let numbers: Array<number> = [1, 1, 2, 3, 5];
    ```

    + 接口数组

        ```ts
        interface NumberArray {
            [index: number]: number;
        }
        let numbers: NumberArray = [1, 1, 2, 3, 5];
        ```

    + 常用的类数组

        ```ts
        interface IArguments {
            [index: number]: any;
            length: number;
            callee: Function;
        }
        ```

    + 任何类型

        ```ts
        let list: any[] = ['string', 25, { html: 'www.github.com/ncdhz' }];
        ```

4. 函数类型
    + 类型规范

        ```ts
        let mySum: (x: number, y: number) => number = function (x: number, y: number): number {
            return x + y;
        };
        
        /**
        * 其中 (x: number, y: number) => number 是类型定义。表示：x参数是数字，y参数也是数字，返回值也是数字
        */
        ```

    + 接口定义函数类型

        ```ts
        interface SearchFunc {
            // 两个参数都是 string 类型，返回值为 boolean 类型
            (source: string, subString: string): boolean;
        }
        
        let mySearch: SearchFunc;
        mySearch = function(source: string, subString: string) {
            return source.search(subString) !== -1;
        }
        ```

    + 函数的可选参数

        ```ts
        // 由于函数不允许多参数或者少参数，所以有这样需求的时候可以使用可选参数处理 ?:  
        // 可选参数后不能出现必需参数
        function buildName(firstName: string, lastName?: string) {}
        ```

    + 参数默认值

        ```ts
        function buildName(firstName: string, lastName: string = 'Cat') {
            return firstName + ' ' + lastName;
        }
        ```

5. 断言
    + 语法

        ```ts
        // 值 as 类型
        // 或
        // <类型>值
        ```

    + 任意类型

        ```ts
        (window as any).foo = 1
        ```

6. 声明
    + 文件名

        ```ts
        // *.d.ts
        ```

    + 全局变量

        ```ts
        // declare var 声明全局变量
        declare let jQuery: (selector: string) => any;
        
        // declare function 声明全局方法
        declare function jQuery(selector: string): any;
        declare function jQuery(callback: () => any): any;
        
        // declare class 声明全局类
        declare class Animal {
            name: string;
            constructor(name: string);
            sayHi(): string;
        }

        // declare enum 声明全局枚举类型
        declare enum Directions {
            Up,
            Down,
            Left,
            Right
        }
        
        // declare namespace 声明（含有子属性的）全局对象 (已经被淘汰)
        declare namespace jQuery {
            function ajax(url: string, settings?: any): void;
            const version: number;
            class Event {
                blur(eventType: EventType): void
            }
            enum EventType {
                CustomClick
            }
        }
        declare namespace jQuery.fn {
            function extend(object: any): void;
        }
        
        // interface 和 type 声明全局类型  
        interface AjaxSettings {
            method?: 'GET' | 'POST'
            data?: any;
        }
        declare namespace jQuery {
            function ajax(url: string, settings?: AjaxSettings): void;
        }
        ```

    + 组合声明

        ```ts
        // 也就是下面两种声明可以同时使用
        declare function jQuery(selector: string): any;
        declare namespace jQuery {
            function ajax(url: string, settings?: any): void;
        }
        ```

    + `export` 只有在声明文件中使用 `export` 导出，然后在使用方 `import` 导入后，才会应用到这些类型声明

        ```ts
        export const name: string;
        export function getName(): string;
        export class Animal {
            constructor(name: string);
            sayHi(): string;
        }
        export enum Directions {
            Up,
            Down,
            Left,
            Right
        }
        export interface Options {
            data: any;
        }
        ```

    + 混用 `declare` 和 `export`

        ```ts
        declare const name: string;
        declare function getName(): string;
        declare class Animal {
            constructor(name: string);
            sayHi(): string;
        }
        declare enum Directions {
            Up,
            Down,
            Left,
            Right
        }
        interface Options {
            data: any;
        }
        
        export { name, getName, Animal, Directions, Options };
        ```

    + `enum` 默认导出

        ```ts
        // 错误的默认导出
        export default enum Directions {
            Up,
            Down,
            Left,
            Right
        }
        ```

        ```ts
        // 正确的默认导出
        declare enum Directions {
            Up,
            Down,
            Left,
            Right
        }
        
        export default Directions;
        ```

    + `export as namespace` （表示可以直接引用该库）
    + 直接扩展全局变量

        ```ts
        interface String {
            prependHello(): string;
        }
        ```

    + 在 `npm` 包或 `UMD` 库中扩展全局变量

        ```ts
        declare global {
            interface String {
                prependHello(): string;
            }
        }
        export {};
        // 注意即使此声明文件不需要导出任何东西，仍然需要导出一个空对象，用来告诉编译器这是一个模块的声明文件，而不是一个全局变量的声明文件
        ```

    + 模块插件

        ```ts
        // 如果是需要扩展原有模块的话，需要在类型声明文件中先引用原有模块，再使用 declare module 扩展原有模块
        import * as moment from 'moment';

        declare module 'moment' {
            export function foo(): moment.CalendarKey;
        }
        
        // 导入 moment 模块
        declare module 'moment';
        ```

    + 三斜线指令（以下两种情况可以使用）
        + 书写一个全局变量的声明文件时，如果需要引用另一个库的类型，那么就必须用三斜线指令了

            ```ts
            /// <reference types="jquery" />
            
            declare function foo(options: JQuery.AjaxSettings): string;
            ```

        + 全局变量不支持通过 import 导入

            ```ts
            /// <reference types="node" />
            export function foo(p: NodeJS.Process): string;
            ```

        + 拆分声明文件

            ```ts
            // 当我们的全局变量的声明文件太大时，可以通过拆分为多个文件，然后在一个入口文件中将它们一一引入，来提高代码的可维护性。
            // node_modules/@types/jquery/index.d.ts

            /// <reference types="sizzle" />
            /// <reference path="JQueryStatic.d.ts" />
            /// <reference path="JQuery.d.ts" />
            /// <reference path="misc.d.ts" />
            /// <reference path="legacy.d.ts" />
            
            export = jQuery;
            // 其中用到了 types 和 path 两种不同的指令。它们的区别是：types 用于声明对另一个库的依赖，而 path 用于声明对另一个文件的依赖 
            ```

    + 自动生成声明文件
        + 可以在命令行中添加 --declaration（简写 -d）
        + `tsconfig.json` 中添加 `declaration` 选项

            ```ts
            {
                "compilerOptions": {
                    "module": "commonjs",
                    "outDir": "lib",
                    "declaration": true,
                }
            }
            ```

        + 其他属性

            ```ts
            declarationDir 设置生成 .d.ts 文件的目录
            declarationMap 对每个 .d.ts 文件，都生成对应的 .d.ts.map（sourcemap）文件
            emitDeclarationOnly 仅生成 .d.ts 文件，不生成 .js 文件
            ```

    + 手动写的声明文件识别条件（一下任何一种都可以）
        + 给 `package.json` 中的 `types` 或 `typings` 字段指定一个类型声明文件地址

            ```json
            {
                "name": "foo",
                "version": "1.0.0",
                "main": "lib/index.js",
                "types": "foo.d.ts",
            }
            ```

        + 如果没有指定 `types` 或 `typings`，那么就会在根目录下寻找 `index.d.ts` 文件
        + 如果没有找到 `index.d.ts` 文件，那么就会寻找入口文件（`package.json` 中的 `main`字段指定的入口文件）是否存在对应同名不同后缀的 `.d.ts`文件。
7. 类型别名

    ```ts
    type Name = string;
    type NameResolver = () => string;
    type NameOrResolver = Name | NameResolver;
    function getName(n: NameOrResolver): Name {
        if (typeof n === 'string') {
            return n;
        } else {
            return n();
        }
    }
    ```

8. 字符串字面变量类型

    ```ts
    type EventNames = 'click' | 'scroll' | 'mousemove';
    ```

9. 元祖

    ```ts
    let tom: [string, number] = ['Tom', 25];
    ```

10. 枚举

    ```ts
    enum Days {Sun, Mon, Tue, Wed, Thu, Fri, Sat};
    ```

    + 手动赋值

        ```ts
        enum Days {Sun = 7, Mon = 1, Tue, Wed, Thu, Fri, Sat};
        ```

11. 类
    + 参数属性

        ```ts
        class Animal {
        public constructor(public name) {}
        }
        // 等同于
        class Animal {
            public name;
            public constructor(name) {
            this.name = name;
        }
        }
        ```

    + `readonly`

        ```ts
        // 只允许出现在属性声明或索引签名或构造函数中
        class Animal {
        readonly name;
        public constructor(name) {
            this.name = name;
        }
        }
        ```

        ```ts
        class Animal {
        public constructor(public readonly name) { }
        }
        // 等同于
        class Animal {
        public readonly name;
        public constructor(name) {
            this.name = name;
        }
        }
        ```

    + 抽象类

        ```ts
        // 抽象方法必须实现
        abstract class Animal {
        public constructor(public name) {}
        public abstract sayHi();
        }
        // 实现
        class Cat extends Animal {
            constructor(name) {
                super(name)
            }
            eat(){ }
        }
        ```

12. 接口
    + 接口继承接口

        ```ts
        interface Alarm { }
        
        interface LightableAlarm extends Alarm { }
        ```

    + 继承多接口

        ```ts
        interface Alarm { }
        
        interface Light { }
        
        class Car implements Alarm, Light { }
        ```

    + 接口继承类

        ```ts
        class Point {
            x: number;
            y: number;
            constructor(x: number, y: number) {
                this.x = x;
                this.y = y;
            }
        }
        
        interface Point3d extends Point {
            z: number;
        }
        
        let point3d: Point3d = {x: 1, y: 2, z: 3};
        ```

13. 泛型

    ```ts
    function createArray<T>(length: number, value: T): Array<T> {
        let result: T[] = [];
        for (let i = 0; i < length; i++) {
            result[i] = value;
        }
        return result;
    }
    
    createArray<string>(3, 'x');
    ```

    + 多个类型参数

        ```ts
        function swap<T, U>(tuple: [T, U]): [U, T] {
            return [tuple[1], tuple[0]];
        }
        
        swap([7, 'seven']);
        ```

    + 泛型约束

        ```ts
        interface Lengthwise {
            length: number;
        }
        
        function loggingIdentity<T extends Lengthwise>(arg: T): T {
            // 能够保证 arg 是 Lengthwise 的子类，一定含有 length 参数
            console.log(arg.length);
            return arg;
        }
        ```

        ```ts
        function copyFields<T extends U, U>(target: T, source: U): T {
            for (let id in source) {
                target[id] = (<T>source)[id];
            }
            return target;
        }
        
        let x = { a: 1, b: 2, c: 3, d: 4 };
        
        copyFields(x, { b: 10, d: 20 });
        ```

    + 泛型接口

        ```ts
        interface CreateArrayFunc {
            <T>(length: number, value: T): Array<T>;
        }
        
        let createArray: CreateArrayFunc = function<T>(length: number, value: T): Array<T> { }
        ```

        ```ts
        interface CreateArrayFunc<T> {
            (length: number, value: T): Array<T>;
        }
        
        let createArray: CreateArrayFunc<any> = function<T>(length: number, value: T): Array<T> { }
        ```

    + 泛型类

        ```ts
        class GenericNumber<T> {
            zeroValue: T;
            add: (x: T, y: T) => T;
        }
        let myGenericNumber = new GenericNumber<number>();
        myGenericNumber.zeroValue = 0;
        myGenericNumber.add = function(x, y) { return x + y; };
        ```

    + 泛型参数的默认类型

        ```ts
        <T = string>
        ```
