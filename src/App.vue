<template>
  <div id="app">
    <div>Password Decryptor</div>

    <div>Calculating Hashes</div>

    <div>
      Password :

      <ProgressivePasswordReveal password="coucou!" @revealed="toggleActive"/>
    </div>

    <div>
      <div style="width: 300px;">Master Key : </div>
      <div>
        <RandomLetter :active="isActive"/>
        <RandomLetter :active="isActive"/>
      </div>
    </div>

    <div>
      <div style="width: 300px;">Transient Key : </div>
      <div>
        <RandomLetter :active="isActive"/>
        <RandomLetter :active="isActive"/>
      </div>
    </div>
  </div>
</template>

<script>
import RandomLetter from './components/RandomLetter';
import ProgressivePasswordReveal from './components/ProgressivePasswordReveal';

export default {
  components: { RandomLetter, ProgressivePasswordReveal},
  data() {
    return {
      isActive: true,
    };
  },
  methods: {
    toggleActive() {
      this.isActive = false;

      this.$forceUpdate();
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
