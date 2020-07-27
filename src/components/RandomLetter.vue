<template>
    <div v-text="letter"></div>
</template>

<script>
export default {
    props: { inMotion: { type: Boolean, default: true } },
    data() {
        return {
            dictionary: ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'A', 'B', 'C', 'D', 'E', 'F'],
            letter: null,
            handler: null,
        };
    },
    mounted() {
        if (!this.inMotion) {
            return;
        }

        this.handler = window.setInterval(() => {
            this.letter = this.randomLetter();

            this.$emit('letter-updated');
        }, 50);
    },
    destroyed() {
        window.clearInterval(this.handler);
    },
    methods: {
        randomLetter() {
            if (!this.inMotion) {
                window.clearInterval(this.handler);

                return this.letter;
            }

            return this.dictionary[Math.floor(Math.random() * this.dictionary.length)];
        },
    },
}
</script>
