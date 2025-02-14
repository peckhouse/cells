<template>
  <Primitive
    class="cell-dropdown"
    :as
  >
    <Select.Root v-bind="forwarded">
      <Select.Trigger v-bind="forwarded" class="cell-dropdown__trigger">
        <Select.Value v-bind="forwarded" class="cell-dropdown__trigger-value" />
        <ChevronDownIcon class="cell-dropdown__trigger-icon" />
      </Select.Trigger>

      <Select.Portal>
        <Select.Content
          class="cell-dropdown__content"
          position="popper"
          data-side="bottom"
          :side-offset="4"
        >
          <Select.Viewport class="cell-dropdown__viewport">
            <Select.Group>
              <Select.Item
                v-for="(option, index) in options"
                :key="index"
                class="cell-dropdown__item"
                :value="option?.value"
                :text-value="option?.text"
              >
                <Select.ItemText class="cell-dropdown__item-text" :value="option?.value">
                  {{ option?.text }}
                </Select.ItemText>
              </Select.Item>
            </Select.Group>
          </Select.Viewport>
        </Select.Content>
      </Select.Portal>
    </Select.Root>
  </Primitive>
</template>

<script setup lang="ts">
import {
  Primitive,
  SelectRootEmits,
  SelectRootProps,
  SelectValueProps,
  useForwardPropsEmits
} from 'radix-vue'
import { Select } from 'radix-vue/namespaced'

import ChevronDownIcon from '@/assets/icons/chevron-down.svg'

defineOptions({ inheritAttrs: false })

type Option = {
  value: string
  text?: string
}

const props= withDefaults(defineProps<SelectRootProps & SelectValueProps & { options: Option[], as?: string }>(), {
  as: 'div'
})

const { options, as, ...forwardedProps } = props
const emits = defineEmits<SelectRootEmits>()

const forwarded = useForwardPropsEmits(forwardedProps, emits)
</script>

<style scoped lang="scss">
.cell-dropdown {

  &__trigger {
    @include button-reset();

    border-radius: var(--global-border-radius-medium);
    background-color: var(--c-white-100);
    border: 1px solid var(--c-gray-80);
    justify-content: space-between;
    align-items: center;
    min-width: 200px;
    padding: 0 12px;
    display: flex;
    height: 32px;
    gap: 8px;
  }

  &__trigger-value {
    color: var(--c-gray-120);
    font-size: 14px;
  }

  &__trigger-icon {
    fill: var(--c-gray-120);
    height: 16px;
    width: 16px;
  }
}

:deep(.cell-dropdown__content) {
  border-radius: var(--global-border-radius-medium);
  background-color: var(--c-white-100);
  border: 1px solid var(--c-gray-80);
  box-shadow: var(--shadow-medium);
  min-width: 200px;
  outline: none;
  padding: 8px;
  width: 100%;
  z-index: 1;

  &[data-state="open"] {
    animation: selectFadeIn var(--transition-global-duration) var(--transition-global-timing-function);
  }

  &[data-state="closed"] {
    animation: selectFadeOut var(--transition-global-duration) var(--transition-global-timing-function);
  }
}

:deep(.cell-dropdown__viewport) {
  outline: none;

  &[data-state="open"] {
    animation: selectFadeIn var(--transition-global-duration) var(--transition-global-timing-function);
  }

  &[data-state="closed"] {
    animation: selectFadeOut var(--transition-global-duration) var(--transition-global-timing-function);
  }
}

:deep(.cell-dropdown__item) {
  border-radius: var(--global-border-radius-medium);
  align-items: center;
  padding: 0px 8px;
  cursor: pointer;
  display: flex;
  outline: none;
  height: 32px;


  &[aria-selected="true"] {
    background-color: var(--c-gray-70);
    pointer-events: none;
    user-select: none;
  }

  &:hover {
    background-color: var(--c-gray-60);
    outline: none;
    border: none;
  }
}

:deep(.cell-dropdown__item-text) {
  color: var(--c-gray-120);
  font-size: 14px;
}

@keyframes selectFadeIn {
  from {
    transform: translate3d(0, -8px, 0);
    opacity: 0;
  }
  to {
    transform: translate3d(0, 0, 0);
    opacity: 1;
  }
}

@keyframes selectFadeOut {
  from {
    transform: translate3d(0, 0, 0);
    opacity: 1;
  }
  to {
    transform: translate3d(0, -8px, 0);
    opacity: 0;
  }
}
</style>
