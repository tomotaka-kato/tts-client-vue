<template>
  <v-container>
    <v-layout
      text-center
        justify-space-around
        row
      wrap
    >
    <v-flex xs10>
        <v-text-field 
            label="発話メッセージ"
            v-model="text"
        ></v-text-field>
    </v-flex>

    <v-flex xs5>
        speechSynthesis
    </v-flex>

    <v-flex xs5>
        <v-btn fab small color=primary @click="blowserSpeak">
            <v-icon dark>volume_up</v-icon>
        </v-btn>
    </v-flex>


    </v-layout>
  </v-container>
</template>

<script lang="ts">
import { Vue, Component } from 'vue-property-decorator';

@Component
export default class Content extends Vue {
    private text: string = 'サンプルテキストです。';

    private blowserSpeak() {
        try {
            const msg = new SpeechSynthesisUtterance();

            const voices = speechSynthesis.getVoices();
            const voice = voices.find(_ => _.lang === 'ja-JP');
            if (voice) {
                msg.voice = voice;
            }
            msg.volume = 1.0;
            msg.rate = 1.0;
            msg.pitch = 1.0;
            msg.lang = 'ja-JP';
            msg.text = this.text;
            speechSynthesis.speak(msg);
        } catch (_) {
            // AndroidのWebVew対応のためエラーは握りつぶす。
        }
    }
}
</script>

<style>
</style>


