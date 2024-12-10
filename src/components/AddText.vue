<template>
  <form @submit="onSubmit" class="add-form">
    <div class="form-control">
      <textarea
        type="text"
        v-model="text"
        name="text"
        placeholder="Add Text"
        rows="4"
        @keydown.enter="onEnter"
        class="text-input"
      />
    </div>

    <input type="submit" value="Save Text" class="btn btn-block" />
  </form>
</template>

<script>
export default {
  name: "AddText",
  data() {
    return {
      text: "",
    };
  },
  methods: {
    onSubmit(e) {
      e.preventDefault();
      if (!this.text) {
        alert("Please add a text");
      }
      const newText = {
        text: this.text,
      };
      this.$emit("add-text", newText);
      this.text = "";
    },
    onEnter(e) {
      if (e.key === "Enter") {
        e.preventDefault();
        this.text += "\n";
      }
    },
  },
};
</script>

<style scoped>
.add-form {
  margin-bottom: 40px;
}

.form-control {
  margin: 20px 0;
}

.form-control label {
  display: block;
}

.form-control input {
  width: 100%;
  height: 40px;
  margin: 5px;
  padding: 3px 7px;
  font-size: 17px;
}

.form-control-check {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.form-control-check label {
  flex: 1;
}

.form-control-check input {
  flex: 2;
  height: 20px;
}

.text-input {
  width: 100%;
  padding: 12px;
  font-size: 16px;
  line-height: 1.5;
  border: 1px solid #ddd;
  border-radius: 4px;
  box-sizing: border-box;
  resize: vertical; /* Allow resizing vertically */
  min-height: 150px; /* Set a minimum height */
  background-color: #fff;
  transition: border-color 0.3s ease;
}

.text-input:focus {
  outline: none;
  border-color: #007bff; /* Add blue border on focus */
}
</style>
