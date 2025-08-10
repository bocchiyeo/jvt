<template>
    <div class="mt-10 w-75 mx-auto">
        <v-row>
            <v-col cols="12" xl="3" lg="4" sm="6" v-if="vocabList.filter(v => v.isActive).length > 0" v-for="vocab in vocabList.filter(v => v.isActive)" :key="vocab.id">
                <v-card variant="tonal" class="text-center">
                    <v-card-title class="text-wrap">{{ vocab.japanese }}</v-card-title>
                    <v-card-subtitle class="text-wrap">{{ vocab.furigana }}</v-card-subtitle>
                    <v-card-text class="text-body-1 text-wrap">{{ vocab.english }}</v-card-text>
                    <v-card-actions class="pt-0">
                        <v-btn @click="deleteVocab(vocab.id)" class="ms-auto"><v-icon>mdi-delete</v-icon>Delete</v-btn>
                    </v-card-actions>
                </v-card>
            </v-col>
            <v-col cols="12" class="text-center" v-else>
                <h2>No words yet. Add below!</h2>
            </v-col>
        </v-row>
    </div>
</template>

<script>
import { mdiDelete } from '@mdi/js'
import emitter from '@/eventBus'

export default {
    data() {
        return {
            vocabList: JSON.parse(localStorage.getItem("vocabList")),
            mdiDelete
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
        },
        deleteVocab(id) {
            for (let i = 0; i < this.vocabList.length; i++) {
                if (this.vocabList[i].id == id) {
                    this.vocabList[i].isActive = false
                    break
                }
            }
            localStorage.setItem("vocabList", JSON.stringify(this.vocabList))
        }
    }
}
</script>

<style>

</style>