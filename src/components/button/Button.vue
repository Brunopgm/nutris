<template>
  <component
    :is="href ? 'a' : 'button'"
    :class="buttonClass"
    :href="href"
    :role="href ? '' : 'button'"
    :aria-busy="isLoading"
    @click="$emit('click')"
  >
    <Spinner
      v-if="isLoading"
      :color="spinnerColor"
      :size="spinnerSize"
      class="loading"
    />
    <span v-if="startIcon" class="startIcon" />
    <span class="children">
      <slot />
    </span>
    <span v-if="endIcon" class="endIcon" />
  </component>
</template>

<script lang="ts">
import { Options, Vue } from 'vue-class-component'
import { Prop } from 'vue-property-decorator'
import Spinner from '@/components/spinner/Spinner.vue'

@Options({
  components: {
    Spinner,
  },
})
export default class Button extends Vue {
  @Prop({ default: 'default' }) color!: string
  @Prop({ default: 'filled' }) variant!: string
  @Prop({ default: 'round-square' }) shape!: string
  @Prop({ default: 'medium' }) size!: string
  @Prop({ default: true }) isLoading!: boolean
  @Prop({ default: '' }) startIcon!: string
  @Prop({ default: '' }) endIcon!: string
  @Prop() href!: string

  get buttonClass() {
    return [
      'button',
      this.color,
      this.variant,
      this.shape,
      this.size,
      this.isLoading ? 'isLoading' : '',
    ]
  }

  get spinnerColor() {
    return this.variant === 'filled' ? `${this.color}-contrast` : this.color
  }

  get spinnerSize() {
    return this.size === 'medium' ? 24 : 36
  }
}
</script>

<style lang="scss" scoped>
.button {
  align-items: center;
  background-color: transparent;
  border: 1px solid transparent;
  border-radius: var(--space-lg);
  cursor: pointer;
  display: inline-flex;
  justify-content: center;
  letter-spacing: var(--typography-button-letter);
  outline: 0;
  overflow: hidden;
  position: relative;
  text-decoration: none;
  transition-duration: var(--transition-duration-speed);
  transition-property: border-color;
  will-change: border-color;

  &::before {
    border-radius: var(--space-lg);
    bottom: 0;
    content: '';
    left: 0;
    opacity: 0;
    position: absolute;
    right: 0;
    top: 0;
    transition-duration: var(--transition-duration-speed);
    transition-property: opacity;
  }

  &[disabled] {
    cursor: not-allowed;

    &.text {
      color: var(--disabled-color);
    }

    &.outlined {
      border-color: var(--disabled-color);
      color: var(--disabled-color);
    }

    &.filled {
      background-color: var(--disabled-color);
      color: var(--disabled-color-contrast);
    }
  }
}

.children {
  display: inherit;
  position: relative;

  img {
    max-width: none;
  }
}

.startIcon {
  display: inline-block;
  position: relative;

  &:not(:empty) {
    margin-right: var(--space-xs);
  }
}

.endIcon {
  display: inline-block;
  position: relative;

  &:not(:empty) {
    margin-left: var(--space-xs);
  }
}

.isLoading {
  cursor: default;

  .children,
  .startIcon,
  .endIcon {
    opacity: 0;
  }
}

.default {
  &.text {
    color: var(--default-color);

    &:not([disabled]):not(.isLoading):hover {
      &::before {
        background-color: var(--default-color-darken);
        opacity: 0.1;
      }
    }
  }

  &.outlined {
    border-color: var(--default-color);
    color: var(--default-color);

    &:not([disabled]):not(.isLoading):hover {
      &::before {
        background-color: var(--default-color-darken);
        opacity: 0.1;
      }
    }
  }

  &.filled {
    background-color: var(--default-color);
    color: var(--default-color-contrast);

    &:not([disabled]):not(.isLoading):hover {
      &::before {
        background-color: var(--default-color-darken);
        opacity: 1;
      }
    }
  }
}

.primary {
  &.text {
    color: var(--primary-color);

    &:not([disabled]):not(.isLoading):hover {
      &::before {
        background-color: var(--primary-color-darken);
        opacity: 0.1;
      }
    }
  }

  &.outlined {
    border-color: var(--primary-color);
    color: var(--primary-color);

    &:not([disabled]):not(.isLoading):hover {
      &::before {
        background-color: var(--primary-color-darken);
        opacity: 0.1;
      }
    }
  }

  &.filled {
    background-color: var(--primary-color);
    color: var(--primary-color-contrast);

    &:not([disabled]):not(.isLoading):hover {
      &::before {
        background-color: var(--primary-color-darken);
        opacity: 1;
      }
    }
  }
}

.secondary {
  &.text {
    color: var(--secondary-color);

    &:not([disabled]):not(.isLoading):hover {
      &::before {
        background-color: var(--secondary-color-darken);
        opacity: 0.1;
      }
    }
  }

  &.outlined {
    border-color: var(--secondary-color);
    color: var(--secondary-color);

    &:not([disabled]):not(.isLoading):hover {
      &::before {
        background-color: var(--secondary-color-darken);
        opacity: 0.1;
      }
    }
  }

  &.filled {
    background-color: var(--secondary-color);
    color: var(--secondary-color-contrast);

    &:not([disabled]):not(.isLoading):hover {
      &::before {
        background-color: var(--secondary-color-darken);
        opacity: 1;
      }
    }
  }
}

.medium {
  height: var(--space-md);
  padding-left: var(--space);
  padding-right: var(--space);

  .children {
    font: var(--typography-button-font);
  }
}

.large {
  height: var(--space-lg);
  padding-left: var(--space);
  padding-right: var(--space);

  .children {
    font: var(--typography-title-font);
  }
}

.round-square {
  min-width: var(--space-xlg);
}

.loading {
  position: absolute;
  z-index: var(--zindex-1);
}

.bold {
  font-weight: bold;
}
</style>