# vue基础

### 1.vue的模块语法

  vue模板语法有两大类“

​    1：插值语法

​      功能：用于解析标签体内容

​      写法：{{xxx}}, xxx是js表达式，并且可以读取到data中的所有属性

​    2：指令语法

​      功能：用于解析标签（标签属性，标签体内容，绑定事件）

​      举例：v-bind:href="xxx" 可以简写成 :href="xxx" 

​      备注：vue有很多指令，形式都是v-???

```vue
<div id="root">
        <h1>插值语法</h1>
        <h3>你好,{{name}}</h3>
        <hr/>
        <h1>指令语法</h1>
        <a v-bind:href="school.url">点我去尚硅谷学习1</a>
        <a :href="school.url">点我去尚硅谷学习1</a>
    </div>    
</body>


<script type="text/javascript">
    Vue.config.productionTip = false  //设置为 false 以阻止 vue 在启动时生成生产提示

    new Vue({
        el:'#root',
        data:{
            name:'jack',
            school:{
                name:'尚硅谷',
                url:'http://www.atguigu.com',
            }
            
        }
    })
```

### 2.数据绑定

​	 vue中有两种数据绑定的方式

​      1：单项绑定(v-bind):数据只能从data流向页面

​      2: 双向绑定(v-model):数据可以双向流动；

​            v-model:value 可以简写成v-model

```vue
<div id="root">
        单向数据绑定:<input type="text" v-bind:value="name"></input> </br>
        双向数据绑定:<input type="text" v-model:value="name"></input> 
    <!-- v-model只能应用在表单元素上（输入类元素）-->
    </div>    
```

### 3.MVVM模型

   MVVM模型：

​       1.M：模型(Model):data中的数据

​       2.V:视图(View)：模板代码

​       3.VM:视图模型(ViewModel)：vue实例

  观察发现：1.data中所有的属性，最后都出现在vm上

​       2.vm身上所有的属性以及vue原型上所有的属性，在vue模板中可以直接使用。

### 4.数据代理

```vue
<!-----数据代理：通过一个对象代理对另外一个对象中属性的操作（读/写）-->
        <script type="text/javascript">
            let obj={x:100}
            let obj2={y:200}
            Object.defineProperty(obj2,'x',{
                get(){
                    return obj.x
                },
                set(value){
                    obj.x = value
                }
            })
        //理解：通过改变obj2中的值从而改变obj中的值，相当于其他通路

        </script>
```

### 5.object.defineProperty方法

```vue
<!-----数据代理：通过一个对象代理对另外一个对象中属性的操作（读/写）-->
        <script type="text/javascript">
            let obj={x:100}
            let obj2={y:200}
            Object.defineProperty(obj2,'x',{
                get(){
                    return obj.x
                },
                set(value){
                    obj.x = value
                }
            })
        //理解：通过改变obj2中的值从而改变obj中的值，相当于其他通路

        </script>
```

### 6.事件处理

  事件的使用：

​    1.使用v-on:click 或者@click绑定事件

​    2.事件的回调需要配置在methods对象中，最终出现在vm上

​    3.@click=“demo()”括号中可以传参数也可以不传

```vue
<div class="root">
        <h2>欢迎来到{{name}}学习</h2>
        <!-- <button v-on:click="showinfo">点我提示信息</button> -->
        <button @click="showinfo1">点我提示信息1(不传参数)</button>
        <button @click="showinfo2(66,$event)">点我提示信息2(传参数)</button>
    </div>
</body>

<script type="text/javascript">

    
   new Vue({
        el:'.root',
        data:{
            name:'尚硅谷',
        },
        methods:{
            showinfo1(){alert('同学你好1')},
            showinfo2(number)
            {console.log(number)}
        }
    })
</script>
```

#### 事件修饰符

Vue中的事件修饰符

​    1.prevent:阻止默认事件（常用）

​    2.stop:阻止事件冒泡（常用）

​    3.once:事件只触发一次（常用）

​    4.capture:使用事件的捕获模式

​    5.self:只有event.target是当前操作的元素才是触发事件

​    6.passive:事件的默认行为立即执行，无需等待时间回调执行完毕

```vue
<div id="root">
        <h2>欢迎来到{{name}}学习</h2>
        <!-- 阻止默认事件（常用）   修饰符可以连续使用-->
        <a href="http://www.atguigu.com" @click.prevent.stop="showinfo">点我提示信息</a>
        <!-- 阻止事件冒泡（常用） -->
        <div>
            <button @click.stop="showinfo">点我提示信息</button>
        </div>
    </div>
```

#### 键盘事件

vue中常用到的按键别名（当按下键是触发事件）

​    1. 回车:enter  删除:delete  退出:esc  空格:space  换行:tab  上下左右： 

​    2.  用法特殊的键：ctrl alt shift meta

​      keyup:按下修饰键的同时，再按下其他键，随后释放其他键，时间才会触发

​      keydown:正常触发事件

​    3.vue.config.keyCodes.自定义键名=键码  可以定制按键别名

```vue
<div id="root">
        <h2>欢迎来到{{name}}学习</h2>
        <input type="text" placeholder="按下回车提示输入" @keyup.13="showinfo">
    </div>
</body>

<script type="text/javascript">
    Vue.config.keyCodes.huiche = 13
    new Vue({
        el:'#root',
        data:{
            name:'尚硅谷'
        },
        methods:{
            showinfo(e){
               console.log(e.target.value)    //拿到输入的值
            }
        },
    })
</script>
```

### 7.计算属性

计算属性：

​    1.定义：要用的属性不存在，需要通过已有属性计算而来

​    2.原理:底层借助object.defineproperty方法提供的getter setter方法

​    3.优势:与methods相比，内部有缓存机制，效率更高，调试方便。

​    4.备注：计算属性最终会出现在vm上，直接读取使用就可

​        如果计算属性需要修改，那必须写set函数去响应修改，

```vue
姓：<input type="text" v-model:value="firstName"><br><br>
        名：<input type="text" v-model="lastName"><br><br>
        姓名：<span>{{fullName}}</span>
    </div>
</body>

<script type="text/javascript">
    new Vue({
        el:'#root',
        data:{
            firstName:'张',
            lastName:'三'
        },
        //计算属性
        computed:{
            //完整写法
            fullName:{
                //get作用：当有人读取fullname时，get就会被调用，返回值为fullname的值
                //当初次读取fullname时候，或者所依赖的数据发生变化时，get才会被调用
                get(){ return this.firstName + '-' + this.lastName},
                //当fullname被修改时set才会被调用
                set(){}
                //此处的this是vm
            }
                //简便写法
                //fullName(){
                // return this.firstName + '-' + this.lastName
                //}
        }
    })
</script>
```

#### methods实现

```vue
<div id="root">
        姓：<input type="text" v-model:value="firstName"><br><br>
        名：<input type="text" v-model="lastName"><br><br>
        姓名：<span>{{fullName()}}</span>
    </div>
</body>

<script type="text/javascript">
    new Vue({
        el:'#root',
        data:{
            firstName:'张',
            lastName:'三'
        },
        methods:{
            fullName(){
                return this.firstName + '—' + this.lastName
            }
        },
    })
</script>
```

### 8.监视属性

```vue
<div id="root">
        <h2>今天天气很{{info}}</h2>
        <button @click="changeWeather">切换天气</button>
        <!-- <button @click="isHot = !isHot">切换天气</button> -->
    </div>
    
</body>
<script>
    new Vue({
        el:'#root',
        data:{
            isHot:true,
        },
        computed:{
            info(){
                return this.isHot ? '炎热' : '凉爽'
            }
        },
        methods: {
            changeWeather(){
                this.isHot = !this.isHot
            }
        },
        // watch:{
        //     isHot:{
        //         immediate:true,  //初始化时候让handler调用一下
        //         handler(newValue,oldValue){console.log('ishot被修改了',newValue,oldValue)}
        //     }
        // }
    })
    //通过vm.$watch监视
    vm.$watch('isHot',{
        immediate:true,  //初始化时候让handler调用一下
                handler(newValue,oldValue){console.log('ishot被修改了',newValue,oldValue)}
    })

</script>
```

### 9.绑定样式

```vue
<div id="root">
        <!-- basic样式不变，a的样式变化， -->
        <!-- 字符串写法：适用于要绑定的样式类名不确定， -->
        <div class="basic" v-bind:class="mood" @click="changemood">{{name}}</div>
        <!-- 数组写法：适用于要绑定的样式个数不确定，名字也不确定，在数组中添加 -->
        <div class="basic" :class="arr" >{{name}}</div>
        <!-- 对象写法：适用于要绑定的样式个数确定，名字也确定，动态决定是否用 -->
        <div class="basic" :class="classObj" >{{name}}</div>

        <div class="basic" :style="styleObj"></div>
    </div>
</body>
<script>
    new Vue({
        el:'root',
        data:{
            name:'尚硅谷',
            mood:'normal',
            arr:['happy','sad','normal'],
            classObj:{
                atguigu1:false,
                atguigu2:false,
            },
            styleObj:{
                fontSize:'40px',             //优势：不需要操作dom
                color:'red',
                backgroundColor:'orange'
            }
        },
        method:{
            changeMood(){
                const arr =['happy','sad','normal']
                const index = Math.floor(Math.random()*3)  //向下取整使得值不超过3
                this.mood = arr[index]
            }
        }
    })
</script>
```

### 10.渲染

#### 条件渲染

```vue
<div id="root">、
        <!-- 渲染不彻底，控制台可查看   用于切换频率高-->
        <h2 v-show="false">欢迎来到{{name}}</h2>
        <h2 v-show="1===1">欢迎来到{{name}}</h2>

        <!-- 渲染彻底 -->
        <h2 v-if="false">欢迎来到{{name}}</h2>
        <h2 v-if="1===1">欢迎来到{{name}}</h2>

        template好处：不影响结构且只能搭配v-if使用
        <template v-if="n===1">   
            <h2>你好</h2>
            <H2>尚硅谷</H2>
            <h2>北京</h2>
        </template>
    </div>
```

#### 列表渲染

基本列表

```vue
<ul>
            <!-- v-for遍历persons数组中的元素 -->
            <li v-for="(p,index) in persons" :key="index">{{p.name}}-{{p.age}}</li>  
</ul>
persons:[
            {id:'001',name:'张三',age:'18'},
            {id:'002',name:'李四',age:'19'},
            {id:'003',name:'王五',age:'20'}
                    ],
```

列表过滤

```vue
<div id="root">
        <h2>人员列表</h2>
        <input type="text" placeholder="请输入名字" v-model="keyWord">
        
        <ul>
            <li v-for="(p,index) in filPersons" :key="index" >
                {{p.name}}-{{p.age}}--{{p.sex}}             
            </li>
        </ul>
    </div>
</body>

<script>
    //用watch实现
    // new Vue({
    //     el:'#root',
    //     data:{
    //         keyWord:'',
    //         persons:[
    //         {id:'001',name:'马冬梅',age:'18',sex:'女'},
    //         {id:'002',name:'周冬雨',age:'19',sex:'女'},
    //         {id:'003',name:'周杰伦',age:'20',sex:'男'},
    //         {id:'004',name:'温兆伦',age:'21',sex:'男'}
    //                 ],
    //         filPersons:[]
    //     },
    //     watch:{
    //         keyWord:{
    //             immediate:true,
    //             handler(val){
    //                 this.filPersons = this.persons.filter((p)=>{
    //                     return p.name.indexOf(val) !== -1
    //                 })
    //             }
    //         }
    //     }
        
    // })

    //用计算属性实现
    new Vue({
         el:'#root',
         data:{
            keyWord:'',
            persons:[
            {id:'001',name:'马冬梅',age:'18',sex:'女'},
            {id:'002',name:'周冬雨',age:'19',sex:'女'},
            {id:'003',name:'周杰伦',age:'20',sex:'男'},
            {id:'004',name:'温兆伦',age:'21',sex:'男'}
                    ],
            
        },
        computed:{
            filPersons(){
                return this.persons.filter((p)=>{     //用于数组过滤
                    return p.name.indexOf(this.keyWord)!==-1  //找到给定元素的索引
                })
            }
        }
        
    })
</script>
```

列表排序

```vue
<div id="root">
        <h2>人员列表</h2>
        <input type="text" placeholder="请输入名字" v-model="keyWord">
        <button @click="sortType = 2">年龄升序</button>
        <button @click="sortType = 1">年龄降序</button>
        <button @click="sortType = 0">原顺序</button>
        <ul>
            <li v-for="(p,index) in filPersons" :key="index" >
                {{p.name}}-{{p.age}}--{{p.sex}}             
            </li>
        </ul>
    </div>
</body>

<script>
    //用计算属性实现
    new Vue({
         el:'#root',
         data:{
            keyWord:'',
            sortType:0,    //0 原顺序  1降序 2升序
            persons:[
            {id:'001',name:'马冬梅',age:'30',sex:'女'},
            {id:'002',name:'周冬雨',age:'19',sex:'女'},
            {id:'003',name:'周杰伦',age:'50',sex:'男'},
            {id:'004',name:'温兆伦',age:'21',sex:'男'}
                    ],
            
        },
        computed:{
            filPersons(){
                const arr =  this.persons.filter((p)=>{     //用于数组过滤
                    return p.name.indexOf(this.keyWord)!==-1  //找到给定元素的索引
                })
                //判断是否需要排序
                if(this.sortType){
                    arr.sort((p1,p2)=>{
                        return  this.sortType ===1 ? p2.age-p1.age : p1.age-p2.age
                    })
                }
                return arr
            }
        }
        
    })
</script>
```

diff算法以及key的作用

```vue
<div id="root">
        <h2>人员列表</h2>
        <button @click.once="add">添加一个老刘</button>
        <ul>
            <!-- v-for遍历persons数组中的元素 -->
                <!-- 
                    diff算法:两个虚拟DOM相互对比
                    冒号里面只能填p.id唯一标识并且必须累加 不能不填 -->
            <li v-for="(p,index) in persons" :key="p.id">
                {{p.name}}-{{p.age}}
                <input type="text">
            </li>
        </ul>
    </div>
    
    
    
    <script>
    new Vue({
        el:'#root',
        data:{
            persons:[
            {id:'001',name:'张三',age:'18'},
            {id:'002',name:'李四',age:'19'},
            {id:'003',name:'王五',age:'20'}
                    ],
        },
        methods: {
            add(){
                const p = {id:'004',name:'老刘',age:'50'}
                this.persons.unshift(p)
            }
        },
    })
</script>
```

### 11.表单

```vue
<div id="root">
        <form @submit.prevent="demo">
            账号:<input type="text" v-model="account"><br/><br/>
            密码:<input type="password" v-model="password"><br/><br/>
            年龄:<input type="number" v-model.number="age"><br/><br/>
            性别：
            男<input type="radio" name="sex" value="male" v-model="sex">
            女<input type="radio" name="sex" value="female" v-model="sex"><br/><br/>
            爱好:
            学习<input type="checkbox" value="study" v-model="hobby">
            打游戏<input type="checkbox" value="game" v-model="hobby">
            吃饭<input type="checkbox" value="eat" v-model="hobby"><br/><br/>
            所属校区
            <select v-model="city">
                <option value="">请选择校区</option>
                <option value="beijing">北京</option>
                <option value="beijing">上海</option>
                <option value="beijing">江西</option>
            </select><br/><br/>
            其他信息
            <textarea v-model="other"></textarea><br/><br/>
            <input type="checkbox" v-model="agree">阅读并接受<a href="www.baidi.com">用户协议</a>
            <button>提交</button>
        </form>
    </div>
</body>

<script>
    new Vue({
        el:'#root',
        data:{
            account:'',
            password:'',
            age:'',
            sex:'female',
            hobby:[],
            city:'',
            other:'',
            agree:'',
        },
        methods:{
            demo(){
                //console.log(JSON.stringify(this._data))
            }
        }
    })
</script>
```

### 12.内置命令

​       v-bind:单向绑定解析式

​      v-model:双向数据绑定

​      v-for:遍历数组/对象/字符串

​      v-on:绑定事件监听，可简写成@

​      v-if:条件渲染（动态控制节点是否存在）

​      v-else:条件渲染（动态控制节点是否存在）

​      v-show:条件渲染（动态控制节点是否展示）

​      v-html:支持格式解析

​      v-cloak:延迟显示页面时间

​      v-once:一次性数据，之后改变静态内容

​      v-pre:跳过该编译过程，原封不动展示

​    -->

### 13.自定义指令

```vue
div id="root">
        <h2>当前的n是：<span v-text="n"></span></h2>
        <h2>放大后的n是：<span v-big="n"></span></h2>
        <button @click="n++">点我n++</button>
        <hr>
        <input type="text" v-fbind:value="n">
    </div>
    
</body>
<script>
    new Vue({
        el:'#root',
        data:{
            n:1
        },
        //自定义指令
        directives:{
            //big在指令和元素成功绑定时或者指令所在的模板被重新解析时才会调用
            big(element,binding){
                element.innerText = binding.value*10  //修改原生dom中的text值
            },
            fbind:{
                //指令与元素成功绑定时
                bind(element,binding){element.value = binding.value},
                //指令所在元素被插入页面时
                inserted(element,binding){element.focus}, //获取焦点
                //指令所在模板被重新解析时
                uodate(element,binding){element.value = binding.value}
            },
        }
    })
</script>
```

### 14.生命周期（钩子）

​     生命周期：生命周期函数，生命周期函数，生命周期钩子

​       vue在关键的时候调用的函数

​       名字不可更改，函数具体内容由程序员根据需求编写

​       生命周期函数中的this指向的是vm或者组件实例对象

#### 引用生命周期

```vue
 <div id="root">
        <h2 :style="{opacity}">欢迎学习vue</h2>
    </div>
</body>

<script>
    new Vue({
        el:'#root',
        data:{
            opacity:1  //透明度
        },
        methods: {
            
        },
        //vue完成模板解析把真实dom放入页面后调用mounted
        //生命周期钩子
        mounted() {
            setInterval(()=>{
            this.opacity -= 0.01
            if(this.opacity<=0) this.opacity=1
            },16)
        },
    })
    //通过外部定时器实现
    // setInterval(()=>{
    //     vm.opacity -= 0.01
    //     if(vm.opacity<=0) vm.opacity=1
    // },16)
</script>
```

#### 分析生命周期

```vue
<script>
    new Vue({
        el:'#root',
        data:{
            n:1
        },
        methods: {
            add(){}
        },
        //创建数据代理，创建数据监测
        beforeCreate() {
            
        },
        //创建完毕
        created() {
            
        },
        //
        beforeMount() {
            
        },
        //页面呈现经过vue编译过后的DOM
        mounted() {
            
        },
        //视图和模型数据尚未同步
        beforeUpdate() {
            
        },
        //数据同步
        updated() {
            
        },
        //解绑和其他实例的连接，销毁前期还没有销毁
        beforeDestroy() {
            
        },
        //销毁完毕
        destroyed() {
            
        },
    })
```

#### 总结生命周期

```vue
<script>
    new Vue({
        el:'#root',
        data:{
            opacity:1  //透明度
        },
        methods: {
            stop(){
                this.$destroy()
            }
        },
        //vue完成模板解析把真实dom放入页面后调用mounted
        //生命周期钩子

        //常用:发送ajax请求，启动定时器，绑定自定义事件
        mounted() {
          this.timer =  setInterval(()=>{
            this.opacity -= 0.01
            if(this.opacity<=0) this.opacity=1
            },16)
        },
        //清除定时器，解除自定义事件，取消订阅消息
        beforeDestroy() {
            clearInterval(this.timer)
            console.log('vm被调用了')
        },
    })
    
</script>
```

### 15.组件

```vue
<div id="root">
        <!-- 第三步：编写组件标签 -->
        <xuexiao></xuexiao>
        <hr>
        <xuesheng></xuesheng>
    </div>
</body>
<script>

    //第一步：创建school组件
    const school = Vue.extend({
        template:`
            <div>
                <h2>学校名称：{{schoolName}}</h2>
                <h2>学校地址：{{address}}</h2>
            </div>
        `,
        //el:'#root',    组件不能够规定专门为谁服务
        data(){
            return {
                schoolName:'尚硅谷',
                address:'北京昌平',
            }
        }
    })
    //创建student组件
    const student = Vue.extend({
        template:`
            <div>
                <h2>学生姓名：{{studentName}}</h2>
                <h2>学生年龄：{{age}}</h2>
            </div>
        `,
        //el:'#root',    组件不能够规定专门为谁服务

        //函数式
        data(){
            return {
                studentName:'张三',
                age:18
            }
        }
    })




    //创建vm
    new Vue({
        el:'#root',
        //第二步：注册组件
        components:{
            xuexiao:school,
            xuesheng:student,
        }
    })
</script>
```

### 16.脚手架