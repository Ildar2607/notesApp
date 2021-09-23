<template>
  <div class="notes">
    <div
      class="note"
      :class="[{ full: !grid }, note.priority]"
      v-for="(note, index) in notes"
      :key="index"
    >
      <div class="note-header" :class="{ full: !grid }">
        <div class="note__editor" :class="{ active: editor }">
          <input 
            class="note__title-editor"
            type="text"
            v-on:keyup.enter="applyTitle(index)"
            v-on:keyup.escape="resetTitle(index)"
          />
        </div>
        <span class="note-header__description">Редактировать заголовок</span>
        <p class="note__title" @click="openEditor(index)">{{ note.title }}</p>
        <p style="cursor: pointer" @click="removeNote(index)">x</p>
      </div>
      <div class="note-body">
        <p>{{ note.descr }}</p>
        <span>{{ note.date }}</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {};
  },
  props: {
    notes: {
      type: Array,
      required: true,
    },
    grid: {
      type: Boolean,
      required: true,
    },
    editor: {
      type: Boolean,
      required: true,
    },
  },
  methods: {
    removeNote(index) {
      this.$emit("remove", index);
    },
    openEditor(index) {
      this.$emit("openEditor", index);
    },
    resetTitle(index) {
      this.$emit("resetTitle", index);
    },
    applyTitle(index) {
      this.$emit("applyTitle", index);
    }
  },
};
</script>

<style lang="scss">
.notes {
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-wrap: wrap;
  padding: 40px 0;
}

.note {
  width: 46%;
  padding: 18px 20px;
  margin-bottom: 20px;
  background: #ffffff;
  transition: all 0.25s cubic-bezier(0.02, 0.01, 0.47, 1);
  box-shadow: 0 30px 30px rgba(0, 0, 0, 0.01);
  position: relative;

  &.full {
    width: 100%;
    text-align: center;
  }
  &:hover {
    box-shadow: 0 30px 30px rgba(0, 0, 0, 0.04);
    transform: translate(0, -6px);
    transition-delay: 0s !important;
  }
  &.medium {
    background: lightyellow;
  }
  &.high {
    background: lightsalmon;
    color: #ffffff;
  }
  &__editor {
    position: absolute;
    top: 0;
    left: 0;
    pointer-events: none;
    opacity: 0;
    transition: opacity 0.25s ease-in-out;
    &.active {
      pointer-events: unset;
      opacity: 1;
    }
  }
  &__title {
    cursor: pointer;
    min-width: 100%;
    height: 30px;
  }
}

.note-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  &:hover {
    & .note-header__description {
      opacity: 1;
      transform: translateY(-100%);
    }
  }
  h1 {
    font-size: 32px;
  }
  p {
    color: #402caf;
  }
  svg {
    margin-right: 12px;
    color: #999999;
    &.active {
      color: #402caf;
    }
    &:last-child {
      margin-right: 0;
    }
  }
  &.full {
    justify-content: center;
  }
  &__description {
    font-size: 15px;
    position: absolute;
    top: 0;
    left: 20px;
    color: orchid;
    opacity: 0;
    transition: opacity 0.25s ease-in-out, transform 0.25s ease-in-out
  }
}

.note-body {
  p {
    margin: 20px 0;
  }
  span {
    font-size: 14px;
    color: #999999;
  }
}
</style>
