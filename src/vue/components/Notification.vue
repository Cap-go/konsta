<template>
  <component :is="component" v-if="theme === 'ios'" ref="elRef" :class="c.base">
    <div :class="c.header">
      <div v-if="icon || slots.icon" :class="c.icon">
        {{ icon }}<slot name="icon" />
      </div>
      <div v-if="title || slots.title" :class="c.title">
        {{ title }}<slot name="title" />
      </div>
      <div
        v-if="titleRightText || slots.titleRightText"
        :class="c.titleRightText"
      >
        {{ titleRightText }}<slot name="titleRightText" />
      </div>
      <div
        v-if="button || slots.button"
        :class="c.button"
        role="button"
        tabindex="0"
        @click="onClose"
      >
        <delete-icon :theme="theme" :class="c.deleteIcon" />
        <slot name="button" />
      </div>
    </div>
    <div :class="c.content">
      <div v-if="subtitle || slots.subtitle" :class="c.subtitle">
        {{ subtitle }}<slot name="subtitle" />
      </div>
      <div v-if="text || slots.text" :class="c.text">
        {{ text }}<slot name="text" />
      </div>
      <slot />
    </div>
  </component>
  <component :is="component" v-else ref="elRef" :class="c.base">
    <div :class="c.header">
      <div v-if="icon || slots.icon" :class="c.icon">
        {{ icon }}<slot name="icon" />
      </div>
      <div :class="c.contentWrapper">
        <div :class="c.contentTitle">
          <div v-if="title || slots.title" :class="c.title">
            {{ title }}<slot name="title" />
          </div>
          <div
            v-if="titleRightText || slots.titleRightText"
            :class="c.titleRightText"
          >
            {{ titleRightText }}<slot name="titleRightText" />
          </div>
        </div>
        <div :class="c.content">
          <div v-if="subtitle || slots.subtitle" :class="c.subtitle">
            {{ subtitle }}<slot name="subtitle" />
          </div>
          <div v-if="text || slots.text" :class="c.text">
            {{ text }}<slot name="text" />
          </div>
          <slot />
        </div>
      </div>
      <div
        v-if="button || slots.button"
        :class="c.button"
        role="button"
        tabindex="0"
        @click="onClose"
      >
        <delete-icon :theme="theme" :class="c.deleteIcon" />
        <slot name="button" />
      </div>
    </div>
  </component>
</template>
<script>
  import { computed } from 'vue';
  import { NotificationsClasses } from '../../shared/classes/NotificationsClasses.js';
  import { NotificationsColors } from '../../shared/colors/NotificationsColors.js';
  import { useDarkClasses } from '../shared/use-dark-classes.js';
  import { useThemeClasses } from '../shared/use-theme-classes.js';
  import { useTheme } from '../shared/use-theme.js';
  import DeleteIcon from './icons/DeleteIcon.vue';

  export default {
    name: 'k-notification',
    components: {
      DeleteIcon,
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
      opened: {
        type: Boolean,
        default: undefined,
      },
      translucent: {
        type: Boolean,
        default: true,
      },
      button: { type: String },
      icon: { type: String },
      title: { type: [String, Number] },
      titleRightText: { type: [String, Number] },
      subtitle: { type: [String, Number] },
      text: { type: [String, Number] },
    },
    emits: ['close'],
    setup(props, ctx) {
      const theme = useTheme();

      const colors = computed(() =>
        NotificationsColors(props.colors || {}, useDarkClasses)
      );

      const c = useThemeClasses(props, () =>
        NotificationsClasses(props, colors.value, ctx.attrs.class)
      );
      const onClose = () => {
        ctx.emit('close');
      };
      return {
        c,
        theme,
        onClose,
        slots: ctx.slots,
      };
    },
  };
</script>
