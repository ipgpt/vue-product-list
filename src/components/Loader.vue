<template>
  <div class="lds-ripple">
    <div></div>
    <div></div>
  </div>
</template>

<style scoped lang="scss">
.lds-ripple {
  display: inline-block;
  position: relative;
  width: 30px;
  height: 30px;

  & div {
    position: absolute;
    border: 2px solid green;
    opacity: 1;
    border-radius: 50%;
    animation: lds-ripple 1s cubic-bezier(0, 0.2, 0.8, 1) infinite;

    &:nth-child(2) {
      animation-delay: -0.5s;
    }
  }
}

@keyframes lds-ripple {
  0% {
    top: 12px;
    left: 12px;
    width: 0;
    height: 0;
    opacity: 1;
  }
  100% {
    top: 0px;
    left: 0px;
    width: 24px;
    height: 24px;
    opacity: 0;
  }
}
</style>
