<style>
    .main {
        margin-top: 10px;
    }

    .main .big-title {
        font-size: 18px;
    }

    .main .task-input {
        margin-top: 15px;
        width: calc(100% - 20px);
        height: 30px;
        padding-left: 20px;
        font-size: 12px;
    }

    .task-count {
        margin-top: 10px;
    }

    .task-count .action a {
        padding: 5px 20px;
    }

    .task-count .action a.active {
        border: 1px solid #ff8000;
    }

    .tasks {
        margin-top: 10px;
    }

    .tasks .no-task-tip {
        display: block;
        color: #ccc;
        margin-left: 25px;
        font-size: 12px;
        margin-bottom: 10px;
    }

    .todo-list .todo {
        height: 40px;
        border: 1px solid #999;
        margin-bottom: 5px;
        position: relative;
    }

    .tasks .todo-list .editing .edit {
        display: block;
    }

    .todo-list .completed .view label a {
        color: #ccc;
        font-size: 22px;
        text-decoration: line-through;
    }

    .todo-list .todo .edit {
        position: absolute;
        left: 0;
        top: 0;
        display: none;
        width: calc(100% - 50px);
        height: 40px;
        border: none;
        padding-left: 50px;
        font-size: 24px;
    }

    .todo .view input[type="checkbox"] {
        width: 20px;
        height: 20px;
        margin-left: 20px;
        margin-top: 13px;
        margin-right: 10px;
    }

    .todo .view label a {
        font-size: 24px;
        line-height: 40px;
        color: #000;
    }

    .todo .view .destroy {
        height: 40px;
        width: auto;
        line-height: 40px;
        margin-right: 20px;
        font-size: 14px;
        color: #f00;
        border: none;
        background-color: #fff;
        cursor: pointer;
    }
</style>

<template>
    <div class="main wrap">
        <h3 class="big-title">添加任务：</h3>
        <input @keyup.enter="addTodo" v-model="todo" type="text" placeholder="例如：吃饭睡觉打豆豆：   提示：+回车即可添加任务" class="task-input" />
        <!-- addTodo(123,$event)行间传参的时候，vue里面还要用事件对象的话就用$event-->
        <ul class="task-count clearFix">
            <li class="fl" style="color: #ff8000;margin-left: 20px;">
                <!--{{
                    list.filter(function(item){
                        return !item.isChecked
                    }).length
                }}-->
                {{ noCheckLength }} 个未完成任务
            </li>
            <li class="action fr">
                <a href="#all" class="fl" :class="{active:false}">所有任务</a>
                <a href="#unfinished" class="fl">未完成任务</a>
                <a href="#finished" class="fl">已完成任务</a>
            </li>
        </ul>
        <h3 class="big-title">任务列表：</h3>
        <div class="tasks">
            <span class="no-task-tip" v-show="!list.length">还没有添加任何任务</span>
            <!--<span class="no-task-tip tip-toggle">
                <input type="checkbox" checked="" class="toggle" />
                <span>全部标记为已完成</span>
            </span>-->
            <ul class="todo-list" v-show="list.length">
                <li class="todo" :class="{completed: item.isChecked,editing: item === editorTodos}" v-for="item in filterList">
                    <div class="view">
                        <input class="fl" type="checkbox" v-model="item.isChecked" class="toggle">
                        <!-- 将单选按钮的数据进行双向绑定，选中与不选中传值 -->
                        <!--<label class="fl" for=""><a href="javascript:;" v-text="item.title"></a></label>-->
                        <label class="fl" for="">
                            <a href="javascript:;" @dblclick="edtorTodo(item)">{{ item.title }}</a>
                        </label>
                        <a href="javascript:;" class="destroy fr" @click="delTodo(item)">删除</a>
                    </div>
                    <input @blur="edtorTodoed(item)" @keyup.13="edtorTodoed(item)" @keyup.esc="cancelTodo(item)" v-focus="editorTodos === item"
                        type="text" class="edit" v-model="item.title" />
                </li>
            </ul>
        </div>
    </div>
</template>
<script>
export default {
    name: 'main-wrapper',
    data() {
        return {
            list: list, //留言的内容数组，注意这里数组内每一条数据都是一个json
            todo: '',
            listMsg: {},
            editorTodos: '',//记录正在编辑的数据
            beforeTitle: '',//记录正在编辑的数据的title
            visibility: 'all'//通过这个属性值的变化对数据进行筛选
        }
    },
    watch: {
        /* list: function () {//监听list这个属性，当这个属性对应的值发生变化就会执行函数
                store.save("datura_msg",this.list);
            }*/
        list: {
            handler: function () {
                store.save("datura_msg", this.list);
            },
            deep: true
        }
    },
    computed: {
        noCheckLength: function () {
            return this.list.filter(function (item) {
                return !item.isChecked
            }).length
        },
        filterList: function () {
            //过滤的时候有三种情况 all finished unfinished

            //return fiter[this.visibility](list);//函数调用
            //找到了过滤函数，就返回过滤后的数据，如果没有就返回所有数据
            return fiter[this.visibility] ? fiter[this.visibility](list) : list;
        }
    },
    methods: {
        addTodo(data, ev) {  //添加任务  第二个参数接收一下事件对象
            //向list中添加一项任务
            //事件处理函数中的this，指向的的那个根实例
            /*if(ev.keyCode == 13){
                this.list.push({
                    title:ev.target.value   //这里是操作dom了，影响性能
                });
                }*/
            /* this.list.push({   //向list中添加一项任务
                    title:this.todo,
                    isChecked:false
                });*/
            this.listMsg = {
                title: this.todo,
                isChecked: false
            }
            this.list.push(this.listMsg);
            this.todo = ''
        },
        delTodo(todo) {//删除任务
            //在数组中查找点击当前数据在list数组中的位置，找到其索引值
            var index = this.list.indexOf(todo);
            if (confirm("确定删除？")) {
                this.list.splice(index, 1);
            }
        },
        edtorTodo(todo) {//编辑任务
            //编辑任务的时候。记录一下编辑这条任务的title，方便在取消编辑的时候重新给之前的title
            this.beforeTitle = todo.title;
            this.editorTodos = todo;
        },
        edtorTodoed(todo) {//任务编辑成功
            this.editorTodos = '';
        },
        cancelTodo(todo) {//取消编辑
            todo.title = this.beforeTitle;
            this.beforeTitle = '';
            //让div显示出来,input隐藏
            this.editorTodos = '';
        }
    },
    directives: {
        "focus": {
            update(el, binding) {
                // console.log(el);//指绑定的元素，可以用来直接操作DOM =>  <input type="text" class="edit" />
                //console.log(binding);
                if (binding.value == true) {
                    el.focus();//当前编辑元素获取焦点
                }
            }
        }
    }
}
</script>