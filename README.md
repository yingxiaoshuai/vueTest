## 背景介绍
* 写这个东西的原因是因为在用vue开发时，因为一些写法导致出现bug，为此专门记录下来，用作警示，也顺带提醒其他人，避免踩坑

## 使用的环境
* vue2
* typescript
* vue-class-component 7.2.3

## 踩坑
### 踩坑1
* Vue ts使用class时要使用name属性作为类的名称，否则在打包时被webpack对类的name进行混淆出现使用name出错（出现class的name失效）或者使用component.options.name获取name
``` typescript
  @Component({
 // 必须要加name
 name:'AuthAssign'
 })
 // 会在webpack中被混淆变成 class a
 export default class AuthAssign extends Vue {}
 ```

### 踩坑2
* Vue Ts 禁止使用get方法返回数据的对象去显示页面，会导致数据无法被vue代理,产生数据变更了但页面没有变更bug，get方法只适用于数据的计算和返回属性。
``` typescript
// 使用对象在页面显示出来是禁止的,<div>{{test.a}}</div>
 get test(){
     return {a:1,b:2};
 }

// 这个是可以的,<div>{{test}}</div>
  get test(){
     return 'test';
 }
```

### 踩坑3
* Vue Ts 在使用箭头函数时，不能在new object(()=>{})中使用，否则使用的this指向会指向创建的类，而不是vueComponent所代理的this
* 构造函数的this指向本身的class,而方法的this指向vueComponent，也就是说，不要在初始化的时候使用箭头函数，或者不要在箭头函数中使用this
``` typescript
  BMITable: CBcTable = new CBcTable({
    setRowStyle: (rowData) => {
      // 这个this
      console.log(this)
      if (rowData.type === this.weightStatus)
        return { color: '#00bcd4' }
    }
  });

  created(){
    // 这个this 这两个的this是不一样的
    console.log(this)
  }
  ```