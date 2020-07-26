<template>
    <div id="app">
        <div>Password Decryptor</div>

        <div>Calculating Hashes</div>

        <div>
            Password :

            <ProgressivePasswordReveal password="coucou!" @revealed="toggleActive"/>
        </div>

        <div>
            [{{hours}}:{{minutes}}:{{seconds}}] :{{numberOfKeys}} keys tested
        </div>

        <div>
            <div style="width: 300px;">Master Key :</div>
            <div>
                <RandomLetter @letter-updated="increment" :active="isActive"/>
                <RandomLetter @letter-updated="increment" :active="isActive"/>
            </div>
        </div>

        <div>
            <div style="width: 300px;">Transient Key :</div>
            <div>
                <RandomLetter @letter-updated="increment" :active="isActive"/>
                <RandomLetter @letter-updated="increment" :active="isActive"/>
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
            seconds: '00',
            minutes: '00',
            hours: '00',
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
            this.seconds = Math.floor(diff % 60) < 10 ? `0${Math.floor(diff % 60)}` : Math.floor(diff % 60);
            diff = diff / 60;
            this.minutes = Math.floor(diff % 60) < 10 ? `0${Math.floor(diff % 60)}` : Math.floor(diff % 60);
            diff = diff / 60;
            this.hours = Math.floor(diff % 24) < 10 ? `0${Math.floor(diff % 24)}` : Math.floor(diff % 24);
        }, 1000);
    },
    destroyed() {
        window.clearInterval(this.handler);
    },
    methods: {
        toggleActive() {
            this.isActive = false;

            this.$forceUpdate();
        },
        increment() {
            this.numberOfKeys++;
        },
    },
}
</script>

<style lang="scss">
    #app {
        font-family: Avenir, Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        margin-top: 60px;
        color: white;
        font-family: "Fira Code";
    }
</style>
