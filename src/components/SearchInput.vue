<template>
  <div class="el-input">
    <input 
      type="text" 
      class="el-input__inner" 
      :placeholder="placeholder" 
      :value="value" 
      @change="onChange"
      @keyup.enter="onEnter"
    /> 
  </div>
</template>

<script>
  // Necesarry component because elements event handling on inputs is trash 
  // Note that it is controlled rather than using v-model so that the parent
  // can access the input value.
  export default {
    props: {
      onChange: {
        type: Function,
        required: true,
      },
      placeholder: {
        type: String,
        default: '',
      },
      value: {
        type: String,
        required: true,
      },
      onEnter: {
        type: Function,
        default: function() {
          return () => {};
        }
      },
    }
  }
</script>