<template>
    <div class="max-w-[263px] max-h-[150px] m-[25px] float-left">
        <div class="relative">
            <div class="absolute w-[24px] h-[24px] m-[7px] text-[#ffffff] font-medium bg-black opacity-80 rounded-[3px] text-center right-px cursor-pointer">x</div>
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

defineEmits(["openModalDetailVideo", "closeModalDetailVideo", "stopVideoDetail", "playVideoDetail"]);
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