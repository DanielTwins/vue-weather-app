<template>
  <!-- use Teleport component to create a portal to render the Teleports content
  at a different location in the DOM tree. Typically outside of its current parent component
  -->
  <Teleport to="body">
    <Transition name="modal-outer">
      <div
        v-show="modalActive"
        class="absolute w-full bg-black bg-opacity-30 h-screen top-0 left-0 flex justify-center px-8"
      >
        <Transition name="modal-inner">
          <div
            v-if="modalActive"
            class="p-4 bg-white self-start mt-32 max-w-screen-md"
          >
            <slot />
            <button
              @click="$emit('close-modal')"
              class="text-white mt-8 bg-weather-primary py-2 px-6"
            >
              Close
            </button>
          </div>
        </Transition>
      </div>
    </Transition>
  </Teleport>
</template>

<script setup>
defineEmits(["close-modal"]);
defineProps({
  modalActive: {
    type: Boolean,
    default: false,
  },
});
</script>

<style scoped>
/* these classes control the duration of the entering and the leaving transition */
.modal-outer-enter-active,
.modal-inner-leave-active {
  transition: opacity 0.3s cubic-bezier(0.52, 0.02, 0.19, 1.02);
}
/* for the transition fade in and fade out */
.modal-outer-enter-from,
.modal-inner-leave-to {
  opacity: 0;
}

.modal-outer-enter-active {
  transition: all 0.3s cubic-bezier(0.52, 0.02, 0.19, 1.02) 0.15s;
}
.modal-inner-leave-active {
  transition: all 0.3s cubic-bezier(0.52, 0.02, 0.19, 1.02);
}

.modal-outer-enter-from {
  opacity: 0;
  transform: scale(0.8);
}
.modal-inner-leave-to {
  transform: scale(0.8);
}
</style>

<!-- 
  The purpose of using <Teleport> is to enable the component to be rendered at a 
    different location in the DOM hierarchy while maintaining its logical position 
    in the component tree. This is useful in situations where you need to render a 
    component's content in a specific part of the document structure, such as 
    directly under the <body> element, to avoid styling and z-index issues.
 -->
