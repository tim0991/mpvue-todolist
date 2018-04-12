<template>
    <div class="container">
        <div class="header">
            <image class="plus" src="/static/images/plus.png" />
            <input class="new-todo" placeholder="Anything here..." @confirm="addTodo" v-model="inputValue" />
        </div>
        <div v-if="todos.length">
            <div class="todos">
                <!-- List items should get the class `completed` when marked as completed -->
                <div v-for="(item, index) in todos" :key="index" :class="{completed: item.completed,item:true}" @click="completeTodo(item)">
                    <!-- completed: success, todo: circle -->
                    <icon class="checkbox" :type="item.completed?'success':'circle'" />
                    <span class="name">{{item.name}}</span>
                    <icon class="remove" type="clear" size="16" @click.stop="removeTodo(item)" />
                </div>
            </div>
            <div class="footer">
                <span class="btn" @click="toggleAll">Toggle all</span>
                <span v-if="leftCount">{{leftCount}} items left</span>
                <span class="btn" v-if="todos.length != leftCount" @click="clearAll">Clear completed</span>
            </div>
        </div>
        <div v-else>
            <div class="empty">
                <span class="title">Congratulations!</span>
                <span class="content">There's no more work left.</span>
            </div>
        </div>
    </div>
</template>

<script>
import _ from "lodash";
export default {
    data() {
        let todos = [
            {
                name: "demo1",
                completed: true
            },
            {
                name: "demo2",
                completed: false
            },
            {
                name: "demo3",
                completed: true
            },
            {
                name: "demo4",
                completed: true
            },
            {
                name: "demo5",
                completed: false
            },
            {
                name: "demo6",
                completed: true
            },
            {
                name: "demo7",
                completed: true
            }
        ];
        return {
            todos: todos,
            toggleAllStatus: false,
            inputValue: ""
        };
    },
    computed: {
        leftCount() {
            let length = this.todos.filter(item => {
                return !item.completed;
            }).length;

            return length;
        }
    },
    methods: {
        addTodo() {
            let value = this.inputValue.trim();
            if (!value) {
                return;
            }
            this.todos.push({
                name: value,
                completed: false
            });
            this.inputValue = "";
        },

        removeTodo(item) {
            this.todos.splice(this.todos.indexOf(item), 1);
            this.todos = _.clone(this.todos);
        },
        completeTodo(item) {
            // item.completed = !item.completed;
            // this.todos = _.clone(this.todos);
        },
        clearAll() {
            this.todos = this.todos.filter(item => {
                return !item.completed;
            });
        },
        toggleAll() {
            this.toggleAllStatus = !this.toggleAllStatus;
            this.todos.forEach(item => {
                item.completed = this.toggleAllStatus;
            });
        }
    }
};
</script>

<style scoped>
.header {
    display: flex;
    align-items: center;
    border: 1rpx solid #e0e0e0;
    border-radius: 10rpx;
    box-shadow: 0 0 5rpx #e0e0e0;
    margin-bottom: 30rpx;
    padding: 20rpx;
}

.header .plus {
    width: 41rpx;
    height: 41rpx;
    margin-right: 20rpx;
}

.header .new-todo {
    flex: 1;
    font-size: 28rpx;
}

.todos {
    border: 1rpx solid #e0e0e0;
    border-radius: 10rpx;
    box-shadow: 0 0 5rpx #e0e0e0;
}

.todos .item {
    display: flex;
    align-items: center;
    padding: 25rpx;
    border-bottom: 1rpx solid #e0e0e0;
}

.todos .item:last-child {
    border-bottom: 0;
}

.todos .item .checkbox {
    margin-right: 20rpx;
}

.todos .item .name {
    flex: 1;
    font-size: 30rpx;
    color: #444;
}

.todos .item.completed .name {
    span-decoration: line-through;
    color: #888;
}
/* 
  .todos .item .remove {
    cursor: pointer;
  }
  */
.footer {
    display: flex;
    justify-content: space-between;
    margin: 30rpx 0;
    padding: 0 10rpx;
    font-size: 26rpx;
    color: #777;
}
/* 
  .footer .btn {
    cursor: pointer;
  }
  */
.empty {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
}

.empty .title {
    font-size: 60rpx;
    margin: 200rpx 50rpx 50rpx;
    color: #444;
}

.empty .content {
    color: #666;
    span-align: center;
}
.hidden {
    display: none;
}
</style>
