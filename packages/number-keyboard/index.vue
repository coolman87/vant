<template>
  <transition :name="transition ? 'van-slide-bottom' : ''">
    <div 
      v-show="show"
      :style="style"
      class="van-number-keyboard"
      :class="`van-number-keyboard--${theme}`"
      @animationend="onAnimationEnd"
      @webkitAnimationEnd="onAnimationEnd"
    >
      <div class="van-number-keyboard__title van-hairline--top" v-if="title || showTitleClose">
        <span v-text="title" />
        <span
          class="van-number-keyboard__close"
          v-if="showTitleClose"
          v-text="closeButtonText"
          @click="onBlur"
        />
      </div>
      <div class="van-number-keyboard__body">
        <key
          v-for="(key, index) in keys"
          :key="index"
          :text="key.text"
          :type="key.type"
          @press="onPressKey"
        />
      </div>
      <div class="van-number-keyboard__sidebar" v-if="theme === 'custom'">
        <key :text="'delete'" :type="['delete', 'big']" @press="onPressKey" />
        <key :text="closeButtonText" :type="['green', 'big']" @press="onPressKey" />
      </div>
    </div>
  </transition>
</template>

<script>
import { create } from '../utils';
import Key from './Key';

export default create({
  name: 'van-number-keyboard',

  components: { Key },

  props: {
    show: Boolean,
    title: String,
    closeButtonText: String,
    theme: {
      type: String,
      default: 'default'
    },
    extraKey: {
      type: String,
      default: ''
    },
    zIndex: {
      type: Number,
      default: 100
    },
    transition: {
      type: Boolean,
      default: true
    },
    showDeleteKey: {
      type: Boolean,
      default: true
    },
    hideOnClickOutside: {
      type: Boolean,
      default: true
    }
  },

  mounted() {
    this.handler(true);
  },

  destroyed() {
    this.handler(false);
  },

  activated() {
    this.handler(true);
  },

  deactivated() {
    this.handler(false);
  },

  watch: {
    show() {
      if (!this.transition) {
        this.$emit(this.show ? 'show' : 'hide');
      }
    }
  },

  computed: {
    keys() {
      const keys = [];
      for (let i = 1; i <= 9; i++) {
        keys.push({ text: i });
      }

      switch (this.theme) {
        case 'default':
          keys.push(
            { text: this.extraKey, type: ['gray'] },
            { text: 0 },
            { text: 'delete', type: ['gray', 'delete'] }
          );
          break;
        case 'custom':
          keys.push(
            { text: 0, type: ['middle'] },
            { text: this.extraKey }
          );
          break;
      }

      return keys;
    },

    style() {
      return {
        zIndex: this.zIndex
      };
    },

    showTitleClose() {
      return this.closeButtonText && this.theme === 'default';
    }
  },

  methods: {
    handler(action) {
      if (action !== this.handlerStatus && this.hideOnClickOutside) {
        this.handlerStatus = action;
        document.body[(action ? 'add' : 'remove') + 'EventListener']('touchstart', this.onBlur);
      }
    },

    onBlur() {
      this.$emit('blur');
    },

    onAnimationEnd() {
      this.$emit(this.show ? 'show' : 'hide');
    },

    onPressKey(text) {
      if (text === '') {
        return;
      }

      if (text === 'delete') {
        this.$emit('delete');
      } else if (text === this.closeButtonText) {
        this.onBlur();
      } else {
        this.$emit('input', text);
      }
    }
  }
});
</script>
