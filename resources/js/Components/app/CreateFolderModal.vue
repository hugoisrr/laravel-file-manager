<template>
    <modal :show="modelValue" @show="onShow" max-width="sm">
        <div class="p-6">
            <h2 class="text-lg font-medium text-gray-900">Create New Folder</h2>
            <div class="mt-6">
                <input-label
                    for="folderName"
                    value="Folder Name"
                    class="sr-only"
                />
                <text-input
                    type="text"
                    id="folderName"
                    ref="folderNameInput"
                    v-model="form.name"
                    class="block w-full mt-1"
                    :class="
                        form.errors.name
                            ? 'border-red-500 focus:border-red-500 focus:ring-red-500'
                            : ''
                    "
                    placeholder="Folder Name"
                    @keyup.enter="createFolder"
                />
                <input-error :message="form.errors.name" class="mt-2" />
            </div>
            <div class="flex justify-end mt-6">
                <secondary-button @click="closeModal">Cancel</secondary-button>
                <primary-button
                    class="ml-3"
                    @click="createFolder"
                    :class="{ 'opacity-25': form.processing }"
                    :disable="form.processing"
                >
                    Submit
                </primary-button>
            </div>
        </div>
    </modal>
</template>

<script setup>
import Modal from "@/Components/Modal.vue";
import InputLabel from "../InputLabel.vue";
import TextInput from "../TextInput.vue";
import InputError from "../InputError.vue";
import { useForm } from "@inertiajs/vue3";
import SecondaryButton from "../SecondaryButton.vue";
import PrimaryButton from "../PrimaryButton.vue";
import { ref, nextTick } from "vue";

const form = useForm({
    name: "",
});

const folderNameInput = ref(null);

const { modelValue } = defineProps({
    modelValue: Boolean,
});

function onShow() {
    nextTick(() => folderNameInput.value.focus());
}

function createFolder() {
    form.post(route("folder.create"), {
        preserveScroll: true,
        onSuccess: () => {
            closeModal();
            form.reset();
            // Show success notification
        },
        onError: () => folderNameInput.value.focus(),
    });
}
const emit = defineEmits(["update:modelValue"]);

function closeModal() {
    emit("update:modelValue");
    form.clearErrors();
    form.reset();
}
</script>
