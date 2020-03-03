<template>
    <div id="app">
        <h1>{{title}}</h1>
        <hr />
        <div>
            <h2>添加课程</h2>
            <div>
                <label for>课程名称：</label>
                <input type="text" v-model="courseInfo.name" />
            </div>
            <div>
                <label for>课程价格：</label>
                <input type="text" v-model="courseInfo.price" />
            </div>
            <div>
                <button @click="addCourseToList">添加到课程列表</button>
            </div>
        </div>

        <div>
            <h2>课程列表</h2>
            <table>
                <tr>
                    <th>课程名称</th>
                    <th>课程价格</th>
                    <th>操作</th>
                </tr>
                <tr v-for="(item,index) in courseList" :key="item.id">
                    <td>{{item.name}}</td>
                    <td>{{item.price}}</td>
                    <td>
                        <button @click="addCourseToCart(index)">添加到购物车</button>
                    </td>
                </tr>
            </table>
        </div>
        <cart :courseItem="courseItem" @removeCoure="remove"></cart>
    </div>
</template>

<script>
import Cart from "./components/Cart";

export default {
    name: "App",
    components: {
        Cart
    },
    methods: {
        addCourseToCart(index) {
            let item = this.courseList[index];
            let isHasCourse = this.courseItem.find(x => x.id == item.id);

            if (isHasCourse) {
                isHasCourse.number += 1;
            } else {
                this.courseItem.push({
                    ...item,
                    number: 1,
                    isActive: true
                });
            }
        },
        addCourseToList() {
            if (this.courseInfo.name == "") {
                alert("请输入课程名称");
            } else if (!this.checkFn(this.courseInfo.name)) {
                alert("此课程已存在");
            } else if (this.courseInfo.price == "") {
                alert("请输入课程价格");
            } else {

                this.courseList.push({...this.courseInfo, id: [...this.courseList].pop().id + 1});

                this.courseInfo.name = '';
                this.courseInfo.price = '';
            }
            console.log(this.courseList);
        },
        remove(index) {
            this.courseItem.splice(index, 1);
        },
        checkFn(obj) {
            for (var i = 0; i < this.courseList.length; i++) {
                if (this.courseList[i].name == obj) {
                    return false;
                }
            }
            return true;
        }
    },
    data() {
        return {
            title: "开课吧课程11",
            courseInfo: {
                name: "",
                price: ""
            },
            courseItem: [],
            courseList: [
                {
                    id: 0,
                    name: "web全栈开发架构师1",
                    price: 2000
                },
                {
                    id: 1,
                    name: "web全栈开发架构师2",
                    price: 1000
                }
            ],
            number: 1
        };
    }
};
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}
</style>
