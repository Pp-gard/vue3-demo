<template>
  <div>{{man}}</div>
  <hr>
  <div>shallowRef:{{ man2 }}</div>
  <hr>
  <div>customRef: {{ man3 }}</div>
  <button @click="change">change</button>
</template>

<script setup lang='ts'>
import { ref, Ref, isRef, shallowRef, triggerRef, customRef } from 'vue'

function MyRef<T>(value: T) {
  return customRef((track, trigger) => {
    let timer: any
    return {
      get() {
        track()
        return value
      },
      set(nval) {
        clearTimeout(timer)
        timer = setTimeout(() => {
          console.log(1);
          value = nval
          timer = null;
          trigger()
        }, 500)
      }
    }
  })
}
const man3 = MyRef<string>('customRef小曼')
type M = {
  name: string
}
// let man = ref<M>({name: '小曼'})
// ref: 深层响应, shallowRef: 浅层响应
// ref 和 shallowRef 不能一块儿写.会影响shallowRef视图更新
let man:Ref<M> = ref({name: '小曼'})
  let man2:Ref<M> = shallowRef({name: '小曼2'})
const change = () => {
  // man.value.name = '我是ref'
  // man2.value.name = '我被影响了'
  // triggerRef(man2)
  // man2.value = {
  //   name: '大曼2'
  // }
  // console.log(isRef(man));
  man3.value = '我变了'
  console.log(man3);
  
}
</script>
<style scoped>

</style>