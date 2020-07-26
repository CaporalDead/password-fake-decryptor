<template>
    <div>
        <div v-for="entry in Object.values(letters)" :key="`${entry.key}`" style="display: inline-block;">
            <RandomLetter style="width: 30px; color: red" v-if="!entry.revealed"/>
            <div style="width: 30px; color: greenyellow" v-else v-text="entry.letter"></div>
        </div>
    </div>
</template>

<script>
import RandomLetter from "./RandomLetter";

export default {
    components: { RandomLetter },
    props: {
        password: { type: String, required: true },
        from: { type: Number, default: 500 },
        to: { type: Number, default: 2500 },
    },
    created() {
        this.password.split('').forEach((l, idx) => {
            this.letters[idx] = { key: `${l}-${idx}`, letter: l, revealed: false };
        });
    },
    mounted() {
        this.process();
    },
    data() {
        return {
            handler: null,
            letters: {},
            order: [],
        };
    },
    methods: {
        delay() {
            return Math.floor(Math.random() * this.to) + this.from;
        },
        process() {
            window.setTimeout(() => {
                const result = this.reveal()

                this.$forceUpdate();

                if (false !== result) {
                    this.process();
                } else {

                    this.$emit('revealed');
                }
            }, this.delay());
        },
        reveal() {
            if (!Object.values(this.letters).find(r => !r.revealed)) {
                return false;
            }

            let itemToReveal = null;

            while (null === itemToReveal || true === this.letters[itemToReveal].revealed) {
                itemToReveal = Math.floor(Math.random() * Object.entries(this.letters).length);
            }

            this.letters[itemToReveal].revealed = true;

            return itemToReveal;
        },
    },
}
</script>
