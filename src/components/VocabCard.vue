<template>
    <div class="mt-10 w-75 mx-auto">
        <v-row>
            <v-col cols="12" xl="3" lg="4" sm="6" v-if="vocabList" v-for="(vocab, index) in vocabList" :key="index">
                <v-card variant="tonal" class="text-center">
                    <v-card-title class="text-wrap">{{ vocab.japanese }}</v-card-title>
                    <v-card-subtitle class="text-wrap">{{ vocab.furigana }}</v-card-subtitle>
                    <v-card-text class="text-body-1 text-wrap">{{ vocab.english }}</v-card-text>
                </v-card>
            </v-col>
            <v-col cols="12" class="text-center" v-else>
                <h2>No words yet. Add below!</h2>
            </v-col>
        </v-row>
    </div>
</template>

<script>
import emitter from '@/eventBus'

export default {
    data() {
        return {
            vocabList: JSON.parse(localStorage.getItem("vocabList"))
        }
    },
    mounted() {
        emitter.on("vocabAdded", this.syncVocab)
    },
    beforeUnmount() {
        emitter.off("vocabAdded", this.syncVocab)
    },
    methods: {
        syncVocab() {
            this.vocabList = JSON.parse(localStorage.getItem("vocabList"))
        }
    }
}
</script>

<style>

</style>