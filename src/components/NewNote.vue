<template>
  <div class="new-note">
    <label>Title</label>
    <input v-model="note.title" type="text" />
    <div class="new-note__priority priority">
      <p class="priority__title">Select Priority</p>
      <div class="priority__radios">
        <label  
          @click="getPriority(index)"
          class="priority__radio radio"
          v-for="(priority, index) in priorities"
          :key="index"
        >
          <input name="priority" class="radio__input" type="radio"/>
          <div class="radio__decor" ></div>
          <span class="radio__label">{{ priority }}</span>
        </label>
      </div>
    </div>
    <label>Description</label>
    <textarea v-model="note.descr"></textarea>
    <button class="btn btnPrimary" @click="addNote">New Note</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      priorityIndex: 0,
      test: 0,
    }
  },
  props: {
    // test: {
    //   type: String,
    // },
    // priorityIndex: "",
    note: {
      type: Object,
      required: true,
    },
    priorities: {
      type: Array,
      required: true,
    },
  },
  methods: {
    addNote() {
      this.$emit("addNote", this.note);
    },
    getPriority(index) {
      this.$emit("getPriority", index);
    },
  },
};
</script>

<style lang="scss">
.new-note {
  text-align: center;
  .btn {
    margin-bottom: 50px;
  }
}

.priority {
  margin-bottom: 30px;
  &__title {
    margin-bottom: 30px;
  }
  &__radios {
    display: flex;
  }
  &__radio {
    margin: 0 50px;
  }
}

.radio {
  cursor: pointer;
  display: flex;
  align-items: center;
  min-width: 30px;
  height: 30px;
  &__input {
    display: none;
    &:checked {
      & ~ .radio__decor::after {
        opacity: 1;
      }
    }
  }
  &__decor {
    width: 30px;
    height: 30px;
    border: 2px solid lightgray;
    border-radius: 5px;
    position: relative;
    display: flex;
    align-items: center;
    justify-content: center;
    margin-right: 15px;
    &::after {
      position: relative;
      display: block;
      content: "";
      z-index: 100;
      width: 60%;
      height: 60%;
      background: #494ce8;
      border-radius: 5px;
      transition: opacity 0.25s ease-in-out;
      opacity: 0;
    }
  }
}
</style>
