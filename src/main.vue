<script setup>  
 import { ref,watch,onMounted } from 'vue';

  const notes = ref([
                     {
                       title: "春節行程安排",
                       content: "吃飽睡，睡飽吃",
                       color: "red",
                     },
                     {
                       title: "工作待辦事項",
                       content: "詢問各家廠商報價",
                       color: "green",
                     },
                     {
                       title: "運動健身計畫",
                       content: "每天早上六點去健身",
                       color: "blue",
                     },
                   ]);

  let getTitle = ref("");
  let getContent = ref("");
  let getColor = ref("");

  let add = function(){
    notes.value.push({
      title: getTitle.value,
      content: getContent.value,
      color: getColor.value
    });
    getTitle.value = "";
    getContent.value = "";
    getColor.value = "";
  };

  const deleteBtn = function(index){
    notes.value.splice(index, 1);
  }
  
  
  // 載入存儲中的備忘錄
  onMounted(() => {
    const savedMemos = JSON.parse(localStorage.getItem('savedMemo'));
    if (savedMemos) {
      notes.value = savedMemos;
    }
  });

  // 監聽備忘錄的變化，並自動儲存到本地 localStorage
  watch(notes, (newMemos) => {
    localStorage.setItem('savedMemo', JSON.stringify(newMemos));
  }, { deep: true });

</script> 

<template>
 
 <div class="container">
    <div class="input-group">
      <input v-model="getTitle" type="text" placeholder="標題"> 
      <textarea v-model="getContent" placeholder="內容"></textarea>

        <select v-model="getColor">
          <option value="" selected disabled >選擇顏色</option>
          <option value="red">紅</option>
          <option value="green">綠</option>
          <option value="blue">藍</option>
        </select>

        <button class="addBtn" @click="add">新增</button>

    </div>
    <div class="note-group">
      <div v-for="(item, index) in notes" class = "note" :class="item.color" :key="index" >
        <h3 class="title">
         {{ item.title }}
        </h3>
        <p class="content">
           {{ item.content }}
        </p>
       <button class="delBtn" @click="deleteBtn(index)"> X </button>
      </div>
    </div>
  </div>

</template>

<style scoped>
  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
    letter-spacing: 1px;
  }
 .container{
   background-color:#efe5ca;
   min-height:100vh;
   margin:0 auto;
   padding:12px;


 }
  .note-group{
    display:flex;
    justify-content:center;
  }
  .note{
    margin: 1rem;
    text-align: center;
    padding: 2rem;
    border-radius: .5rem;
    box-shadow: 2px 2px 2px 3px #49462c;
    position: relative;
  }
  h3{
    font-size:20px;
    color:#333333;
  }
  .red{
   background-color: #d9a891;
  }
  .green{
    background-color: #b4cf8f;
  }
  .blue{
    background-color: #b9d0d0;
  }
  .input-group {
    width: 650px;
    margin: 56px auto;
  }
    input,
    textarea,
    select,
    .addBtn {
      display:block;
      width: 100%;
      padding: 8px;
      margin-bottom:12px;
      border-radius: 3px;
      border:1px solid #c8baa7;
    }
  .addBtn{
    background-color:#c4972f;
    cursor:pointer;
    transition: all .2s;
    border:none;
  }
  .addBtn:hover{
    box-shadow: 4px 3px 2px #49462c;
  }
  .addBtn:active{
    box-shadow: none;
  }
  input:focus,
  textarea:focus{
    outline: 1px solid #b4a468;
    border:1px solid #b4a468;
  }
  .delBtn{
    font-weight:bold;
    padding:8px 10px;
    color:#ffffff;
    background-color: #999999;
    border:none;
    border-radius: 15px;
    position: absolute;
    top:-12px;
    right:-12px;
    cursor:pointer;
  }
  .delBtn:hover{
    background-color: #b7282d;
  }
</style>