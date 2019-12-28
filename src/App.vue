<template>
  <div id="app">
    <header>
      <nav class="nav">
        <h1>Tags/Chips Input with Vue</h1>
        <a href="https://docs-vue-chips.netlify.com" target="_blank"
          >See Docs</a
        >
      </nav>
    </header>
    <main>
      <p>Fully Customizable Tags/Chips input for Vue</p>
      <img alt="Vue logo" src="./assets/logo.png" />
      <h2>Simple Chips</h2>
      <ChipsInput v-model="fruits" placeholder="Enter a fruit"></ChipsInput>
      <h2>Non Removable Chips</h2>
      <ChipsInput
        v-model="heroes"
        class="custom-chips"
        placeholder="Enter a new hero"
        :removable="false"
      ></ChipsInput>
      <h2>Chips with Custom text rendering and Chip Classes</h2>
      <ChipsInput
        v-model="fruits"
        class="custom-chips"
        :chipClass="'fruit-chip'"
        :invalidClass="'err'"
        placeholder="Enter a fruit"
      >
        <template v-slot="slotProps">
          {{
            slotProps.index + " - " + (slotProps.chip.value || slotProps.chip)
          }}
        </template>
        <template v-slot:removeIcon>
          ❌
        </template>
      </ChipsInput>
      <h2>Disabled Chips</h2>
      <ChipsInput v-model="cars" disabled></ChipsInput>
    </main>
  </div>
</template>

<script>
import ChipsInput from "./components/ChipsInput.vue";

export default {
  name: "app",
  components: {
    ChipsInput
  },
  data() {
    return {
      fruits: [
        "apple",
        "banana",
        { value: "potato", invalid: true },
        "tomato",
        { value: "cabbage", invalid: true }
      ],
      heroes: [
        "Iron Man",
        { value: "Thor", invalid: false },
        "Captain America",
        { value: "Thanos", invalid: true },
        { value: "Ronan", invalid: true },
        "Black Panther"
      ],
      cars: [
        "BMW X6",
        "Audi R8",
        "Camaro",
        { value: "harley", invalid: true },
        "Aventador",
        { value: "vespa", invalid: true }
      ]
    };
  }
};
</script>

<style lang="scss">
html,
body {
  margin: 0;
  padding: 0;
}
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;

  header {
    border-bottom: 1px solid #eaeaea;
    position: fixed;
    top: 0;
    width: 100%;
    background-color: #ffffff;

    .nav {
      display: flex;
      padding: 0 20px;
      justify-content: space-between;
      align-items: center;

      h1 {
        font-size: 19px;
      }

      a {
        text-decoration: none;
        color: inherit;
        padding: 8px 20px;
      }
    }
  }

  main {
    max-width: 800px;
    margin: 80px auto;
  }

  .custom-chips {
    font-size: 16px;
  }

  .fruit-chip {
    background-color: goldenrod;
  }

  .err {
    background-color: red;
  }
}
</style>
