<template>
    <div style="color: white;" v-text="letter"></div>
</template>

<script>
export default {
    props: { active: { type: Boolean, default: true } },
    data() {
        return {
            dictionary: ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'A', 'B', 'C', 'D', 'E', 'F'],
            letter: null,
            handler: null,
        };
    },
    mounted() {
        this.randomLetter();

        this.handler = window.setInterval(() => {
            this.letter = this.randomLetter();
        }, 50);

        if (!this.active) {
            window.clearInterval(this.handler);
        }
    },
    methods: {
        randomLetter() {
            return this.dictionary[Math.floor(Math.random() * this.dictionary.length)];
        },
    },
}
</script>
