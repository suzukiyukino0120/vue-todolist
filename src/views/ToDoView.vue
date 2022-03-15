<template>
    <el-container>
    <el-main>

    <div class="todo-list">

    <h1>ToDoList</h1>
    
<el-input class="input" v-model="content" placeholder="タスクを入力"></el-input>
<el-button @click="postMemo">追加</el-button>
 
    <br>

    <ul>
    <li v-for="memo in memos" :key="memo.id" class="todo">
      <h3>{{memo.content}}</h3>
      <div>
         <el-button @click="deleteMemo(memo.id)">削除</el-button>
      </div>
    </li>
  </ul>

</div>

</el-main>
</el-container>
    
</template>

<script>
import axios from 'axios'
 
  export default {
    name: "ToDoListView",
    data () {
      return {
        memos: [
            {
                id: "",
                content: ""
            }
        ]
      }
    },
    methods: {
      getMemo () {
        const path = 'http://localhost:8080/test/memos'
        axios.get(path)
            .then(response => {
            this.memos = response.data
            })
            .catch(error => {
            console.log(error)
            })
        },
        postMemo(){
            const path = 'http://localhost:8080/test/memo'
            axios.post(path, {
                content: this.content
            })
            .then((response) => {
                console.log(response);
                this.posts = response.data.posts;
                this.getMemo();
            })
            .catch((err) => {
                console.log(err);
            });
        },
        deleteMemo(id){
            const path = 'http://localhost:8080/test/memo/'
            axios.delete(path + id)
                .then((response) => {
                console.log(response);
                this.getMemo();
            })
            .catch((err) => {
                console.log(err);
            });
        }
    },
    created() {
        this.getMemo()
    }
}   
  
</script>

<style scoped>
#app {
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.input {
    width: 30%
}
h3 {
  margin-right: 20px;
}
li {
  width: 30vw;
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
  list-style: none;
}
.todo {
  cursor: pointer;
}
.done {
  text-decoration: line-through;
}

</style>