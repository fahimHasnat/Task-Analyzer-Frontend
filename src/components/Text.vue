<template>
  <div class="text">
    <h3>
      <span v-if="!isEditing">
        {{ text.text }}
      </span>
      <span v-else>
        <input
          type="text"
          v-model="editText"
          class="edit-input"
          @keyup.enter="saveEdit"
          @keyup.esc="cancelEdit"
        />
      </span>
      <span class="actions">
        <i @click="onEditClick" class="fas fa-edit"></i>
        <i @click="$emit('delete-text', text.id)" class="fas fa-times"></i>
      </span>
    </h3>
    <p>{{ `Last Updated: ${text.updated_at}` }}</p>

    <div v-if="isEditing" class="modal">
      <div class="modal-content">
        <h4>Edit Text</h4>
        <textarea v-model="editText" class="modal-textarea"></textarea>
        <div class="modal-actions">
          <button @click="saveEdit" class="save-btn">Save</button>
          <button @click="cancelEdit" class="cancel-btn">Cancel</button>
        </div>
      </div>
    </div>

    <div class="analysis">
      <button @click="getAnalysis('word-count', text.id)">Word Count</button>
      <button @click="getAnalysis('character-count', text.id)">
        Character Count
      </button>
      <button @click="getAnalysis('sentence-count', text.id)">
        Sentence Count
      </button>
      <button @click="getAnalysis('paragraph-count', text.id)">
        Paragraph Count
      </button>
      <button @click="getAnalysis('longest-words', text.id)">
        Longest Words
      </button>
    </div>

    <div v-if="expandedSection" class="result">
      <p>{{ expandedSection }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "Text",
  props: { text: Object },
  data() {
    return {
      isEditing: false,
      editText: "",
      expandedSection: "",
    };
  },
  methods: {
    async getAnalysis(type, id) {
      const result = await fetch(`http://localhost:3001/texts/${id}/${type}`);
      const data = await result.json();

      let typeName = type
        .split("-")
        .map((word) => word.charAt(0).toUpperCase() + word.slice(1))
        .join(" ");

      if (typeof data.data == "object") {
        data.data = data.data.longestWords.join(", ");
      }

      this.expandedSection = `${typeName}: ${data.data}`;
    },
    onEditClick() {
      this.isEditing = true;
      this.editText = this.text.text;
    },
    async saveEdit() {
      const result = await fetch(`backend/texts/${this.text.id}`, {
        method: "PUT",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ text: this.editText }),
      });

      if (result.ok) {
        this.$emit("update-text", { id: this.text.id, text: this.editText });
        this.isEditing = false;
      }
    },
    cancelEdit() {
      this.isEditing = false;
    },
  },
};
</script>

<style scoped>
.fas {
  color: red;
  cursor: pointer;
  margin-left: 10px; /* Space between icons */
}

.text {
  background: #f4f4f4;
  margin: 5px;
  padding: 10px 20px;
  cursor: pointer;
}

.text h3 {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.analysis {
  margin-top: 10px; /* Add some spacing above the buttons */
}

.analysis button {
  margin: 5px; /* Add spacing between buttons */
  padding: 10px 15px; /* Make buttons larger for better usability */
  font-size: 14px; /* Adjust font size */
  background-color: #007bff; /* Use a distinct color */
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.analysis button:hover {
  background-color: #0056b3; /* Slightly darker color on hover */
}

.analysis button:focus {
  outline: none;
  box-shadow: 0 0 3px #0056b3;
}

.text h3 i {
  margin-left: 10px; /* Consistent spacing between icons */
}

.text .actions {
  display: flex; /* Align the delete and edit icons horizontally */
  gap: 10px; /* Add space between action buttons */
}
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background: white;
  padding: 20px;
  border-radius: 10px;
  width: 400px;
  text-align: center;
}

.modal-textarea {
  width: 100%;
  height: 100px;
  margin: 10px 0;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}

.modal-actions {
  display: flex;
  justify-content: space-around;
}

.save-btn,
.cancel-btn {
  padding: 5px 15px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.save-btn {
  background-color: #007bff;
  color: white;
}

.cancel-btn {
  background-color: #f4f4f4;
  color: black;
}
</style>
