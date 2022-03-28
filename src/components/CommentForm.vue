<template>
  <div class="card p-lg-2 border-0">
    <div class="card-body">
      <form @submit.prevent="onSubmit">
        <textarea
          class="form-control d-lg-none px-3 py-2 mb-3"
          rows="3"
          placeholder="Add a comment..."
          v-model="comment"
        ></textarea>
        <div class="d-flex align-items-center align-items-lg-start">
          <img
            class="avatar me-auto me-lg-3 mt-lg-1"
            :src="require('../assets/images/avatars/' + user.image.png)"
            alt="avatar"
          />
          <textarea
            class="form-control d-none d-lg-block px-3 py-2 me-3"
            rows="3"
            placeholder="Add a comment..."
            v-model="comment"
          ></textarea>
          <button type="submit" class="btn btn-primary px-4 py-2">SEND</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  name: "CommentForm",
  props: {
    user: {
      type: Object,
      required: true,
    },
    content: {
      type: String,
      default: "",
    },
  },
  emits: ["comment-submit"],
  data() {
    return {
      comment: this.content,
    };
  },
  methods: {
    onSubmit() {
      this.$emit("comment-submit", this.comment);
      this.comment = "";
    },
  },
};
</script>

<style scoped>
textarea {
  border-radius: 8px !important;
  resize: none;
}

@media screen and (min-width: 992px) {
    img.avatar {
        width: 2.5em;
    }
}
</style>