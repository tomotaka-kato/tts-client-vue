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

    <v-flex xs10>
        <v-layout row wrap >
            <v-flex xs7>
                AudioContextでTTS呼び出し（buffer）
            </v-flex>

            <v-flex xs3>
                <v-btn fab small color=primary @click="playSound">
                    <v-icon dark>volume_up</v-icon>
                </v-btn>
            </v-flex>
        </v-layout>
    </v-flex>

    </v-layout>

    <div v-if="logMessage.length > 0">
        {{ logMessage }}
    </div>

  </v-container>
</template>

<script lang="ts">
import { Vue, Component } from 'vue-property-decorator';

@Component
export default class Content extends Vue {
    private text: string = 'サンプルテキストです。';
    private logMessage: string = '';
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
        } catch (e) {
            this.logMessage = e;
        }
    }

    private openTTSApiPage() {
        window.location.href = this.getApiUrl();
    }

    private getApiUrl(): string {
        return `${this.ttsApiUrl}${this.text}`
    }

    private audioContext = new ((<any>window).AudioContext || (<any>window).webkitAudioContext)();
    private playSound() {
        const AudioContext = (<any>window).AudioContext || (<any>window).webkitAudioContext;
        const ctx = new AudioContext();
        this.logMessage = 'start request'
        // iOSで音声再生するための手段
        // 参考: https://qiita.com/zprodev/items/7fcd8335d7e8e613a01f
        ctx.createBufferSource().start();

        const request = new XMLHttpRequest();
        const url = this.getApiUrl();
        request.open('GET', url, true);
        request.responseType = 'arraybuffer';
        request.onload =  () => {
            this.logMessage = 'request loaded'
            ctx.decodeAudioData(request.response, (audioBuffer: any) => {
                const audioSource = ctx.createBufferSource();
                audioSource.buffer = audioBuffer;
                audioSource.connect(ctx.destination);
                audioSource.start();
            });
        }
        request.send();
    }
}
</script>

<style>
</style>


