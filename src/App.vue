<template>
  <div class="container">
    <Header
      @toggle-add-text="toggleAddText"
      title="Text Analyzer"
      :showAddText="showAddText"
    />
    <div v-show="showAddText">
      <AddText @add-text="addText" />
    </div>
    <Texts @update-text="updateText" @delete-text="deleteText" :texts="texts" />
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import Texts from "./components/Texts.vue";
import AddText from "./components/AddText.vue";
export default {
  name: "App",
  components: { Header, Texts, AddText },
  data() {
    return {
      texts: [],
      showAddText: false,
    };
  },
  methods: {
    toggleAddText() {
      this.showAddText = !this.showAddText;
    },
    async addText(text) {
      const res = await fetch("backend/texts", {
        method: "POST",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(text),
      });
      const data = await res.json();
      this.texts.unshift({
        id: data.data.id,
        text: data.data.text,
        updated_at: data.data.updated_at,
      });
    },
    async deleteText(id) {
      const result = await fetch(`backend/texts/${id}`, {
        method: "DELETE",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ is_deleted: true }),
      });

      result.ok
        ? (this.texts = this.texts.filter((x) => x.id != id))
        : alert("Error");
    },
    updateText(data) {
      this.texts = this.texts.map((x) => {
        if (x.id == data.id) {
          x["text"] = data.text;
        }
        return x;
      });
    },
    async fetchTexts() {
      const res = await fetch("backend/texts");
      const data = await res.json();
      return data.data;
    },
  },
  async created() {
    this.texts = await this.fetchTexts();
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap");

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: "Poppins", sans-serif;
}

.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}

.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}

.btn:focus {
  outline: none;
}

.btn:active {
  transform: scale(0.98);
}

.btn-block {
  display: block;
  width: 100%;
}
</style>
