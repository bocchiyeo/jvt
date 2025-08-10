<template>
    <div class="mt-10 w-75 mx-auto">
        <v-form ref="form" @submit.prevent=add>
            <v-container>
                <v-row justify="center" class="mb-2">
                    <h1>Add New Word</h1>
                </v-row>
                <v-row justify="center">
                    <v-col cols="12" lg="4" sm="12">
                        <v-text-field v-model="japanese" :rules="[emptyRule, japaneseRule]" label="Japanese" required>
                        </v-text-field>
                    </v-col>
                    <v-col cols="12" lg="4" sm="12">
                        <v-text-field v-model="furigana" :rules="[furiganaRule]" label="Furigana (Optional)">
                        </v-text-field>
                    </v-col>
                    <v-col cols="12" lg="4" sm="12">
                        <v-text-field v-model="english" :rules="[emptyRule, englishRule]" label="English" required>
                        </v-text-field>
                    </v-col>
                    <v-col cols="12" sm="12" class="d-flex justify-center">
                        <v-btn type="submit" size="large">add</v-btn>
                    </v-col>
                </v-row>
            </v-container>
        </v-form>
    </div>
</template>

<script>
import emitter from '@/eventBus'

export default {
    data() {
        const jpRegex = /^[\u3040-\u30FF\u3400-\u4DBF\u4E00-\u9FFF\uF900-\uFAFF]+$/
        const enRegex = /^[a-zA-Z_]+( [a-zA-Z_]+)*$/
        const fgRegex = /^[\u3040-\u309F\u30A0-\u30FF]+$/
        return {
            japanese: "",
            english: "",
            furigana: "",
            emptyRule: v => !!v || "Field cannot be empty",
            japaneseRule: v => jpRegex.test(v) || "Only Japanese characters allowed",
            englishRule: v => enRegex.test(v) || "Only English characters allowed",
            furiganaRule: v => !v || fgRegex.test(v) || "Furigana must be Hiragana or Katakana"
        }
    },
    methods: {
        async add() {
            const { valid } = await this.$refs.form.validate()
            if (valid) {
                const newWord = {
                    id: Date.now(),
                    japanese: this.japanese,
                    english: this.english,
                    furigana: this.furigana,
                    isActive: true
                }
                let vocabList 
                if (localStorage.getItem("vocabList")) {
                    vocabList = JSON.parse(localStorage.getItem("vocabList"))
                } else {
                    vocabList = []
                }
                vocabList.push(newWord)
                localStorage.setItem("vocabList", JSON.stringify(vocabList))
                emitter.emit("vocabAdded")
                this.$refs.form.reset()
            } else {
                
            }
        }
    }
}
</script>

<style>

</style>