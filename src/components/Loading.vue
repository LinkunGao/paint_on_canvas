<template>
  <div class="flex-center head">
    <div class="loading-grid">
      <!-- two balls -->
      <div class="flex-center">
        <div class="loading two-balls">
          <div class="pink"></div>
          <div class="blue"></div>
        </div>
      </div>
      <!-- three balls -->
      <div class="flex-center">
        <div class="loading three-balls-bounce">
          <div class="circle"></div>
          <div class="circle"></div>
          <div class="circle"></div>
          <div class="shadow"></div>
          <div class="shadow"></div>
          <div class="shadow"></div>
        </div>
      </div>
      <!-- four balls -->
      <div class="flex-center">
        <div class="loading four-balls"></div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts"></script>

<style lang="scss" scoped>
* {
  margin: 0;
  box-sizing: border-box;
}
.head {
  width: 100vw;
  height: 100vh;
  //   z-index: 10;
}
.flex-center {
  display: flex;
  justify-content: center;
  align-items: center;
}

.loading-grid {
  display: grid;
  grid-template-columns: repeat(3, 240px);
  grid-template-rows: repeat(3, 240px);
}
.two-balls {
  position: relative;
  width: 44px;
}
.two-balls div {
  position: absolute;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  animation: two-balls-pink 1.05s ease-in-out infinite;
}

.two-balls .pink {
  background-color: #ff4a69;
  left: -2px;
}

.two-balls .blue {
  background-color: #1342ed;
  animation-name: two-balls-blue;
  right: -2px;
}

@keyframes two-balls-pink {
  from {
    z-index: 1;
  }
  50% {
    transform: translateX(calc(20px + 3 * 2px));
  }
}
@keyframes two-balls-blue {
  50% {
    transform: translateX(calc(-20px + 3 * 2px));
  }
}

// three-balls-bounce

.three-balls-bounce {
  position: relative;
  width: 200px;
  height: 62px;
}
.three-balls-bounce .circle {
  width: 20px;
  height: 20px;
  position: absolute;
  border-radius: 50%;
  background-color: #ff4a69;
  left: 15%;
  transform-origin: 50%;
  animation: balls-circle 0.5s alternate ease infinite;
}
.three-balls-bounce .shadow {
  width: 20px;
  height: 4px;
  border-radius: 50%;
  background-color: rgba(0, 0, 0, 0.4);
  position: absolute;
  transform-origin: 50%;
  top: 62px;
  z-index: 1;
  left: 15%;
  filter: blur(1px);
  animation: balls-shadow 0.5s alternate ease infinite;
}
.three-balls-bounce .circle:nth-child(2),
.three-balls-bounce .shadow:nth-child(4) {
  left: 45%;
  animation-delay: 0.2s;
}
.three-balls-bounce .circle:nth-child(3),
.three-balls-bounce .shadow:nth-child(5) {
  left: auto;
  right: 15%;
  animation-delay: 0.3s;
}

@keyframes balls-circle {
  from {
    top: 60px;
    height: 5px;
    border-radius: 50px 50px 25px 25px;
    transform: scaleX(1.7);
  }
  40% {
    height: 20px;
    border-radius: 50%;
    transform: scaleX(1);
  }
  to {
    top: 0%;
  }
}

@keyframes balls-shadow {
  from {
    transform: scaleX(1.5);
  }
  40% {
    transform: scaleX(1);
    opacity: 0.7;
  }
  to {
    transform: scaleX(0.2);
    opacity: 0.4;
  }
}

// three balls end

// four balls start

.four-balls {
  height: 50px;
  width: 50px;
  animation: four-balls-rotate 1s infinite;
}
.four-balls:before,
.four-balls:after {
  border-radius: 50%;
  content: "";
  display: block;
  width: 25px;
  height: 25px;
}

.four-balls::before {
  animation: ball1 1s infinite;
  background-color: #cb2025;
  box-shadow: 30px 0 0 #f8b334;
  margin-bottom: 10px;
}
.four-balls::after {
  animation: ball2 1s infinite;
  background-color: #00a096;
  box-shadow: 30px 0 0 #97bf0d;
}

@keyframes four-balls-rotate {
  0% {
    transform: rotate(0deg) scale(0.8);
  }
  50% {
    transform: rotate(360deg) scale(1.2);
  }
  100% {
    transform: rotate(720deg) scale(0.8);
  }
}
@keyframes ball1 {
  0% {
    box-shadow: 30px 0 0 #f8b334;
  }
  50% {
    box-shadow: 0 0 0 #f8b334;
    margin-bottom: 0px;
    transform: translate(15px, 15px);
  }
  100% {
    box-shadow: 30px 0 0 #f8b334;
    margin-bottom: 10px;
  }
}
@keyframes ball2 {
  0% {
    box-shadow: 30px 0 0 #97bf0d;
  }
  50% {
    box-shadow: 0 0 0 #97bf0d;
    margin-top: -20px;
    transform: translate(15px, 15px);
  }
  100% {
    box-shadow: 30px 0 0 #97bf0d;
    margin-top: 0px;
  }
}
</style>
