<template>
  <div class="wrapper">
    <div class="wrapper-contnet">
      <section>
        <div class="container">
          <message v-if="message" :message="message" />
          <newNote
            :note="note"
            :priorities="priorities"
            @addNote="addNote"
            @getPriority="getPriority"
          />
          <!--@getPriority="getPriority" :test="test"-->
          <div class="note-header" style="margin: 36px 0;">
            <h1>{{ title }}</h1>
            <search
              @search="search = $event"
              :value="search"
              placeholder="Find you note"
            />
            <div class="icons">
              <svg
                :class="{ active: grid }"
                @click="grid = true"
                style="cursor: pointer"
                xmlns="http://www.w3.org/2000/svg"
                width="24"
                height="24"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
                stroke-linecap="round"
                stroke-linejoin="round"
              >
                <rect x="3" y="3" width="7" height="7"></rect>
                <rect x="14" y="3" width="7" height="7"></rect>
                <rect x="14" y="14" width="7" height="7"></rect>
                <rect x="3" y="14" width="7" height="7"></rect>
              </svg>
              <svg
                :class="{ active: !grid }"
                @click="grid = false"
                style="cursor: pointer"
                xmlns="http://www.w3.org/2000/svg"
                width="24"
                height="24"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
                stroke-linecap="round"
                stroke-linejoin="round"
              >
                <line x1="8" y1="6" x2="21" y2="6"></line>
                <line x1="8" y1="12" x2="21" y2="12"></line>
                <line x1="8" y1="18" x2="21" y2="18"></line>
                <line x1="3" y1="6" x2="3" y2="6"></line>
                <line x1="3" y1="12" x2="3" y2="12"></line>
                <line x1="3" y1="18" x2="3" y2="18"></line>
              </svg>
            </div>
          </div>
          <notes
            :notes="notesFilter"
            :grid="grid"
            :editor="editor"
            @remove="removeNote"
            @openEditor="openEditor"
          />
        </div>
      </section>
    </div>
  </div>
</template>

<script>
import message from "@/components/Message.vue";
import newNote from "@/components/NewNote.vue";
import notes from "@/components/Notes.vue";
import search from "@/components/Search.vue";
export default {
  components: {
    message,
    newNote,
    notes,
    search,
  },
  data() {
    return {
      title: "Notes App",
      search: "",
      message: null,
      grid: true,
      editor: false,
      note: {
        editor: false,
        title: "",
        descr: "",
        priority: "",
      },
      priority: "norm",
      notes: [
        {
          title: "First Note",
          descr: "Description for first note",
          date: new Date(Date.now()).toLocaleString(),
          priority: "norm",
        },
        {
          title: "Second Note",
          descr: "Description for second note",
          date: new Date(Date.now()).toLocaleString(),
          priority: "medium",
        },
        {
          title: "Third Note",
          descr: "Description for third note",
          date: new Date(Date.now()).toLocaleString(),
          priority: "high",
        },
      ],
      priorities: ["norm", "medium", "high"],
    };
  },
  computed: {
    notesFilter() {
      let array = this.notes,
        search = this.search;

      if (!search) return array;

      //small

      search = search.trim().toLowerCase();

      //filter

      array = array.filter(function(item) {
        if (item.title.toLowerCase().indexOf(search) !== -1) {
          return item;
        }
      });

      return array;
    },
  },
  methods: {
    openEditor(index) {
      let editors = document.querySelectorAll(".note__editor");
      let titleEditors = document.querySelectorAll(".note__title-editor");
      let title = document.querySelectorAll(".note__title")
      editors.forEach((elem) => {
        elem.classList.remove("active");
      });
      editors[index].classList.add("active");

      document.addEventListener("keydown", function(event) {
        if (event.code === "Enter") {
          title[index].textContent = titleEditors[index].value 
          editors[index].classList.remove('active')
        }
        if (event.code === "Escape" ) {
          editors[index].classList.remove('active')
        }
      });
    },

    // resetTitle(index) {
    //   this.editor = false
    // },
    // applyTitle(index) {
    //   this.editor = !this.editor
    // },

    addNote() {
      let { title, descr, priority } = this.note;

      if (title === "") {
        this.message = "title can't be blank";
        return false;
      }
      this.notes.push({
        title,
        descr,
        date: new Date(Date.now()).toLocaleString(),
        priority: this.priority,
      });
      this.note.title = "";
      this.note.descr = "";
      this.priority = "norm";
      this.message = null;
    },
    removeNote(index) {
      this.notes.splice(index, 1);
    },
    getPriority(index) {
      this.priority = this.priorities[index];
      console.log(this.priority);
    },
  },
};
</script>

<style></style>
