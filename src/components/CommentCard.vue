<template>
  <div>
    <div class="card p-lg-2 border-0">
      <div class="card-body d-lg-flex align-items-start">
        <div
          class="
            badge
            rounded-pill
            bg-light
            d-none d-lg-flex
            flex-column
            p-0
            me-4
          "
        >
          <button class="btn btn-light" @click="score++">
            <img src="../assets/images/icon-plus.svg" alt="icon-plus" />
          </button>
          {{ score }}
          <button class="btn btn-light" @click="score--">
            <img src="../assets/images/icon-minus.svg" alt="icon-minus" />
          </button>
        </div>
        <div class="mb-3 mb-lg-0">
          <div class="d-lg-flex align-items-center">
            <div class="me-auto">
              <img
                class="avatar me-3"
                :src="
                  require('../assets/images/avatars/' + comment.user.image.png)
                "
                alt="avatar"
              />
              <h6 class="card-title d-inline">
                {{ comment.user?.username ?? "" }}
              </h6>
              <span
                v-if="isUserComment"
                class="badge badge-user bg-primary align-self-start ms-2"
                >you</span
              >
              <span class="card-subtitle text-muted ms-3">{{
                comment.createdAt ?? ""
              }}</span>
            </div>
            <div class="d-none d-lg-block">
              <button
                :class="['btn p-0 me-2', isUserComment ? 'btn-delete' : '']"
                @click="
                  isUserComment
                    ? $emit('delete-comment', comment)
                    : (showReplyForm = true)
                "
                v-bind="
                  isUserComment
                    ? {
                        'data-bs-toggle': 'modal',
                        'data-bs-target': '#deleteModal',
                      }
                    : {}
                "
              >
                <img
                  :src="
                    require(`../assets/images/icon-${
                      isUserComment ? 'delete' : 'reply'
                    }.svg`)
                  "
                  :alt="isUserComment ? 'icon-delete' : 'icon-reply'"
                  class="me-1"
                />
                {{ isUserComment ? "Delete" : "Reply" }}
              </button>
              <button
                v-if="isUserComment"
                class="btn p-0"
                @click="showEditForm = true"
              >
                <img src="../assets/images/icon-edit.svg" alt="" class="me-1" />
                Edit
              </button>
            </div>
          </div>
          <p class="card-text text-muted mt-3">
            <a class="text-mention" href="#">{{
              comment.replyingTo ? `@${comment.replyingTo}` : ""
            }}</a>
            {{ content }}
          </p>
        </div>
        <div class="d-flex d-lg-none">
          <span class="badge rounded-pill bg-light p-0 me-auto">
            <button class="btn btn-light" @click="score++">
              <img src="../assets/images/icon-plus.svg" alt="icon-plus" />
            </button>
            {{ score }}
            <button class="btn btn-light" @click="score--">
              <img src="../assets/images/icon-minus.svg" alt="icon-minus" />
            </button>
          </span>
          <button
            :class="['btn p-0 me-2', isUserComment ? 'btn-delete' : '']"
            @click="
              isUserComment
                ? $emit('delete-comment', comment)
                : (showReplyForm = true)
            "
            v-bind="
              isUserComment
                ? {
                    'data-bs-toggle': 'modal',
                    'data-bs-target': '#deleteModal',
                  }
                : {}
            "
          >
            <img
              :src="
                require(`../assets/images/icon-${
                  isUserComment ? 'delete' : 'reply'
                }.svg`)
              "
              :alt="isUserComment ? 'icon-delete' : 'icon-reply'"
              class="me-1"
            />
            {{ isUserComment ? "Delete" : "Reply" }}
          </button>
          <button
            v-if="isUserComment"
            class="btn p-0"
            @click="showEditForm = true"
          >
            <img src="../assets/images/icon-edit.svg" alt="" class="me-1" />
            Edit
          </button>
        </div>
      </div>
    </div>
    <div class="border-start mt-3">
      <CommentForm
        v-show="showReplyForm"
        :user="currentUser"
        class="ms-3 mb-3"
        @comment-submit="postReply"
      />
      <CommentForm
        v-show="showEditForm"
        :user="currentUser"
        :content="content"
        class="ms-3 mb-3"
        @comment-submit="editComment"
      />
      <CommentCard
        v-for="reply in replies"
        :key="reply.id"
        :comment="reply"
        :currentUser="currentUser"
        class="ms-3 mb-3"
        @delete-comment="(comment) => $emit('delete-comment', comment)"
      />
    </div>
  </div>
</template>

<script>
import CommentForm from "./CommentForm.vue";

export default {
  name: "CommentCard",
  components: {
    CommentForm,
  },
  props: {
    comment: {
      type: Object,
      required: true,
    },
    nextCommentId: Number,
    currentUser: {
      type: Object,
      required: true,
    },
  },
  emits: ["delete-comment"],
  data() {
    return {
      content: this.comment.content ?? "",
      score: this.comment.score ?? 0,
      replies: this.comment.replies ?? [],
      showReplyForm: false,
      showEditForm: false,
    };
  },
  methods: {
    postReply(reply) {
      this.replies.push({
        id: this.nextCommentId,
        content: reply,
        createdAt: "just now",
        score: 0,
        replyingTo: this.comment.user.username,
        user: { ...this.currentUser },
        replies: [],
      });
      this.showReplyForm = false;
    },
    editComment(edit) {
      this.content = edit;
      this.showEditForm = false;
    },
  },
  computed: {
    isUserComment() {
      return this.comment.user?.username === this.currentUser.username;
    },
  },
};
</script>

<style scoped>
.text-mention {
  color: hsl(238, 40%, 52%);
  font-weight: 500;
  text-decoration: none;
}

.badge-user {
  font-weight: 500;
}

.badge.bg-light {
  font-size: 1em;
  font-weight: 500;
}

.badge .btn {
  border-radius: 10px;
}

.btn-delete {
  color: hsl(358, 79%, 66%);
}
</style>
