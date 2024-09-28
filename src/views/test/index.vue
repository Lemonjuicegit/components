<script setup>

const option = [
  {
    title:'第一项'
  },
    {
    title:'第二项1111111111111'
  },
      {
    title:'第三项'
  }
]
const aaa = ref('')
const onchange = () => {
  console.log(aaa.value)
}
</script>
<template :prop>  

  <Dropdown :isArrowIcon="true":option="option" trigger="hover" :maxWidth="60" :isEditColor="true">test</Dropdown>
<div class="dropdown">  
  <button class="dropbtn">下拉菜单</button>  
  <div class="dropdown-content">  
    <a href="#">链接 1</a>  
    <a href="#">链接 2</a>  
    <a href="#">链接 3</a>  
  </div>  
</div>
</template>
<style scoped>
/* 容器样式 */  
.dropdown {  
  position: relative;  
  display: inline-block;  
}  
  
/* 下拉按钮样式 */  
.dropbtn {  
  background-color: #4CAF50;  
  color: white;  
  padding: 16px;  
  font-size: 16px;  
  border: none;  
  cursor: pointer;
}  
  
/* 下拉菜单样式 */  
.dropdown-content {  
  display: none;  
  position: absolute;  
  background-color: #f9f9f9;  
  min-width: 160px;  
  box-shadow: 0px 8px 16px 0px rgba(0,0,0,0.2);  
  z-index: 1;  
}  
  
/* 链接样式 */  
.dropdown-content a {  
  color: black;  
  padding: 12px 16px;  
  text-decoration: none;  
  display: block;  
}  
  
/* 鼠标悬停在链接上时的样式 */  
.dropdown-content a:hover {background-color: #f1f1f1}  
  
/* 显示下拉菜单 */  
.dropdown:hover .dropdown-content {  
  display: block;  
}  
  
/* 可选：添加动画效果 */  
.dropdown-content {  
  animation-name: fadeIn;  
  animation-duration: 0.4s;  
}  
  
@keyframes fadeIn {  
  from {opacity: 0;}  
  to {opacity: 1;}  
}

</style>
