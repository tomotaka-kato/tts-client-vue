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

    <v-flex xs10>
        <v-layout row wrap >
            <v-flex xs7>
                speechSynthesis
            </v-flex>

            <v-flex xs3>
                <v-btn fab small color=primary @click="blowserSpeak">
                    <v-icon dark>volume_up</v-icon>
                </v-btn>
            </v-flex>
        </v-layout>
    </v-flex>
            


    <v-flex xs10>
        <v-layout row wrap >
            <v-flex xs7>
                TTSのAPIを直接表示
            </v-flex>

            <v-flex xs3>
                <v-btn fab small color=primary :href="getApiUrl()">
                    <v-icon dark>volume_up</v-icon>
                </v-btn>
            </v-flex>
        </v-layout>
    </v-flex>



    </v-layout>
  </v-container>
</template>

<script lang="ts">
import { Vue, Component } from 'vue-property-decorator';

@Component
export default class Content extends Vue {
    private text: string = 'サンプルテキストです。';
    private ttsApiUrl: string = 'https://arcane-temple-52272.herokuapp.com/?text=';

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

    private openTTSApiPage() {
        window.location.href = this.getApiUrl();
    }

    private getApiUrl(): string {
        return `${this.ttsApiUrl}${this.text}`
    }
}
</script>

<style>
</style>


