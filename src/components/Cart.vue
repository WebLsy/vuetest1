<template>
    <div>
        <h2>我是购物车</h2>
        <table>
            <tr>
                <td>勾选</td>
                <td>课程名称</td>
                <td>课程单价</td>
                <td>数量</td>
                <td>课程总价</td>
            </tr>
            <tr v-for="(item,index) in courseItem" :key="item.id">
                <td><input type="checkbox" v-model="item.isActive"></td>
                <td>{{item.name}}</td>
                <td>{{item.price}}</td>
                <td>
                    <button @click="minusNum(index)">-</button>
                    {{item.number}}
                    <button @click="addNum(index)">+</button>
                </td>
                <td>{{item.number * item.price}}</td>
            </tr>
            <tr>
                <td></td>
                <td colspan="2">
                    {{activeCoureNum}}
                    /
                    {{allCoureNum}}
                </td>
                <td colspan="2">
                    {{allPrice}}
                </td>
            </tr>
        </table>
    </div>
</template>

<script>
    export default {
        props: ['courseItem'],
        methods: {
            minusNum(index) {
                let num = this.courseItem[index].number;
                
                if (num > 1)
                {
                    this.courseItem[index].number -= 1;
                } else{
                    if (window.confirm('确定要删除此课程吗？'))
                    {
                        this.$emit('removeCoure', index);
                    }
                }
            },
            addNum(index) {
                this.courseItem[index].number += 1;
            }
        },
        computed: {
            activeCoureNum() {
                return this.courseItem.filter(item => item.isActive).length;
            },
            allCoureNum() {
                return this.courseItem.length;
            },
            allPrice() {
                let num = 0;
                this.courseItem.forEach(item => {
                    if (item.isActive)
                    {
                        num += item.price * item.number
                    }
                });
                return num;
            }
        },
    }
</script>

<style lang="scss" scoped>

</style>