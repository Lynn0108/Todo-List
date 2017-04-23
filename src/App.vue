<template>
  <div id="app">
    <h1>{{title}}</h1>
    <input type="text" id="write" v-model="newItem" @keyup.enter="addNew" >
    <button @click="addNew">{{submit}}</button>
    <form>
      <div v-for="(item, index) in items">
        <label :class="{finished: item.isFinished}">
          <input type="checkbox" :checked="item.isFinished" @click="toggleFinish(item)"/>
          {{item.label}}
        </label>
        <span @click="delThis(index)">x</span>
      </div>
    </form>
    <div id="clc">
      <span @click="clcFinish">{{clcF}}</span>
      <span @click="reset">{{clc}}</span>
    </div>
  </div>
</template>

<script>
import Store from './store'
export default {
  data () {
    return {
      title: 'Todo List',
      submit: '提交',
      clc: 'Clear All',
      clcF: 'Clear Finished',
      items: Store.fetch(),
      newItem: ''
    }
  },
  watch: {
    items: {
      handler (items) {
        Store.save(items);
      },
      deep: true
    }
  },
  methods: {
    toggleFinish (item) {
      item.isFinished = !item.isFinished;
    },
    addNew () {
      if (this.newItem) {
        this.items.unshift({label: this.newItem, isFinished: false});
        //console.log(this.newItem);
        this.newItem = '';
      }
    },
    reset () {
      this.items = [];
      Store.clear();
    },
    clcFinish () {
      for(let i in this.items) {
        if (this.items[i].isFinished === true) {
          this.items.splice(i, 1);
        }
      }
    },
    delThis (index) {
      this.items.splice(index, 1);
    }
  }
}
</script>

<style>
*{margin:0; padding: 0;}
html {
  height: 100%;
}
body {
  display: flex;
  /*align-items: flex-start;*/
  justify-content: center;
  /*height: 100%;*/
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  /*color:  #2E8B57; */ /*#2c3e50;*/
  margin-top: 10%;
  width: 80%;
}
h1 {
  font-size: 3rem;
  color:  #2E8B57;
}
#write {
  width: 72%;
  height: 2rem;
  margin: 20px 0;
  border: 1px solid #ccc;
  border-radius: 0.2rem;
}
#write:focus {
  outline: 0;
  border: 1px solid #39B662;
  box-shadow: 0 0 4px #39B662;
}
button {
  margin-left: 2%;
  vertical-align: middle;
  width: 23%;
  height: 2rem;
  background-color: #3CB371;
  border: 0;
  color: #FFF;
  border-radius: 0.3rem;
}
form {
  margin-bottom: 2rem;
}
form div {
  color:  #000;
  text-align: left;
  font-size: 1.1rem;
  margin: 0.6rem 0;
}
label {
  display: inline-block;
  width: 93%;
  word-wrap: break-word;
}
label input {
  width: 1.2rem;
  height: 1.2rem;
  vertical-align: middle;
  margin-bottom: 2px;
}
form span {
  color: #F00;
}
.finished {
  color: #39B662;
}
#clc {
  width: 100%;
  position: fixed;
  bottom: 0;
  /*left: 50%;*/
  margin-left: -10%;
  cursor: pointer;
  background-color: #39B662;
  color: #FFF;
  height: 2rem;
}
#clc span:first-child {
  border-right: 1px solid #FFF;
  padding-right: 3%;
}
#clc span {
  display: inline-block;
  width: 40%;
  height: 2rem;
  line-height: 2rem;
}
</style>
