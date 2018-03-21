<template>
  <div class="flex" :class="classes" >
    <slot></slot> 
  </div>
</template>

<script>
  /**
   * A helper component for using flexbox
   */
  export default {
    name: 'Flex',
    props: {
      column: {
        type: Boolean,
        default: false,
      },
      auto: {
        type: Boolean,
        default: false,
      },
      justify: {
        type: String,
      },
      align: {
        type: String,
      },
      wrap: {
        type: Boolean,
        default: false,
      }
    },
    computed: {
      classes: function() {
        let classes = [];
        if (this.column) {
          classes.push('flex-col');
        }
        if (this.auto) {
          classes.push('flex-auto');
        } 
        if (this.justify) {
          classes.push(`flex-justify__${this.justify}`);
        }
        if (this.align) {
          classes.push(`flex-align__${this.align}`);
        }
        if (this.wrap) {
          classes.push('flex-wrap');
        }
        return classes;
      }
    }
  } 
</script>

<style>
  .flex {
    display: flex;
  }
  .flex-col {
    flex-direction: column;
  }
  .flex-auto {
    flex: 1 1 auto;
  }
  .flex-wrap {
    flex-wrap: wrap;
  }
  .flex-justify__start {
    justify-content: flex-start;
  }
  .flex-justify__center {
    justify-content: center;
  }
  .flex-justify__end {
    justify-content: flex-end;
  }
  .flex-align__start {
    align-items: flex-start;
  }
  .flex-align__center {
    align-items: center;
  }
  .flex-align__end {
    align-items: flex-end;
  }
</style>