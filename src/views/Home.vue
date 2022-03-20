<template>
  <div class="w-full h-full text-2xl text-blue-600">
    <div class="door-wrap relative w-full h-full">
      <div class="door-back bg-black w-full h-full absolute top-0 left-0">

      </div>
      <div 
        class="door w-full h-full flex justify-center items-center bg-gray-700 text-white" 
        :style="doorStyle"
      >
        <div 
          @touchstart="startSliding"
          @touchmove="sliding"
          @touchend="endSliding"
          class="w-16 h-16 border-2 border-white absolute top-1/2 left-5 transform -translate-y-1/2"
        >
          handle
        </div>
        open the door please
      </div>
    </div>
  </div>
</template>

<script>
import { ref, computed } from "vue";

export default {
  setup() {
    const diff = ref(0);
    let startPosition;
    let currentPosition = 0;

    const doorStyle = computed(() => {
      const amount = currentPosition + diff.value;

      return {
        transform: `translateX(${amount >= 0 ? amount : 0}px)`
      }
    });
    const startSliding = (e) => {
      console.log("start sliding");
      startPosition = e.touches[0].clientX;
    };
    const sliding = (e) => {
      const amount = e.touches[0].clientX - startPosition;

      diff.value = amount;
    };
    const endSliding = () => {
      currentPosition += diff.value;
    };

    /**
     * 문을 슬라이드한다.
     * 손가락을 터치(touchstart)해서 옆으로 민다(touchmove).
     * 좌표는 e.touches[0].clientX에 들어있음
     * 문을 슬라이드 하기:
     * 1. touchstart 할 때 좌표를 저장해놓는다
     * 2. touchmove가 발생하면 move 이벤트의 clientX - start의 clientX를 구한다.
     * 그 값이 문이 움직인 크기임!
     * 3. 문을 그 값만큼 translateX 해준다.
     * 4. 터치가 끝나면 그 값을 저장을 해둔다.
     * 5. 다시 터치가 되면 그 값 + 움직인 거리만큼 문을 이동한다.
     */

    return {
      diff,
      doorStyle,
      startSliding,
      sliding,
      endSliding
    }
  }
  
};
</script>

<style></style>
