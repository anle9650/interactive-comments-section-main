<template>
  <CommentCard
    v-for="comment in comments"
    :key="comment.id"
    :comment="comment"
    :nextCommentId="nextCommentId"
    :currentUser="user"
    class="mb-3"
    @delete-comment="comment => toDelete = comment"
  />
  <CommentForm :user="user" @comment-submit="postComment" />
  <DeleteModal @confirm-delete="deleteComment" />

  <div class="attribution mt-5">
    Challenge by
    <a href="https://www.frontendmentor.io?ref=challenge" target="_blank"
      >Frontend Mentor</a
    >. Coded by <a href="#">Andy Le</a>.
  </div>
</template>

<script>
import userData from "./data.json";
import CommentCard from "./components/CommentCard.vue";
import CommentForm from "./components/CommentForm.vue";
import DeleteModal from "./components/DeleteModal.vue";

export default {
  name: "App",
  components: {
    CommentCard,
    CommentForm,
    DeleteModal
  },
  data() {
    return {
      user: userData.currentUser,
      comments: userData.comments,
      toDelete: {},
    };
  },
  methods: {
    postComment(comment) {
      this.comments.push({
        id: this.nextCommentId,
        content: comment,
        createdAt: "just now",
        score: 0,
        user: { ...this.user },
        replies: [],
      });
    },
    deleteComment() {
      this._deleteCommentHelper(this.comments);
    },
    _deleteCommentHelper(comments) {
      comments.forEach((comment, index) => {
        if (comment.id === this.toDelete.id) {
          comments.splice(index, 1);
          return true;
        }
        if (comment.replies && this._deleteCommentHelper(comment.replies)) {
          return true;
        }
      });
      return false;
    },
  },
  computed: {
    nextCommentId() {
      const numReplies = this.comments.reduce(
        (total, comment) => (total += comment.replies.length),
        0
      );
      return this.comments.length + numReplies;
    }
  }
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Rubik:wght@400;500;700&display=swap");

body {
  font-size: 16px;
  font-family: "Rubik", sans-serif;
  background-color: hsl(228, 33%, 97%);
}

.card {
  border-radius: 8px;
}

img.avatar {
  width: 2em;
}

.bg-primary {
  background-color: hsl(238, 40%, 52%) !important;
}
.bg-light {
  color: hsl(238, 40%, 52%);
}

.btn {
  color: hsl(238, 40%, 52%);
  font-weight: 500;
  border-radius: 5px;
}
.btn-primary {
  background-color: hsl(238, 40%, 52%) !important;
  color: white;
  border-color: hsl(238, 40%, 52%);
}
.btn-secondary {
  color: white;
}
.btn-danger {
  color: white;
  background-color: hsl(358, 79%, 66%);
}

.modal-content {
    border-radius: 10px;
    border: none;
}
.modal .btn {
  border-radius: 10px;
}

.attribution {
  font-size: 11px;
  text-align: center;
}
.attribution a {
  color: hsl(228, 45%, 44%);
}
</style>
