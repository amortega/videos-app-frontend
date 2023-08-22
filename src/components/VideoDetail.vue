<template>
    <div class="max-w-[263px] max-h-[150px] m-[25px]">
        <div class="relative">
            <div @click="$emit('openDeleteModal')" class="absolute w-[24px] h-[24px] m-[7px] text-[#ffffff] font-medium bg-black opacity-80 rounded-[3px] text-center right-px cursor-pointer">
                <svg class="w-3 h-3 inline pb-[3px]" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 14 14">
                    <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                    d="m1 1 6 6m0 0 6 6M7 7l6-6M7 7l-6 6" />
                </svg>
            </div>
            <img @click="$emit('openModalDetailVideo')" :src="props?.thumbnail_image" width="263" height="150">
            <div :class="{'w-[65px]': props.format_duration.length < 12, 'w-[100px]': props.format_duration.length >= 12}" class="absolute h-[22px] m-[6px] text-[#ffffff] font-bold bg-black opacity-80 rounded-[3px] text-center right-px bottom-px">{{ props?.format_duration }}</div>
        </div>
        <PlayModal 
            :key="props.id" 
            :modalActive="props.modal_active" 
            @closeModal="$emit('closeModalDetailVideo')"
            @stopVideo="$emit('stopVideoDetail')"
            @playVideo="$emit('playVideoDetail')"
            :title="props.title" 
            :description="props.description" 
            :external_id="props.external_id"
            :thumbnail_image="props.thumbnail_image"
            :format_duration="props.format_duration"
            :play_video="props.play_video"
        />
    </div>
</template>

<script setup>
import PlayModal from './PlayModal.vue';

defineEmits(["openModalDetailVideo", "closeModalDetailVideo", "stopVideoDetail", "playVideoDetail", "openDeleteModal"]);
const props = defineProps({
    id: {
      type: Number,
    },
    modal_active: {
      type: Boolean,
      default: false,
    },
    play_video: {
        type: Boolean,
        default: false,
    },
    thumbnail_image: {
        type: String,
        default: ''
    },
    format_duration: {
        type: String,
        default: ''
    },
    title: {
        type: String,
        default: ''
    },
    description: {
        type: String,
        default: ''
    },
    external_id: {
        type: String,
        default: ''
    }
});
</script>

<style scoped>
</style>