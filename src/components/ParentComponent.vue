<!-- 父组件 -->
<template>
    <div>
      <ChildComponent />
    </div>
  </template>
  
  <script>
  import ChildComponent from './ChildComponent.vue';
  
  export default {
    provide: {
      message: 'Hello from ParentComponent',
    },
    components: {
      ChildComponent,
    },
  };
  </script>
  
  //上面provide还可以写成函数形式
  export default {
      provide(){
          return {
                message: this.message
          }
      }
  }
  