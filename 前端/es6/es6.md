### es6新特性

#### 1. let声明变量

> var声明的变量 往往会跨域；let声明的变量往往会有严格作用域

```javascript
 <script>
        {
            let a = 10;
            var b = 10;
        }
        console.log(a) // 10
        console.log(b) // ReferenceError: a is not defined
</script>
```

> var可以多次声明变量  let只可以声明一次

```javascript
 // 2. var可以多次声明变量  let只可以声明一次
        let a = 10;
        let a = 20;
        var b = 10;
        var b = 20; 
        console.log(a) // SyntaxError: Identifier 'a' has already been declared
        console.log(b) // 10
```

> var 会变量提升 let 不存在变量提升

```javascript
 // 3. var会出现变量提升
        console.log(x); // undefined
        var x = 10;
        console.log(y) // ReferenceError: Cannot access 'y' before initialization
        let y = 10;
```

#### 2. const 声明只读变量

```javascript
 // 4. const 声明只读变量
        const a = 10;
        a= a + 1; // TypeError: Assignment to constant variable.
        console.log(a)
```

#### 3. 结构表达式

```javascript
 		// 1. 数组解构
        let arr = [1,2,3]
        let [a,b,c] = arr;
        console.log(a,b,c) // 1,2,3
		// 2. 对象
        const person = {
            name: "jack",
            age: 20,
            language: ['java', 'es6']
        };
        const { name, age, language } = person;
        console.log(name, age, language)
		//3. 对象起别名
        const person = {
            name: "jack",
            age: 20,
            language: ['java', 'es6']
        };
        const { name: abc, age, language } = person;
        console.log(abc, age, language)
		 // 4. 对象函数
        const str = "hello.vue"
        console.log(str.startsWith('h')) // true
        // 5. 字符串模板
        let ss = `<div>
            <span>hello word </span>
         </div>`
        console.log(ss)
        // 5.2 字符串插入变量或者函数
        let info = `我是 ${abc} , 我今年 ${age} 岁了`;
        console.log(info)
```

#### 4. 函数优化

```javascript
	 // 1. 函数参数设置默认值
        // 原
        function add(a, b) {
            b = b || 1
            return a + b;
        }
        console.log(add(10))
        // 优化后
        function add2(a, b = 10) {
            return a + b;
        }
        console.log(add2(10))
	 	// 2. 不定参数
        function fun(...values) {
            console.log(values.length)
        }
        fun(1, 2)
        fun(1, 2, 3)
 		//3. 箭头函数
        let print = function (obj) {
            console.log(obj)
        }
        print("hello");
        let print2 = obj => console.log(obj)
        print2("hello2")
        let add3 = function (a, b) {
            return a + b;
        }
        let add4 = (a, b) => a + b;
        console.log(add3(1,3))
        console.log(add4(1,2))
		  let add5 = (a, b) => {
            let c = a + b;
            return a + c;
        }
        console.log(add5(1, 3));
		// 对象析构
        const person = {
            name: "jack",
            age: 20,
            language: ['java', 'es6']
        };
        let helllo2 = ({ name }) => console.log(name)
        helllo2(person)
```

#### 5. 对象优化

```javascript
		// 
        const person = {
            name: "jack",
            age: 20,
            language: ['java', 'es6']
        };
        // 
        console.log(Object.keys(person)) // 获取key
        console.log(Object.values(person)) // 获取value
        console.log(Object.entries(person)) //  key-value
        // 对象复制
        const target = { a: 1 }
        const source1 = { b: 1 }
        const source2 = { c: 1 }
        Object.assign(target, source1, source2)
        console.log(target) // {a: 1, b: 1, c: 1}
        // 对象声明
        const person3 = {
            name: "jack",
            age: 20,
            language: ['java', 'es6'],
            eat: function (food) {
                console.log(this.name + '在吃' + food)
            },
            // 箭头函数不能使用this 必须使用对象才可以
            eat2: food => console.log(person3.name + '在吃' + food),
            eat3(){
                console.log(this.name + '在吃' + food);
            }
        };
        person3.eat("苹果")
        person3.eat2("香蕉")
		person3.eat3("橘子")
		// 对象扩展 
 		// 1. 对象深copy
        const p1 = {
            name: "jack",
            age: 20,
            language: ['java', 'es6']
        };
        let copyone = {...p1};
        copyone.name='测试';
        console.log(p1)
        console.log(copyone)
		//2. 对象的合并
        let name1 = {name:'zhangsan'};
        let age1 = {age: 10};
        // 如果两个对象的字段有重复的话 后边的会覆盖前边的
        let persontest = {...name1,...age1}
        console.log(persontest)
```

