<template>
  <section
    :class="[
      {
        'InputField--dirty': isDirty,
        'InputField--disabled': isDisabled,
        'InputField--focused': isFocused,
        'InputField--invalid': warningText,
      },
    ]"
    class="InputField"
  >
    <div
      class="InputField__wrapper relative appearance-none mb-1 inline-flex items-end w-full"
      :class="type !== 'textarea' ? 'h-12' : ''"
    >
      <slot :inputClass="inputClass" />
      <label
        v-show="label"
        class="absolute pointer-events-none text-grey truncate"
        :class="type !== 'textarea' ? 'InputField__label' : 'InputField__label__textarea'"
        >{{ label }}</label
      >
    </div>
    <p v-show="helperText || warningText" class="InputField__helper text-grey-400 text-xs mt-1">
      <slot name="helper">{{ helperText || warningText }}</slot>
    </p>
  </section>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";

@Component
export default class InputField extends Vue {
  @Prop({ default: null }) public label!: string | null;
  @Prop({ default: null }) public helperText!: string | null;
  @Prop({ default: false }) public isDirty!: boolean;
  @Prop({ default: false }) public isDisabled!: boolean;
  @Prop({ default: false }) public isFocused!: boolean;
  @Prop({ default: false }) public isInvalid!: boolean;
  @Prop({ default: null }) public warningText!: string | null;
  @Prop({ required: true }) public type!: string;

  get inputClass() {
    let inputClasses = "InputField__input w-full pt-3 left-0 bg-transparent transition text-grey";
    if (this.type !== "textarea") {
      inputClasses += " border-b border-theme-input-field-border h-10";
    }
    return inputClasses;
  }
}
</script>

<style>
.InputField--disabled {
  pointer-events: none;
}

.InputField__input:focus {
  border-color: var(--color-theme-accents);
}

.InputField__input:hover {
  border-color: var(--color-theme-accents);
}

.InputField__label {
  bottom: 0.5em;
  transform-origin: left top;
  transition: transform 150ms cubic-bezier(0.4, 0, 0.2, 1), color 150ms cubic-bezier(0.4, 0, 0.2, 1);
}

.InputField--focused .InputField__label,
.InputField--dirty .InputField__label {
  font-weight: 500;
  font-size: 1.2rem;
  bottom: 0.5em;
  transform: translate(0%, -100%) scale(0.75);
}

.InputField__label__textarea {
  top: 0.9em;
  transform-origin: left top;
  transition: transform 150ms cubic-bezier(0.4, 0, 0.2, 1), color 150ms cubic-bezier(0.4, 0, 0.2, 1);
}

.InputField--focused .InputField__label__textarea,
.InputField--dirty .InputField__label__textarea {
  @apply .font-semibold;
  top: 0.5em;
  transform: translate(0%, -100%) scale(0.75);
}

.InputField--focused .InputField__label,
.InputField--focused .InputField__label__textarea {
  color: var(--color-theme-accents);
}

.InputField--disabled .InputField__input {
  @apply .text-grey .border-dotted;
}

.InputField--invalid .InputField__label,
.InputField--invalid .InputField__helper,
.InputField--invalid .InputField__label__textarea {
  @apply .text-red-dark;
}
.InputField--invalid .InputField__input {
  @apply .border-red-dark;
}

.InputField--warning .InputField__label,
.InputField--warning .InputField__helper,
.InputField--warning .InputField__label__textarea {
  @apply .text-orange-dark;
}
.InputField--warning .InputField__input {
  @apply .border-orange-dark;
}
</style>
