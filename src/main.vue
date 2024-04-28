<script setup>  
 import { ref,watch,onMounted } from 'vue';

  const notes = ref([
                     {
                       title: "社内業務",
                       content: "事務処理や、在庫状況の問い合わせなど",
                       color: "brown",
                     },
                     {
                       title: "営業訪問",
                       content: "取引先の事業所を訪問し、商談を行う",
                       color: "gray",
                     },
                     {
                       title: "朝礼",
                       content: "部署の朝礼に参加し、全社の営業目標の進捗などを確認",
                       color: "beige",
                     },
                   ]);

  let getTitle = ref("");
  let getContent = ref("");
  let getColor = ref("");

  let add = function(){
  if (!getTitle.value || !getContent.value) {
    alert('タイトルと内容を両方入力してください');
    return;
  }
  else if (!getColor.value ) {
    alert('カラーを選択してください');
    return;
  }
  else{
    notes.value.push({
      title: getTitle.value,
      content: getContent.value,
      color: getColor.value
    });
    getTitle.value = "";
    getContent.value = "";
    getColor.value = "";
  };

  }
    
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
 
 <div class="banner">
    <div class="container">
      <div class="input-group">
        <input v-model="getTitle" type="text" placeholder="タイトル"> 
        <textarea v-model="getContent" placeholder="内容"></textarea>

          <select v-model="getColor">
            <option value="" selected disabled >カラーを選択</option>
            <option value="yellow">黄色</option>
            <option value="brown">茶色</option>
            <option value="gray">灰色</option>
            <option value="beige">ベージュ</option>
            <option value="pink">ピンク</option>
            <option value="lightGray">ライト・グレイ</option>
          </select>

          <button class="addBtn" @click="add">追加</button>

      </div>

      <div class="note-group">
        <div v-for="(item, index) in notes" class = "note" :class="item.color" :key="index" >
          <h3 class="title">
          {{ item.title }}
          </h3>
          <p class="content">
            {{ item.content }}
          </p>
        <button class="delBtn" @click="deleteBtn(index)"> <i class="fa-regular fa-circle-xmark"></i></button>
        </div>
      </div>
    </div>

    
  </div>

</template>

<style scoped>
 /* .container{
   
   background-color:#efe5ca;
   
 } */
</style>