<template>
  <component :is="component" :id="id" ref="elRef" :class="c.base">
    <k-toolbar :outline="outline" :colors="colors">
      <div v-if="slots.left" :class="c.left"><slot name="left" /></div>
      <div :class="c.messagebarArea">
        <textarea
          id="textareaId"
          ref="areaElRef"
          type="textarea"
          :class="c.messagebarInput"
          :placeholder="placeholder"
          :name="name"
          :value="value"
          :disabled="disabled"
          :size="size"
          @input="onInput"
          @change="onChange"
          @focus="onFocusInternal"
        />
      </div>
      <div v-if="slots.right" :class="c.right"><slot name="right" /></div>
    </k-toolbar>
  </component>
</template>
<script>
  import { ref, computed } from 'vue';
  import { MessagebarClasses } from '../../shared/classes/MessagebarClasses.js';
  import { MessagebarColors } from '../../shared/colors/MessagebarColors.js';
  import { useDarkClasses } from '../shared/use-dark-classes.js';
  import { useThemeClasses } from '../shared/use-theme-classes.js';
  import kToolbar from './Toolbar.vue';

  export default {
    name: 'k-messagebar',
    components: {
      kToolbar,
    },
    props: {
      component: {
        type: String,
        default: 'div',
      },
      colors: {
        type: Object,
      },
      ios: {
        type: Boolean,
        default: undefined,
      },
      material: {
        type: Boolean,
        default: undefined,
      },
      id: String,
      name: { type: String, default: undefined },
      value: { type: [Number, String], default: undefined },
      placeholder: { type: String, defaullt: 'Message' },
      outline: { type: Boolean, default: false },
      rightClass: { type: String, default: '' },
      leftClass: { type: String, default: '' },
      disabled: { type: Boolean, default: undefined },
      textareaId: String,
      size: { type: [Number, String], default: undefined },
    },
    emits: ['change', 'input', 'focus'],
    setup(props, ctx) {
      const elRef = ref(null);
      const areaElRef = ref(null);
      const isFocused = ref(false);

      const colors = computed(() =>
        MessagebarColors(props.colors || {}, useDarkClasses)
      );

      const onFocusInternal = (e) => {
        isFocused.value = true;
        ctx.emit('focus', e);
      };

      const c = useThemeClasses(props, () =>
        MessagebarClasses(
          {
            ...props,
          },
          colors.value,
          { isFocused }
        )
      );

      const onChange = (e) => {
        ctx.emit('change', e);
      };

      const onInput = (e) => {
        ctx.emit('input', e);
      };

      return {
        elRef,
        areaElRef,
        c,
        onChange,
        onInput,
        onFocusInternal,
        slots: ctx.slots,
      };
    },
  };
</script>
