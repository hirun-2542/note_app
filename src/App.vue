<script setup>
import { ref } from "vue";

const notes = ref(JSON.parse(localStorage.getItem("notes")) || []);

const addtitle = ref("");
const adddetail = ref("");
const noteId = ref("");

const selectedContent = (detail) => {
    noteId.value = detail.id;
    addtitle.value = detail.title;
    adddetail.value = detail.detail;
};

const AddNote = () => {
    if (addtitle.value !== "" || adddetail.value !== "") {
        const note = {
            id: notes.value.length,
            title: addtitle.value,
            detail: adddetail.value,
            date: new Date().toLocaleDateString("en-us", {
                day: "numeric",
                year: "numeric",
                month: "short",
            }),
        };
        notes.value.push(note);
        localStorage.setItem("notes", JSON.stringify(notes.value));
        addtitle.value = "";
        adddetail.value = "";
        noteId.value = null;
    }
};

const EditNote = (title, desc) => {
    const note = {
        id: noteId.value,
        title: title,
        detail: desc,
        date: new Date().toLocaleDateString("en-us", {
            day: "numeric",
            year: "numeric",
            month: "short",
        }),
        modal: "false",
    };
    notes.value[noteId.value] = note;
    localStorage.setItem("notes", JSON.stringify(notes.value));
    addtitle.value = "";
    adddetail.value = "";
    modal.value = false;
};

const DeteleNote = (index) => {
    notes.value.splice(index, 1);
    localStorage.setItem("notes", JSON.stringify(notes.value));
    modal.value = false;
};

const showcontent = "setting show";
const hidecontent = "setting";
const indexmenu = ref(null);
const modal = ref(false);

const showmenu = (index) => {
    notes.value.forEach((note) => {
        if (note.id === index) {
            indexmenu.value = index;
            modal.value = !modal.value;
        }
    });
};

const reset = () => {
    addtitle.value = "";
    adddetail.value = "";
    noteId.value = null;
};
</script>

<template>
    <div class="container w-max">
        <div class="notes">
            <div class="add-box">
                <a href="#my-modal-2" @click="reset">
                    <svg
                        xmlns="http://www.w3.org/2000/svg"
                        fill="none"
                        viewBox="0 0 24 24"
                        stroke-width="1.5"
                        stroke="currentColor"
                        class="image-add cursor-pointer"
                    >
                        <path
                            stroke-linecap="round"
                            stroke-linejoin="round"
                            d="M12 9v6m3-3H9m12 0a9 9 0 11-18 0 9 9 0 0118 0z"
                        />
                    </svg>
                </a>
                <div class="modal" id="my-modal-2">
                    <div class="modal-box bg-white">
                        <input
                            class="bg-white font-bold text-lg pb-2 border-b-2 w-11/12"
                            type="text"
                            v-model="addtitle"
                            placeholder="Title"
                        />
                        <a
                            @click="reset"
                            href="#"
                            class="btn btn-sm absolute right-2 top-2 bg-white border-0 text-black hover:bg-white"
                            >✕</a
                        >

                        <p class="py-4">
                            <textarea
                                class="bg-white"
                                v-model="adddetail"
                                name="noteapp"
                                rows="4"
                                cols="40"
                                placeholder="Write your note here..."
                            />
                        </p>

                        <div class="modal-action mt-0">
                            <a
                                href="#"
                                class="btn bg-blue-500 text-white border-0 hover:bg-blue-700"
                                @click="AddNote"
                                >Add Note</a
                            >
                        </div>
                    </div>
                </div>

                <p id="add-note">Add new note</p>
            </div>
        </div>
        <div class="notes" v-for="(note, index) in notes" :key="index">
            <ul class="list-note">
                <li class="title-note">{{ note.title }}</li>
                <li class="detail-note">
                    {{ note.detail }}
                </li>
                <div class="button-content">
                    <span class="date-note">{{ note.date }}</span>
                    <div
                        :class="[
                            indexmenu === index && modal
                                ? showcontent
                                : hidecontent,
                        ]"
                    >
                        <div @click="showmenu(index)" class="more-note">
                            ...
                        </div>
                        <ul class="menu-popup">
                            <label
                                for="my-modal-3"
                                @click="selectedContent(note)"
                            >
                                <li class="hover:bg-slate-300">Edit</li></label
                            >

                            <li
                                @click="DeteleNote(index)"
                                class="hover:bg-slate-300"
                            >
                                Delete
                            </li>
                        </ul>

                        <input
                            type="checkbox"
                            id="my-modal-3"
                            class="modal-toggle"
                        />
                        <div class="modal">
                            <div class="modal-box relative bg-white">
                                <label
                                    for="my-modal-3"
                                    class="btn btn-sm absolute right-2 top-2 bg-white border-0 text-black hover:bg-white"
                                    >✕</label
                                >
                                <input
                                    class="bg-white font-bold text-lg pb-2 border-b-2 w-11/12"
                                    type="text"
                                    v-model="addtitle"
                                    placeholder="Title"
                                />
                                <p class="py-4">
                                    <textarea
                                        class="bg-white"
                                        v-model="adddetail"
                                        name="noteapp"
                                        rows="4"
                                        cols="40"
                                        placeholder="Write your note here..."
                                    />
                                </p>
                                <div class="modal-action mt-0">
                                    <label
                                        @click="EditNote(addtitle, adddetail)"
                                        for="my-modal-3"
                                        class="btn bg-blue-500 text-white border-0 hover:bg-blue-700"
                                        >Edit Note</label
                                    >
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </ul>
        </div>
    </div>
</template>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Prompt:ital,wght@0,500;1,200&display=swap");
.container {
    font-family: "Prompt", sans-serif;
    color: black;
    display: grid;
    grid-template-columns: repeat(auto-fill, 250px);
    gap: 20px;
    margin: 50px 0 0 50px;
}

.add-box {
    text-align: -webkit-center;
}

#add-note {
    color: #6a90f3;
}

.image-add {
    width: 100px;
    height: 100px;
    margin-top: 50px;
}

.notes {
    position: relative;
    width: 250px;
    height: 250px;
    background-color: #f5f5f5;
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
    border-radius: 5px;
}

.list-note {
    list-style: none;
    padding: 0;
    margin: 20px 0;
}

.title-note {
    font-size: 20px;
    font-weight: bold;
    padding: 0 20px 0;
}
.detail-note {
    word-break: break-all;
    padding: 15px 20px 20px;
}

.date-note {
    font-size: 12px;
    padding: 0 20px 0;
}
.more-note {
    cursor: pointer;
    font-size: 22px;
    font-weight: 500;
    padding: 0 20px 0;
}

.button-content {
    display: flex;
    justify-content: space-between;
    align-items: center;
    border-top: solid 1px #e0e0e0;
    position: absolute;
    bottom: 0;
    width: 100%;
    margin-bottom: 10px;
}

.setting {
    position: relative;
}

.setting .menu-popup {
    transform: scale(0);
    transition: transform 0.2s ease;
}
.setting.show .menu-popup {
    transform: scale(1);
}
.menu-popup {
    margin: 30px 0;
    position: absolute;
    bottom: 0;
    background-color: #f1f1f1;
    min-width: 80px;
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
    border-radius: 5px;
    z-index: 1;
    padding: 0;
}

.menu-popup li {
    color: black;
    cursor: pointer;
    display: block;
    padding: 5px 20px;
}
</style>
