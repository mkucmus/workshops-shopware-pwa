<template>
    <div>
        <CmsElementText v-for="text in textSlots" :key="text.id" :content="text" />
        <div v-for="column in columns" :key="column.text">
            <div v-html="column.text"/>
            <SwImage :src="column.image" />
        </div>
    </div>
</template>
<script>
import { computed } from "@vue/composition-api"
import SwImage from "@/components/atoms/SwImage.vue";
import CmsElementText from "@/cms/elements/CmsElementText.vue";

export default {
    components: {
        SwImage,
        CmsElementText
    },
    props: {
        content: {
            type: Object,
            require: true
        }
    },

    setup({ content }) {

        const slots = computed(() => {
            return [
                {
                    text: content.slots.find(({slot})=> slot === "left-text")?.data.content,
                    image: content.slots.find(({slot})=> slot === "left-image")?.data?.media?.url
                },
                {
                    text: content.slots.find(({slot})=> slot === "center-text")?.data.content,
                    image: content.slots.find(({slot})=> slot === "center-image")?.data?.media?.url
                },
                {
                    text: content.slots.find(({slot})=> slot === "right-text")?.data.content,
                    image: content.slots.find(({slot})=> slot === "right-image")?.data?.media?.url
                }
            ]
        })

        return {
            columns: slots,
            textSlots: content.slots.filter(({type}) => type === "text")
        }
    }
}
</script>