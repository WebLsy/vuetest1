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
        <cart :courseItem="courseItem"></cart>
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
            
            if (isHasCourse)
            {
                isHasCourse.number += 1;
            } else{
                this.courseItem.push({
                    ...item,
                    number: 1,
                    isActive: true
                });
            }
        },
        addCourseToList() {
            this.courseList.push(this.courseInfo);
        }
    },
    data() {
        return {
            title: "开课吧课程11",
            courseInfo: {
                id: 2,
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
