<template>
    <div id="app" class="p-10">
        <div class="text-center text-4xl font-sans font-bold">Password Decryptor</div>

        <div class="font-sans mb-10 text-center tracking-wider">Calculating Hashes</div>
        <div style="max-width: 600px" class="mx-auto space-y-4">

            <div>
                [{{elapsedTime}}] : {{numberOfKeys}} keys tested
            </div>

            <div>
                <div class="inline-block mr-4">
                    Current password :
                </div>
                <div class="inline-block">
                    <ProgressivePasswordReveal password="yo" @revealed="toggleActive"/>
                </div>
            </div>

            <div>
                <div class="inline-flex justify-between mr-4 align-top" style="width: 140px;">
                    <div>Master Key</div>
                    <div>:</div>
                </div>
                <div class="inline-block align-top tracking-tighter">
                    <div v-for="_index in 30" :key="`master-key-${_index}`"
                         class="inline-block"
                         :class="{'mr-2': 0 === _index%2}">
                        <RandomLetter @letter-updated="increment" :active="isActive"/>
                        <RandomLetter @letter-updated="increment" :active="isActive"/>
                    </div>
                </div>
            </div>

            <div>
                <div class="inline-flex justify-between mr-4 align-top" style="width: 140px;">
                    <div>Transient Key</div>
                    <div>:</div>
                </div>
                <div class="inline-block align-top tracking-tighter">
                    <div v-for="_index in 30" :key="`transient-key-${_index}`"
                         class="inline-block"
                         :class="{'mr-2': 0 === _index%2}">
                        <RandomLetter @letter-updated="increment" :active="isActive"/>
                        <RandomLetter @letter-updated="increment" :active="isActive"/>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import RandomLetter from './components/RandomLetter';
import ProgressivePasswordReveal from './components/ProgressivePasswordReveal';

export default {
    components: { RandomLetter, ProgressivePasswordReveal },
    data() {
        return {
            isActive: true,
            startedAt: null,
            handler: null,
            elapsedTime: '00:00:00',
            numberOfKeys: 0,
        };
    },
    created() {
        this.startedAt = (new Date()).getTime();
    },
    mounted() {
        this.handler = window.setInterval(() => {
            const now = (new Date()).getTime();
            let diff = (now - this.startedAt) / 1000;
            const seconds = Math.floor(diff % 60) < 10 ? `0${Math.floor(diff % 60)}` : Math.floor(diff % 60);
            diff = diff / 60;
            const minutes = Math.floor(diff % 60) < 10 ? `0${Math.floor(diff % 60)}` : Math.floor(diff % 60);
            diff = diff / 60;
            const hours = Math.floor(diff % 24) < 10 ? `0${Math.floor(diff % 24)}` : Math.floor(diff % 24);
            this.elapsedTime = `${hours}:${minutes}:${seconds}`;
        }, 1000);
    },
    destroyed() {
        window.clearInterval(this.handler);
    },
    methods: {
        toggleActive() {
            this.isActive = false;

            window.clearInterval(this.handler);
            this.$forceUpdate();
        },
        increment() {
            this.numberOfKeys++;
        },
    },
}
</script>

<style lang="scss">
    /*#app {*/
    /*    font-family: Avenir, Helvetica, Arial, sans-serif;*/
    /*    -webkit-font-smoothing: antialiased;*/
    /*    -moz-osx-font-smoothing: grayscale;*/
    /*    text-align: center;*/
    /*    margin-top: 60px;*/
    /*    color: white;*/
    /*    font-family: "Fira Code";*/
    /*}*/
</style>
