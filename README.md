# TypeScript
typeScript笔记
1、类型系统：
	基本类型
	对象，数组，函数
	字面量，元组，枚举
	接口，类
	泛型
	
	1.1 基本类型
		number , string , boolean , null , undefined , symbol , void , enum , any
		
		1.1.1 void --空（不允许赋值，一般用于返回值）
		1.1.2 
			null -- null(只有null一个值)
			undefined ----undefined (只有undefined一个值)
		1.1.3
			enum(枚举)-- enumerate---（有限的可能性）
				例如： 性别，星期
				enum gender{
					male,
					female
			    }
			    let sex:gender = gender.male
		1.1.4 
			any（变体变量，可以是任何类型，js中的变量可以简单理解为any）
			let val:any = 'abc';
				val = '123';
		1.1.5
			symbol
	1.2 类型推测(隐式类型声明) ： 会根据初始化的值推测类型
		例如 let a = 12;  <==> let a:number = 12
			 let b;       <==> let b:any;
	1.3 冲突检测 ： 编译器编译时会自动排除掉无用的选项
	1.4 联合类型 ： 变量可以是多个类型混合；
		例如 ： let a:number|string;
				a = 12;//可以
				a = 'abc';//可以
				a = false;//编译报错
	1.5 数组类型
		1.5.1
			例如 ： let arr = [1,2,3]  <==> let arr:number[] = [1,2,3]
			   let arr = [2,false,'abc']   <==>  let arr:any[] = [2,false,'abc']
		1.5.2 为数组的值设置联合类型
			例如： let arr :(string|number)[] = ['abc','bob',3]
	1.6 函数参数类型
		1.6.1 为函数参数赋类型：
			function show(a:number,b:number){
				console.log(a+b)
			}
			//a,b只能传 number类型，其他类型直接编译报错
		1.6.2 ts中调用函数的实参必须与形参个数，类型相统一，否则报错
		1.6.3 为函数的返回值设置类型
			function add(a,b):number{
				return a+b; //a+b的返回值只能是number类型，否则报错
			}
		1.6.4 函数中的可选参数
			function sum(a:number,b:number,c?:number){
				return a+b
			}
			//此时c为可选参数，可有可无，有且只能是number类型
			如： sum(12,5)
				 sum(12,5,10)
	1.7 外部变量声明 ---例如 declare let 名字($,jquery...)
		declare let $;   //将外界引用的$引入到ts中,如果不引入,直接使用则会编译报错
		$(function(){
			$('#div').css('width','200px')
		})
	1.8 对象类型 
		1.8.1 对象的名值对默认必须个数与类型相统一，否则报错，但有时参数不定，所以用到 ?:
			例如
				let point:{
					x:number,
					y:number,
					z?:number //此句表明，z参数可有可无，但如果有，只能是number类型
				}
				如： point:{x:12,y:5}
					point:{x:12,y:5,z:10}

	2.1 接口 --interface
		2.1.1 接口与枚举的区别？
			枚举约定的是值可以有哪些
			接口约定的是成员可以有哪些
		2.1.2接口：约定，限制成员有哪些
		例如：定义一个 点 的接口
			interface Point{
				x:number,
				y:number,
				z?:number
			}
			let p:Point = {x:12,y:5}
		2.1.3 需要注意一点
			interface A{
				x:number,
				y:number
			}
			interface B{
				x:number,
				y:number,
				z:number
			}
			let n1:A|B;
			console.log(n1.z) // 只赋类型，获取值就会编译报错
			let n:A|B = {x:12,y:6,z:9}
			console.log(n1.z) // 9 [赋值后在取值则不会报错]
		2.1.4 当实参不确定时
			interface Person{
				name:string;
				age:number;
				skill?:string;
				[propName:string]:[string|number] | any;  //此句表明可以为变量添加除name,age以外的属性，属性名必须为string,属性值需要包含以上定义的所有类型，也就是可以用string和number或者直接用any
			}
			//调用
			let p:Person = {
				name:'suna',
				age:12,
				eat:'something'
			}
	
	3.1 类
		3.1.1 class写法，extend ,多继承
		class Person{
			name:string;
			age:number;

			constructor(name:string,age:number){
				this.name = name;
				this.age = age;
			}

			showMe(){
				console.log(`我的名字是${this.name},我的年龄是${this.age}`)
			}
		}
		调用：
			let p = new Person('sun',20)
			p.showMe()
		3.1.2 访问修饰符
			public     --公有，任何人都可以访问   
			private   --私有，只有类内部可以访问
			protected -- 受保护的（友元），只有子类能用
			例如:
				class Person{
					public name:string;
					private age:number;
					constructor(){
						this.age = 20
					}
					showAge(){
						console.log(this.age)
					}
				}
				let p1 = new Person()
				p.name = 'suns' //不报错
				p.age = 23    //类的私有变量，类外部不可访问，编译报错
	4.1 泛型--不同于 any
		4.1.1 Array 其实就是典型的泛型
			interface Array<T>{
				reverse():T[];
				sort(compare?:(a:T,b:T) => number):T[];
				...
			}
		4.1.2 简单示例：
			class Cacl<T>{
				a:T;
				b:T;
				constructor(a:T,b:T){
					this.a = a;
					this.b = b;
				}
				add(){
					console.log(this.a)
					console.log(this.b)
				}
			}
			let obj = new Cacl<number>(12,5)
			obj.add()
			let c1 = new Cacl<string>('123','abc')
			c1.add()

	5.1 类型字面量，利用 type 可以自定义类型的值

		5.1.1 字符串字面量
			例如 ： type myType = 'one' | 'two' | 'abc';
			let n:myType = 'one' // ok
			let n1:myType = 'three' //报错
 		5.1.2 数字字面量
 			例如 ： type myType1 = 1 | 2 | 4;
			let n:myType1 = 1 // ok
			let n1:myType1 = 'three' //报错
		5.1.3 布尔字面量
			例如 ： type myType2 = true | false;
			let n:myType2 = true // ok
			let n1:myType2 = 'three' //报错
		5.1.4 混合字面量
			例如 ： type myType3 = 'one' | 1 | true | false;
			let n:myType3 = true // ok
			let n1:myType3 = 'three' //报错
