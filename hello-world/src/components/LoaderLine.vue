<template>
    <div class="wrapper">
        <div class="loading">
            <div class="dot"></div>
             <div class="dot"></div>
              <div class="dot"></div>
               <div class="dot"></div>
                <div class="dot"></div>
        </div>
    </div>
  

</template>

<script>
export default {

}
</script>

<style scoped>
.wrapper {
  display: flex;
  justify-content: center;
  flex-direction: column;
  gap: 20px;
  align-items: center;
  /* position: fixed;
  top: 23px;
  left: 50%;
  transform: translateY(-50%);
  z-index: 99999; */
}
/* loading wave */

.loading {
  /* $opacities: 0.6, 0.7, 0.8, 0.9, 1; */
  display: flex;

  
}
.dot {
    position: relative;
    width: 3px;
    height: 3px;
    margin: 3px;
    border-radius: 50%;
}

    .dot::before {
      position: absolute;
      content: "";
      width: 100%;
      height: 100%;
      background: green;

      border-radius: inherit;
      animation: wave 1s ease-out infinite;
    }

    @for $i from 1 through 5 {
      &:nth-child(#{$i}) {
        //background: nth($colors, $i);
        //opacity: nth($opacities, $i);

        &::before {
          animation-delay: $i * 0.1s;
        }
      }
    }
  

@keyframes wave {
  10%,
  25% {
    background: green;
     transform: scale(2.5);
  }

  80%,
  100% {
    opacity: 0;
  }
}


</style>
