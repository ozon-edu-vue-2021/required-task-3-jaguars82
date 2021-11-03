<template>
    <div class="load-wrapp">
        <p>Загрузка</p>
        <div class="load">
            <div class="line"></div>
            <div class="line"></div>
            <div class="line"></div>
        </div>
    </div>
</template>

<style scoped>
.load-wrapp {
  float: left;
  width: 100%;
  height: 120px;
  margin-top: 50px;
  padding: 20px 20px 20px;
  border-radius: 5px;
  text-align: center;
}

.load-wrapp:last-child {
  margin-right: 0;
}

.line {
  display: inline-block;
  width: 15px;
  height: 15px;
  margin: 0 2px;
  border-radius: 15px;
  background-color: #4b9cdb;
}
.load .line:nth-last-child(1) {
  animation: loadingB 1.5s 1s infinite;
}
.load .line:nth-last-child(2) {
  animation: loadingB 1.5s 0.5s infinite;
}
.load .line:nth-last-child(3) {
  animation: loadingB 1.5s 0s infinite;
}

@keyframes loadingB {
  0% {
    width: 15px;
  }
  50% {
    width: 35px;
  }
  100% {
    width: 15px;
  }
}
</style>