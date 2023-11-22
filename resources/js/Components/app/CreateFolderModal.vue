<template>
    <modal :show="modelValue"></modal>
</template>

<script setup>
import Modal from "@/Components/Modal.vue";

const { modelValue } = defineProps({
    modelValue: Boolean,
});
</script>
