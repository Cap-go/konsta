<template>
  <component :is="component" ref="elRef" :class="c.base">
    <slot />
  </component>
</template>
<script>
  import { ref, computed } from 'vue';
  import { TableCellClasses } from '../../shared/classes/TableCellClasses.js';
  import { TableCellColors } from '../../shared/colors/TableCellColors.js';
  import { useDarkClasses } from '../shared/use-dark-classes.js';
  import { useThemeClasses } from '../shared/use-theme-classes.js';

  export default {
    name: 'k-table-cell',
    props: {
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
      header: {
        type: Boolean,
        default: false,
      },
    },
    setup(props, ctx) {
      const elRef = ref(null);
      const component = computed(() => (props.header ? 'th' : 'td'));

      const colors = computed(() =>
        TableCellColors(props.colors || {}, useDarkClasses)
      );
      const c = useThemeClasses(props, () =>
        TableCellClasses(props, colors.value, ctx.attrs.class)
      );

      return {
        c,
        elRef,
        component,
      };
    },
  };
</script>
