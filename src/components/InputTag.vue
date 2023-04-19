<template>
    <div @mouseenter="setVisibleTagInput()" @mouseleave="setInvisibleTagInput()" class="wrapper">
        {{ label }}
        <div class="tag-wrapper">

            <div v-for="(item, index) in tagsArr" :key="index" class="tag-item">
                {{ item }} <button class="tag-delete" @click="deleteTag(item)">&#10006;</button>
            </div>

            <input ref="tagInputItem" :class="`${isTagVisible ? 'tag-input-visible' : ''}`" placeholder="Type here"
                class="tag-input" v-model="tagInputValue" @keydown.enter="addTag(tagInputValue)" />
        </div>
    </div>

    <!-- Small hint.. -->
    <span :class="`${isTagVisible ? 'tag-input-visible' : ''}`"
        style="display: block;margin-top: 10px;opacity: 0;transition: all .3s ease-in-out;">Press
        <b>Enter</b> after
        filling..</span>
</template>


<script setup lang="ts">
import { Ref, ref } from 'vue';

defineProps(['label'])

const tagsArr: Ref<string[]> = ref([])
const isTagVisible: Ref<boolean> = ref(false)
const tagInputValue: Ref<string> = ref('')
const tagInputItem = ref()

function addTag(value: string) {
    if (tagInputValue.value.trim() !== '') {
        tagsArr.value.push(value.trim())
        tagInputValue.value = ''
    }
}

function setVisibleTagInput() {
    isTagVisible.value = true
    tagInputItem.value.focus()
}

function setInvisibleTagInput() {
    if (tagInputValue.value.trim() === '') {
        isTagVisible.value = false
    }
}

function deleteTag(value: string) {
    tagsArr.value = tagsArr.value.filter((item: string) => item !== value)
}
</script>


<style>
.wrapper {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
}

.tag-wrapper {
    display: flex;
    align-self: center;
    align-items: center;
    flex-wrap: wrap;
    margin-top: 10px;
    padding: 23px 15px;
    padding-right: 100px;
    min-width: 300px;
    max-width: 400px;
    border: 1px solid #E3E5E5;
    border-radius: 12px;
    cursor: text;
}

.tag-input {
    opacity: 0;
    padding: 6px 0px;
    padding-left: 10px;
    background: none;
    border: 1px solid #E3E5E5;
    border-radius: 4px;
    transition: all 0.1s ease-in-out;
}

.tag-input-visible {
    opacity: 1 !important;
}

.tag-input:focus {
    outline: none;
}

.tag-item {
    display: flex;
    align-items: center;
    justify-content: center;
    margin-top: 3px;
    margin-bottom: 3px;
    padding: 3px 4px;
    margin-right: 4px;
    border-radius: 4px;
    background: #5042ac;
}

.tag-delete {
    cursor: pointer;
    background: none;
    padding: 0;
    border: none;
    margin-left: 4px;
}
</style>
