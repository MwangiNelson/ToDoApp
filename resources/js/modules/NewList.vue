<template>
    <!-- This is the newlist module -->
    <!-- When a user wants to add a new list, this is displayed -->
    <div
        class="wrapper container position-fixed d-flex justify-content-center align-items-center"
    >
        <div class="w-50 d-flex container bg-dark rounded flex-column p-3">
            <div
                class="w-100 d-flex justify-content-between align-items-end border-bottom-secondary"
            >
                <h1 class="text-secondary m-0 p-0">ADD NEW LIST</h1>

                <!-- Triggers the toggle method -->
                <button
                    class="btn btn-outline-danger"
                    @click="toggleVisibility"
                >
                    <i class="fa-solid fa-xmark"></i>
                </button>
            </div>
            <hr class="seperator w-100" />
            <form class="">
                <div
                    class="form-group mb-3 d-flex flex-column justify-content-start"
                >
                    <div class="w-100 d-flex justify-content-between">
                        <!-- Binding the input data to a variable that I can work with using v-model -->
                        <input
                            type="text"
                            v-model="title"
                            class="form-control w-75"
                            placeholder="List title:"
                        />

                        <!-- Mthd that's called when submit -->
                        <button
                            class="btn btn-primary"
                            @click.prevent="handleSubmit"
                        >
                            ADD ITEM
                            <i class="fa-solid fa-paper-plane ps-3"></i>
                        </button>
                    </div>

                    <small class="form-text text-muted text-secondary"
                        >Please enter your list title here</small
                    >
                </div>
            </form>
        </div>
    </div>
</template>

<script>
import axios from "axios";
//importing a notification library
import Toastify from "toastify-js";
import "toastify-js/src/toastify.css";

export default {
    //initialising app variables
    data() {
        return {
            title: "",
        };
    },

    //initialising props
    props: {
        visibleProp: Boolean,
    },
    methods: {
        toggleVisibility() {
            this.$emit("update:visibleMethod", !this.visibleProp);
        },
        handleSubmit() {
            //    Empty validation
            if (this.title === "") {
                Toastify({
                    text: "Please enter a title for your list!  ",
                    duration: 2000,
                    newWindow: true,
                    close: true,
                    gravity: "top", // bottom-right, bottom-left, top-right, top-left
                    position: "center", // left, right, center
                    backgroundColor: "#ffc107",
                    stopOnFocus: false, // Prevents dismissing of toast on hover
                }).showToast();
                return;
            }
            // handling API posting
            axios
                .post("/api/lists", { title: this.title })
                .then((response) => {
                    //handling a successful insertion
                    Toastify({
                        text: `${response.data.data}    `,
                        duration: 2000,
                        newWindow: true,
                        close: true,
                        gravity: "top", // bottom-right, bottom-left, top-right, top-left
                        position: "center", // left, right, center
                        backgroundColor: "#ffc107",
                        stopOnFocus: false, // Prevents dismissing of toast on hover
                    }).showToast();

                    //triggers a listUpdated hook that triggers the method that'll be passed as an arg
                    this.$emit("listUpdated");
                    //Triggers another hook that toggles the visibility of the new List Module
                    this.$emit("update:visibleMethod", !this.visibleProp);
                })
                .catch(console.error());
        },
    },
};
</script>

<style lang="scss" scoped>
.seperator {
    color: #fff;
}
.wrapper {
    width: 100vw;
    height: 100vh;
    backdrop-filter: blur(3px);
}
</style>
